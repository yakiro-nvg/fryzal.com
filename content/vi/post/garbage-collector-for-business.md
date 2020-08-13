+++
title = "Lập trình hướng nhặt rác"
date = 2020-08-13
draft = false
authors = ["Nguyen, Giang (G. Yakiro)"]

tags = ["architecture", "COBOL"]
summary = "Thiết kế người nhặt rác trong môi trường doanh nghiệp"
+++

Câu chuyện bắt đầu từ ngày xửa ngày xưa, mấy cái hệ thống ERP, bảo hiểm, tài chính
được viết bằng COBOL và chạy trên mainframe. Bây giờ thì? Phần lớn vẫn thế nốt. Một
số đã thoát khỏi "cái máng lợn" này, bằng cách convert sang Java và lăn ra chết,
công ty phá sản. Số khác thì ITX thành công và vào fortune 500, số còn lại thì cũng
ITX thành công và vào fortune under 500. Hiện tại thì có khoảng 240 tỷ dòng code COBOL
vẫn đang chạy, và mỗi năm thêm cỡ 5 tỷ dòng. 

{{< figure src="/img/garbage_collector_for_business/age-of-cobol-programmer.png" >}}

Xu hướng chuyển từ COBOL sang những ngôn ngữ mới hơn đã có từ lâu, phần vì không có
nhân lực làm COBOL (toàn mấy ông già sắp về hưu). Phần vì người ta nghĩ COBOL đã cũ rồi,
sắp chết tới nơi (người ta cũng bảo thế với C++, Java etc.). Tuy nhiên thì [Mỹ cần lập
trình viên COBOL để chiến đấu với coronavirus](https://www.techrepublic.com/article/cobol-programmers-are-in-demand-to-fight-the-coronavirus-pandemic/)
cho thấy ngày anh ra đi vẫn còn xa.

> Một số report đánh giá 240 tỷ dòng code COBOL là tài sản có giá trị đứng thứ hai của Mỹ,
> xếp ngay sau dầu mỏ.

# Tại sao nên dùng COBOL?

Thực lòng mà nói, ý tưởng convert từ COBOL sang Java hay thứ gì đó "hiện đại" hơn khá
tào lao. Có nhiều luận điểm cả từ góc độ kỹ thuật lẫn chiến thuật cho vấn đề này mà tôi
xin phép được đề cập trong một bài viết khác. Tuy nhiên kết luận ở đây là COBOL rất ngon
trong bài toán xử lý nghiệp vụ, cũng là bài toán nó được thiết kế để giải quyết (tốt hơn
nhiều so với Java, JEE các kiểu).

{{< figure src="/img/garbage_collector_for_business/trump_cics.png" >}}

## Dùng như thế nào?

COBOL hay, nhưng ta nên mua quả chuối, đừng mua cả con khỉ. Tôi thiên hướng dùng COBOL
như một loại domain specific language (DSL), tương tự như cách mà SQL chuyên để truy vấn
cơ sở dữ liệu, hay QML để xây dựng giao diện. Để COBOL hữu dụng ta cần kết hợp nó với
những công nghệ hiện đại. Điều này đạt được thông qua khả năng "nhúng" tương tự như Lua.

Một case study kiểu như sau. Giả dụ bạn vừa đi hội thảo Outsystems của Fpt, và bạn nghĩ
cái này cool đấy, rất hợp cho một số feature bạn sắp triển khai. Nhưng hệ thống cũ đang
chạy COBOL thì liên quan gì nhỉ? Hay convert sang Javascript? Đừng! cái gì đang chạy ngon,
debug chán rồi thì đừng viết lại. Ném tiền qua cửa sổ đấy. Thay vào đó hãy kết hợp, phần
nghiệp vụ những gì liên quan đến hệ thống cũ, cứ viết bằng COBOL tiếp.

{{< figure src="/img/garbage_collector_for_business/cobol_dsl.png" title="COBOL as domain specific language" >}}

---

Tuy nhiên vấn đề phát sinh là trên thị trường hiện tại không có giải pháp nào để nhúng
COBOL vào một hệ thống khác như vậy cả. Vì thế Fpt sắp có giải pháp :) và bài viết này
tôi sẽ chia sẻ về một thách thức trong quá trình thiết kế giải pháp này.

# Chuyện bắt đầu từ async

Giả dụ ta có cái program để lấy số dư tài khoản như sau:

```CBL
program-id. Lấy-Số-Dư
    data division                             *> khai báo các biến
        linkage section                       *> khu vực truyền / nhận
            01 Số-Tài-Khoản pic 9(12)         *> 12 digits 
            01 Số-Dư        pic 9(18)V9(3)    *> 21 digits, 3 scale digits
    
    procedure division using     Số-Tài-Khoản *> tham số nhận
                       returning Số-Dư        *> giá trị trả về
        exec sql                              *> inline SQL, như LinQ
            select Balance into :Số-Dư        *> lấy vào biến :Số-Dư
                from Account                  *> từ bảng Account
                where Id = :Số-Tài-Khoản      *> với điều kiện
        end-exec
end-program Lấy-Số-Dư
```

Hy vọng trông nó có vẻ dễ hiểu (mà không hiểu thì cũng ok luôn). COBOL hỗ trợ inline SQL,
query với tham số từ biến và trả giá trị thẳng vào biến như trên. Tuy nhiên đó không phải
cái tôi muốn nói ở đây (mặc dù nó khá là cool). Vấn đề là khi câu lệnh trên được thực thi
thì hàm Lấy-Số-Dư sẽ bị block để đợi kết quả trả về từ SQL driver.

Với giải pháp dạng standalone như kiểu Micro Focus thì không sao, mỗi request có thể được
phục vụ bởi một thread cho nên có block thì cũng chả chết ai. Mà thực ra có chết, chết thẳng
cẳng luôn nếu nhiều request là khác. Tôi cũng chưa thấy anh Micro Focus giải quyết vấn đề này
kiểu gì.

Đến giờ tôi vẫn chưa thấy non-blocking I/O của anh ý đâu cả.

## Vấn đề nếu không phải standalone?

Vì giải pháp của ta là để nhúng, ví dụ như nhúng vào NodeJS đi. Thì việc gọi cái hàm trên từ
NodeJS sẽ block luôn cả chương trình, bởi vì NodeJS nó dùng có mỗi một thread :) Tức là ta phải
thiết kế kiểu multi-process. Mỗi request là ta phải tạo một process mới, GUI nếu có (Electron, React-Native etc.) cũng cần một process riêng nốt. Mà tôi tin là giải pháp kiểu này dek chạy.

Mỗi request một thread còn chết nữa là mỗi thằng một NodeJS process.

## Giả dụ ta khôn hơn, có async

Trường hợp này cần non-blocking I/O. Thì vấn đề là hai thằng "async" với "pass-by-reference"
nó không chơi với nhau. Để ví dụ đơn giản thì chỗ này tôi xin phép code bằng C++ cho nó thân quen.

```C++
std::function<int ()> f;

std::function<int ()> something_later(int &x) {
    return [&] {
        return x + 1;
    }
}

void set_f() {
    int x = 14;
    f = something_later(x);
} // x đã được giải phóng

set_f();
printf("%d\n", f());
```

Theo logic thông thường ta nghĩ nó in ra 15, nhưng với C++ thì nó không in ra 15 mà chết
luôn. Lý do là cái lambda trên mặc dù capture biến 'x', nhưng khi escape thì nó không mang
thằng x này theo cùng. Tức là khi print thì x đã chết theo cái scope ở trước.

Bạn không thể mang theo tài sản khi chết? C++ không nghĩ thế.

Tức là "pass-by-reference" không safe khi kết hợp với async. Nguyên lý thiết kế của C++ là
`ngu thì chết`, cho nên đây chưa phải vấn đề. Tuy nhiên với các ngôn ngữ safe như C# chẳng
hạn thì đây là câu chuyện hoàn toàn khác. C# nó cấm luôn trường hợp trên, khi ta mix chúng
với nhau thì compiler sẽ báo lỗi.

Java khôn nhất, không có pass-by-reference, khỏi lỗi.

COBOL của ta thì "quá đần", **pass-by-reference-by-default**.

## Đừng mang theo tài sản khi chết

Một số ngôn ngữ như Lua hay C# thì cho phép biến cấp phát trên stack được escape theo lambda
lên heap. Tuy nhiên biến ở đây là lexical scope, chứ không phải tham số được truyền cho function
dưới dạng reference. COBOL còn vãi hơn ở khoản callee không phân biệt (được) giữa tham số dạng 
by-reference và by-content. Tức là cùng một hàm thằng gọi thích truyền kiểu gì cũng được.

```CBL
call Something-Later using X.
call Something-Later using by content X.
```

Giải pháp là stacklets, mỗi stack frame là một object được cấp phát trên heap và quản lý bởi GC.
Cách làm này giống python, mặc dù python không có pass-by-reference (nhưng có generator). Điều này
giúp stack frame (chứa biến x) có thể vẫn còn sống ngay cả khi hàm set_f đã return.

# Implementation

Đầu tiên cái CAM (COBOL Abstract Machine) của tôi không quan tâm lắm về vụ garbage, version đầu
tiên dự định chơi kiểu chạy dc là xong, chậm cũng ok luôn. Tức là implementation càng naive càng tốt.
Thuật toán được sử dụng là semi-spaces copying, cụ thể hơn là [Cheney's algorithm](https://en.wikipedia.org/wiki/Cheney%27s_algorithm)
cho đỡ phải đệ quy.

{{< figure src="/img/garbage_collector_for_business/semispaces.png" title="Semispace tracing garbage collector" >}}

Object được cấp phát bằng cách bump pointer (theo chiều đi xuống). Khi heap đầy, ta thực hiện việc
nhặt "không phải rác" bằng cách scan từ rootset (stack) và copy tất cả những thằng vẫn còn sống (màu
đen) qua chỗ khác (to-space), rồi release toàn bộ from-space. Thực tế sẽ phức tạp hơn tý do ta cần
tính đến cả trường hợp object chúng nó tự refer lẫn nhau, tức là nó sẽ có 3 màu là đen (đã copy và
scan hết đám children), xám (đã copy, chưa scan) và trắng (chưa copy).

## Ưu điểm

* Naive, code xong trong một buổi chiều cả UT.
* Allocate object bằng pointer bump, nhanh như stack.
* Lúc copy thì tự compact, không bị fragment.
* Không cần quan tâm mấy thằng không alive, nhanh.

## Nhược điểm

* Tốn gấp đôi RAM, giả dụ app dùng 8GB, thì nó cần 16GB.
* Stop-the-world, tức là nó pause, càng nhiều thằng alive càng lâu.
  - Tăng latency, heap to có thể pause đến vài giây.
* Không thích chơi với interior pointer (vấn đề lớn).

## Interior pointer là cái gì?

Trong khi những nhược điểm trên chưa phải chí mạng, vì thực ra nó là non-functional, có thể cải
tiến sau thì interior pointer khiến tôi phải consider lại hoàn toàn về GC. Interior pointer là
pointer trỏ đến member của object thay vì object.

```C++
struct A
{
    int x;
    int y;
};

void f(int &y)
{
    // ...
}

A a;
f(a.y);
```

Đoạn code trên pass `a.y` by-reference. Trong khi ta cần retain a. Hiểu theo kiểu ref count với
smart pointer thì ta cần tăng ref count của a chứ không phải a.y. Tuy nhiên khi đã ở trong hàm f,
ta nhiều nhất thì cũng chỉ biết rằng y là interior pointer đến một vùng nhớ kiểu int (bằng [pointer tagging](https://en.wikipedia.org/wiki/Tagged_pointer)).

.Net cho phép interior pointer, và sử dụng một cơ chế khá phức tạp là [bricks and plugs tree](https://tooslowexception.com/managed-pointers-in-net/)
để scan ra nơi bắt đầu của object chứa vùng nhớ được interior pointer trỏ tới. Và vì quá chậm nên .Net 
không cho phép interior pointer nằm trên heap.

Quay về với bài toán pass-by-ref + async của chúng ta. Thì pointer đến một biến được cấp phát trên
stack chính là interior pointer mà object ở đây là stack frame. Từ pointer này muốn ra được cái frame
để retain là vấn đề không đơn giản.

Ngay cả khi không tính đến vụ escape biến từ stack lên heap, thì COBOL 2002 với dynamic allocation
cũng cần interior pointer luôn. Tức là ta có thể pass member của một instance by reference. Nếu không
retain được thì khi GC chạy instance có thể bị collect nhầm => dẹo.

Xin nhắc lại, Java khôn nhất, không có pass-by-reference, khỏi lỗi :)

## Go interior pointer

GC của Go thì dạng non-moving mark-and-sweep. Các object được cấp phát theo "size segregated span". Tức
là pool chứa nhiều object cùng kích thước. Như vậy từ interior pointer ta có thể tìm ra nó thuộc pool nào
và kích thước mỗi object là bao nhiêu (round down to pool-size -> pool header). Sau khi đã có kích thước
object ta tìm ra nơi object bắt đầu (round down to object size).

{{< figure src="/img/garbage_collector_for_business/go_interior_pointer.png" >}}

Với GC dạng semispaces, bởi vì tất cả object được cấp phát vào chung một space nên từ interior pointer
ta không có cách nào tìm ra kích thước của object để round down như Go mà bắt buộc phải build ra một cấu
trúc dữ liệu nào đó để scan, ví dụ như .Net. Mà món này thì quá chậm và phức tạp.

# Nhặt rác kiểu Apple

Garbage collector có lịch sử phát triển khoảng 60 năm. Và theo anh Apple thì nó là vấn đề vẫn chưa được
giải quyết. Nếu ai nào từng vật lộn với GC để tránh lag spike trong các ứng dụng soft-realtime như game 
hoặc transaction processing thì có lẽ phần nào cảm nhận được logic của nhà táo.

Thay vì GC (tracing GC), Apple chọn sử dụng reference counting.

Xung quanh topic này là vô vàn các cuộc cãi lộn cả trong giới khoa học lẫn dân làm sản phẩm. Tuy nhiên ở
đây tôi xin trích quan điểm của Chris Lattner, một cái tên rất "khét". Bố đẻ của LLVM, Clang, Swift. Có
thể nói Chris là nhân vật định hướng cả ngành công nghiệp hiện nay.

[[swift-evolution] What about garbage collection?](https://lists.swift.org/pipermail/swift-evolution/Week-of-Mon-20160208/009422.html)

## Deterministic destruction

Tương tự như RAII trong C++, ref count luôn call destructor rất sớm, ngay khi rời scope. Và thông thường
đó là điều mà chúng ta muốn. Trong khi GC chỉ call finalizer khi nó tiến hành dọn rác, tức là rất muộn,
và có thể trên thread không xác định. Thông thường với GC ta cần tự quản lý (unmanaged resource) để đảm bảo
nó được free đúng timing.

```C#
var file = new File(...);
// end of scope
```

Với GC, đoạn code trên là sai, ta cần tự Dispose object.

```C#
var file = new File(...);
file.Dispose(); // not exception-safe
// or with using (exception-safe)
using (var file = new File(...)) { }
```

## Object reference update overhead

Mỗi lần update ta cần tăng hoặc giảm counter, tuy nhiên GC thì lại có write-barrier để đảm bảo invariant.
Tức là cũng có overhead chứ không phải miễn phí, may ra semispaces kiểu naive như tôi implement cho CAM lúc
đầu thì không cần write-barrier, nhưng đó là do nó stop-the-world, tức là còn tệ hơn. Đã incremental GC
thì hầu hết đều cần write-barrier, hoặc read-barrier, hoặc là cả hai :)

## Heap fragmentation

Sau khi compact thì các object sẽ ở gần nhau hơn, điều đó tốt cho cache. Nhưng thực ra cái này có thể gây
tranh cãi. Vì quá trình compact cần duyệt qua gần như toàn bộ heap, quá trình này có cache performance rất
kém vì tiền đề là heap đang to và rất fragment (không thì compact làm gì?). Tức là về cơ bản ta tăng tốc một
đoạn xử lý bằng cách làm chậm một đoạn xử lý khác.

Fragmentation cũng có thể tránh được bằng pool, và rẻ hơn GC compaction.

## Weak pointer phức tạp

Reference counting không giải quyết được cyclic-reference. Vì vậy sẽ gây ra leak nếu không dùng weak pointer.
Mà dùng weak pointer thì phức tạp hơn là GC, vốn không cần tách bạch giữa hai loại này (nó trace từ rootset).

## GC có thể nhanh nhưng rất tốn RAM

Thông thường GC cần 3-4 lần lượng RAM so với reference counting để có performance tốt. Một số ông promote GC
thì bảo là Chris bias vì lấy tham chiếu từ paper cách đây 10 năm. Cá nhân tôi thì phần lớn những paper tôi từng
đọc cũng như kinh nghiệm thực tế cho thấy Chris đúng. Mà với một topic có 60 năm tuổi đời, cùng những thuật toán
cơ bản đều được phát minh từ 60 năm trước (về sau chỉ là improvement) thì 10 năm gần đây cũng không khác biệt quá
nhiều.

{{< figure src="/img/garbage_collector_for_business/relative_memory_footprint.png" >}}

Trục y là thời gian tương đối, và trục x là lượng RAM tương đối. Tương đối so với cái gì? so với không dùng GC.
Kết quả ở đây cho thấy GC cần lượng RAM gấp 6 lần để có thời gian xử lý tương đương với không dùng GC.

GC có thể rất nhanh, nhưng chỉ ngon trên server hoặc desktop, khi lượng RAM và điện ta cấp cho nó có phần
relaxed. So với lợi ích về thời gian và chi phí phát triển mà nó đem lại. Với môi trường mobile như OSX, hoặc
iOS thì GC chán vô cùng. Một con iPhone với 1GB RAM chạy ngon bằng một con Android 4GB RAM là có thật. Cho
đến thời điểm hiện tại, không dùng GC vẫn là financial win cho mobile.

# Nhặt rác kiểu CAM

Apple là một case study hay, tuy nhiên khó áp dụng với trường hợp của COBOL. Trừ khi ta muốn break COBOL standard
bằng cách thêm weak reference. Mặc dù vậy tôi vẫn cân nhắc khả năng này, vì từ sau COBOL 85 thì standard khá tệ,
và cũng ít được sử dụng. COBOL 85 thì chưa có dynamic allocation :)

Đầu tiên, loại trừ khả năng đi theo lối mòn của .NET và Java. Hai ông này thú thật đúng là nơi tinh hoa hội tụ
cùng phát triển. 60 năm có cái võ vẽ gì hay nhất cho GC thì họ thử hết rồi. Chắc chắn nếu chạy theo họ thì may ra
làm tốt gần bằng, đừng nghĩ đến chuyện tốt hơn. Mà thực tế là giờ họ cũng chưa phải tốt hẳn. Tức là tôi muốn tránh
tạo ra một thứ rối rắm, phức tạp và powerful trong khi bảo với user là tốt hơn hết đừng có dùng nếu nghiêm túc về
latency.

## Hướng (1): reference counting

Như case study của Apple, tôi thấy đây là một hướng khá hay. Mặc dù nó làm cho user phải suy nghĩ cẩn thận hơn
về quản lý memory và ownership của các object. Nhưng nó là điểm dung hòa giữa quản lý tự động và quản lý thủ công.
Nên nhớ trên thực tế GC (tracing) là giải pháp chưa thể hoàn hảo cho đến thời điểm này.

User vẫn sẽ phải suy nghĩ nghiêm túc về memory nếu cái họ làm là nghiêm túc. Compiler có thể hỗ trợ một số tiện ích 
tối thiểu, dễ implement và dễ chứng minh. Đồng thời tránh ngáng chân user khi họ muốn có thêm quyền kiểm soát. Trên
thực tế việc tối ưu một hệ thống sử dụng GC để đạt performance là khó khăn hơn rất nhiều so với việc học cách sử
dụng weak pointer hay memory pool.

## Hướng (2): private heap

Các hệ thống xây dựng với COBOL thì có hai thành phần cơ bản là OLTP (realtime transation) và batch processing.
Batch processing thì được tối ưu cho through-put, cho nên latency bởi GC không phải vấn đề quá lớn. Tất nhiên không
cần GC thì vẫn tốt hơn :) Còn OLTP thì câu chuyện khá phức tạp.

OLTP được tối ưu cho latency, và với số lượng concurrent task lớn. Vì vậy hai tính chất cơ bản cần thỏa mãn là không
pause quá lâu và không chơi blocking I/O. Nếu dùng blocking I/O thì phải có green thread. Sở dĩ z/OS (mainframe) và
COBOL trên z/OS thỏa mãn điều này là vì nó sử dụng scheduler riêng dạng cooperative, tối ưu cho latency và overhead
trên mỗi task nhỏ.

Trong khi đó, Windows hoặc Linux có scheduler dạng preemptive, với non-deterministic context switch phi thằng về 
kernel space. Tất cả đều không tốt cho latency, ta có thể spawn 10k COBOL task trên z/OS và blocking I/O thoải mái. 
Với Windows hay Linux thì spawn 10k thread là tự sát.

Scheduling là một vấn đề, tuy nhiên trong bài viết này ta tập trung vào memory management. Thì điều cần thiết
là GC không được pause quá lâu, tức là phải incremental. RAM thì thôi tốn cũng đành, server mà. Cùng lắm là chán
như .NET, Java là cùng. Tức là x1 vs x2 thì còn khác biệt, chứ x6 vs x8 thì như nhau (không thể chán hơn!).

---

GC với scale lớn và latency thấp là vấn đề khó. Tuy nhiên có một trường hợp GC vô cùng đơn giản, chắc chỉ phức tạp
hơn quả Cheney semispaces đời đầu mà tôi đã implement cho CAM một tý mà vẫn có latency lẫn khả năng scale cực tốt đó
là Erlang. Về khoản này thì nó chấp luôn cả Go chứ dăm ba ông Java với .NET lại càng không đáng nhắc tới.

Nguyên lý cũng dễ hiểu, Erlang sử dụng Actor Model, tức là các task không share state, và không sử dụng chung heap.
Mỗi task có một heap riêng, giao tiếp giữa các task thông qua message passing. Bởi vì các heap này có kích thước
nhỏ, nên về cơ bản ở đây không có bài toán heap to thì collect thế nào cho nhanh :-) Từng heap có thể được collect 
một cách độc lập, bởi vậy cũng rất là incremental.

Tuy nhiên vấn đề ở đây là, Erlang nhanh vì nó né bài toán heap to, nhưng không gì cản user tạo ra một actor rất to
(như trường hợp Wing 3D). Nói cách khác, để ứng dụng được mô hình của Erlang ta cần decompose bài toán theo actor
model.

Tuyệt đối tránh việc người thì code Erlang mà tâm hồn Java.

---

May mắn là, hai anh COBOL và Erlang ở chỗ này lại tư tưởng lớn gặp nhau. Nếu nhìn vào cách mà z/OS xử lý OLTP, ta
có thể thấy những điểm tương đồng. Trung tâm của OLTP là CICS transation server. Với nghiệp vụ chia nhỏ thành nhiều
transaction program khác nhau và giao tiếp qua channel (như message passing). Mỗi program này có thể coi như một
actor, với heap riêng.

Như vậy với OLTP, ta có thể map sang green thread + private heap.

(to be continued...)
