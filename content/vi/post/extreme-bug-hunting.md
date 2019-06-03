+++
title = "Nào thì đi săn bug: phần 1"
date = 2019-06-03
draft = true
authors = ["Giang 'Yakiro' Nguyen"]

tags = ["bug-hunting", "fundamental"]
summary = "Debug qua đường email và cái kết"
+++

Series này để đổi gió bằng những pha debug hay (hoặc hài).

Làm việc tại một công ty ao-sọt đôi khi có những trải nghiệm phải nói là vãi đái. Cái
nghề mà đôi khi khiến mình thấy bản thân dễ tính như phò vậy. Tuy nhiên tư duy tích cực
thì nó cũng có chỗ sướng riêng.

# Những cú mail lúc nửa đêm

Tôi luôn đặt điện thoại ở chế độ "do not disturb", chủ yếu là các thể loại thông báo bây
giờ rất là bất lịch sự. Trừ khi bạn là Elon Musk với khả năng multitasking thần thánh 5
phút switch context một phát. Tuy vậy, khách hàng có vẻ không biết điều này nên vẫn hồn
nhiên bắn mail vào lúc 2h sáng.

Chả là các bạn có con bug được đánh giá là rất khoai, block đã nhiều tháng và có nguy cơ
ảnh hưởng đến việc release. Vì chuyện này mà bạn huy động nguồn lực thức đêm thức hôm, đi
làm cuối tuần các kiểu. Nhật mà, tinh thần trách nhiệm thì khỏi nói. Có bệnh thì vái tứ
phương, khó quá nên thôi nhờ vendor fix hộ. Chuyện thì cũng chả có gì cho đến khi ta nhận
ra đây là khách hàng mới, chưa ký NDA.

Tức là?

`No source code, no environment.`

Tư duy theo lối thông thường mà nói thì dẹp luôn cho nhanh. Tuy vậy, vì bạn là khách, mà
khách đến nhà thì mình phải tiếp, chiều được bạn thì mới có hợp đồng, mới có cơm mà ăn nên
thôi ta cứ coi như đang tham gia chương trình thách thức danh hài vậy. Càng khó, càng nhảm
thì lại càng dễ chứng minh năng lực mà.

# Ta có gì trong tay?

{{< figure src="/img/extreme_bug_hunting/what-we-have.png" title="Nó chết chỗ này nè (dòng 198)" >}}

Dòng 198 ở đây là cả cái function à?

{{< figure src="/img/extreme_bug_hunting/10points.gif" >}}

Bị crash ⇒ xin thêm crash dump về đọc thì thấy.

`signal 11 (Segmentation fault), fault addr 0x0000000c`

Địa chỉ `0xC` là quá thấp nên có thể hiểu lỗi ở đây là do truy cập vào `NULL pointer`. Nói
thêm một chút chỗ này vì độc giả của blog có vẻ có nhiều newbie. `NULL` thường được hiểu là
bằng `0x0`, theo quy ước này thì OS sẽ không bao giờ cấp phát một vùng nhớ với địa chỉ `0x0`.
Vì vậy ta có thể dùng giá trị này để đánh dấu một pointer không hợp lệ, không trỏ đến cái gì
hoặc đã bị free. Việc truy cập vào địa chỉ `0x0` này sẽ dẫn đến crash chương trình.

**Tuy nhiên địa chỉ được báo lỗi thường sẽ không phải 0x0.**

Việc bắt được cái lỗi truy cập này do CPU thực hiện, tức là nó ở mức assembly. Cách check thì
tuỳ loại CPU nhưng về cơ bản là ta đánh dấu một số vùng trong address space là hợp lệ, chạy
ra ngoài là đứt. Nó hoàn toàn không hiểu gì về C/C++ hay những cấu trúc phức tạp. Thực tế hiếm
khi ta truy cập vào địa chỉ `0x0`, nhất là khi nó trỏ đến class hoặc struct. Khi viết `p->member`
thì compiler sẽ gen ra assembly để truy cập đến địa chỉ `p + member_offset`. Và khi crash thì
cái địa chỉ được báo sẽ là địa chỉ thực tế. Trừ khi debugger đủ thông minh để diễn dịch lại
thành `NULL` còn không thì cứ thấy address thấp khoảng vài chục đến vài trăm bytes tuỳ class
thì cứ hiểu nó là `NULL`.

Ok, lan man một chút. Quay về câu chuyện thì trong các thể loại bug liên quan đến memory thì
`NULL` là thể loại phải nói là dễ thở nhất. Với các vùng nhớ khác `NULL` thì có vô số khả năng
xảy ra. Chứ với `NULL` thì đơn giản là ông code không chịu check đã truy cập rồi, phải không?

# Cú lừa của compiler

Bởi vì ta không có source cho nên việc phán đoán chương trình chết cụ thể ở dòng nào hơi chuối
tý. Lội crash dump rồi xem mấy cái biến được pass vào ra sao, có cái gì `NULL` không. Do thiếu
debug symbol nên ta không thể print trực tiếp giá trị các biến bằng tên như `index` hay `this`
được. Thay vào đó ta lôi manual của ARM check xem theo calling convention thì mấy cái biến đấy
nó đưa cho function qua đường nào. May quá, ít tham số nên nó vứt hết vào register, check phát
thì thấy `this` đang là `NULL` luôn. Xời ơi, dễ vãi loz.

Và code thì nó đang như thế này.

{{< figure src="/img/extreme_bug_hunting/what-we-have.png" title="`this` bằng NULL thì!?" >}}

Việc check `!this` có vẻ không hay, tuy nhiên từ góc độ low-level mà nói thì code như trên sẽ
return luôn chứ không crash. Bởi vì `this` cũng chỉ là một đối số được truyền ngầm định. Nếu ta
không đụng vào nó (dereference) thì chưa chết được. Test `!this` chưa phải là dereference. Tức
là có gì đó rất hip-hop ở đây.

Với những lập trình viên có kinh nghiệm, việc code một đằng chạy một nẻo là quá bình thường.
Kinh nghiệm ở đây là high-level không thấy bug thì phải xuống thấp hơn cho đến khi nào ra vấn
đề. Càng low-level thì khả năng ăn quả lừa càng thấp. Đấy cũng là lý do vì sao 99.99% thời gian
bạn toàn code .Net, Javascript nhưng vẫn phải học mấy món như C/C++, ASM.

**Code nó dell chạy thì còn biết đường mà fix thôi.**

# Nào thì mình disassembly

Về cơ bản là assembly khá là đơn giản, rất ít concept / obstacle cho nên hiếm khi nó lừa mình.
Nếu nghi ngờ compiler nó làm trò gì đó hip-hop thì tốt nhất cứ disassembly ra mà đọc xem nó làm
gì là chắc cú nhất. May quá khách không gửi code nhưng có crash dump thì vẫn xem disassembly được.
Ở đây xin phép không trình bày cụ thể để bài viết ngắn gọn. Tóm lại kết quả chúng ta thấy là.

**Cái đoạn `!this` nó biến mất như chưa từng xuất hiện.**

Đây là dấu hiệu của [dead code elimination](https://en.wikipedia.org/wiki/Dead_code_elimination).

Đại loại là khi compiler nó phát hiện ra những đoạn code không thể reach đến được thì nó sẽ tối
ưu bằng cách coi như không có luôn. Ví dụ kiểu `if (1 > 2)` hay `while (1 + 1 == 3)`. Đến đây ta
cần điều tra tiếp tại sao `!this` lại được compiler hiểu là `always false`.

---

Đi từ C++ standard, ta phát hiện được một điểm là C++ nó yêu cầu `this` không bao giờ được phép
`NULL`. Tức là khi ta đang ở trong method, ta luôn có thể truy cập đến nó mà không cần tư duy là
liệu nó có `NULL` hay không. Tiện thể, chả hiểu sao các boss không cho `this` thành reference
luôn mà lại đẻ ra cái thể loại *pointer never be `NULL`* này confuse vãi đái.

Bởi vì `this` không thể `NULL`, cho nên mặc dù `this` không phải là constant, nhưng `!this` lại là
constant, và giá trị của biểu thức này được biết ngay từ lúc compile là `false`. Cái trò biến biểu
thức thành constant này trong lý thuyết compiler nó gọi là [constant folding](https://en.wikipedia.org/wiki/Constant_folding). Kiểu `1 + 1` thì lúc dell nào chả bằng `2`, tính luôn cho nó vui. Mà tiện đã fold `!this` thành
`false` rồi thì `if (false)` cho đi luôn chứ để làm gì.

# Chứng minh

Sau khi "có vẻ" đã tìm được root cause, ta cần tái hiện và tìm counter evidence cũng như gợi ý
cách sửa. Dựa trên luận điểm `!this` là constant, nếu ta check kiểu khác thì compiler sẽ không
optimize được nữa và chương trình sẽ chạy như mong muốn. Tuy nhiên để fix triệt để lỗi này thì code
cần theo C++ standard, tức là phải check `NULL` trước khi gọi method chứ không phải vào method rồi
lại đi check `!this`.

Phát đầu tiên.

```C++
bool not(const void *p) {
  return !p;
}

if (not(this)) {
  return 0;
}
```

Kết quả là không chạy, có vẻ compiler nó "khôn" hơn ta tưởng.

---

```C++
if ((size_t)this != 0) {
  return 0;
}
```

Vẫn không chạy, chắc do nó vẫn nhìn thấy code.

---

```C++
if (this == sin(0)) {
  return 0;
}
```

{{< figure src="/img/extreme_bug_hunting/ffuclear.gif" title="Trên lý thuyết, `sin` là pure function, và `sin(0)` là constant" >}}

---

Ultima weapon.

```C++
if (this == fopen("this should returns 0")) {
  return 0;
}
```

Cuối cùng cũng ăn, vì `fopen` return cái gì phải chạy mới biết được.

---

Btw, cái này cũng ok.

```C++
if (this == atoi("0")) {
  return 0;
}
```

Thực ra `atoi` cũng là pure function, nếu compiler đủ khôn vẫn fold dc.

---

Hoặc là như thế này.

```C++
void * NULL_POINTER_VAR = NULL;
if (this == NULL_POINTER_VAR) {
  return 0;
}
```

Vì đây là `variable`, tức là trên lý thuyết nó có thể bị thay đổi khi chương trình chạy,
không phải constant nên compiler không có cơ sở để optimize.