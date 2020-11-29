+++
title = "BMPM #1: lập trình suýt nhúng"
date = 2019-09-17
draft = false
authors = ["Nguyen, Giang (G. Yakiro)"]

tags = ["bare-metal", "fundamental", "baby-programmer"]
summary = "Học lập trình \"cơ sở\", để cho vui"
+++

Nhiều bạn muốn học lập trình "nhúng" nhưng không biết bắt đầu từ đâu.

Thực lòng mà nói đấy là chuyện của bạn, tôi cũng dell quan tâm lắm. Nói hơi phũ tý nhưng
câu chuyện dạy học, nhất là dạy học qua đường internet chưa bao giờ là mối quan tâm của
tôi luôn. Nói như vậy không phải là tôi đánh giá thấp chuyện học hành qua mạng, ngược lại
là đằng khác. Tôi thấy có rất nhiều người đang làm rất tốt rồi, tôi chả có gì mới để chia
sẻ, nên không muốn mất thời gian của các bạn vào những bài viết kém chất lượng do tôi tự viết.

{{< figure src="https://media.giphy.com/media/TS0HT0Km06WmQ/giphy.gif" >}}

Tuy nhiên thì lại tuy nhiên là, có nhiều bài viết hay bằng Tiếng Anh thật, nhưng dân mình
lười đọc bm. Nguyên nhân có phải liên quan đến vụ Anh hay Việt không thì tôi cũng chưa rõ
nữa. Nhưng thôi, lan man mãi thì tóm lại là tôi sẽ tăng cường viết bài bằng Tiếng Việt vào
những chủ để mà tôi thấy lập trình viên Việt Nam còn yếu. Lý do là tôi đang cần rất nhiều
nhân lực để tham gia phát triển platform cho ô tô. Thế nhưng nói thật là dell có một ai.

**Xin được nhấn mạnh là dell có một ai để cho tôi tuyển luôn!!!**

# Đối tượng nên đọc

<span style="color:green">**Nên đọc nếu bạn là:**</span>

* Người muốn tìm hiểu về nguyên lý, chống chỉ định fast-food.
* Đã có hiểu biết tốt về lập trình C, bài viết không dạy C.
* Nhiều thời gian để đọc và làm, tôi sẽ đi chậm.

<span style="color:red">**Không nên đọc nếu bạn:**</span>

* Chỉ quan tâm làm sao "get the job done".
* Đang phải nuôi con, nuôi gấu không có thời gian học.
* Bạn là managẻ, chỉ thích code bằng Excel.
* Bạn viết vào CV là biết dùng Eclipse.

# Cách quản lý bộ nhớ

Trừ khi bạn đang làm cái gì đấy nó rất là ...tởm. Thì khả năng cao là bạn đang dùng von-Neumann
Machine. Hiểu kiểu ngu si như tôi thì bạn có chương trình bao gồm một chuỗi các phép tính
toán, với input / output được lưu ở cái chỗ mà ta tạm gọi là `bộ nhớ` đi. Cứ giả dụ là ta có
RAM, khoảng 2GB.

Vậy làm sao quy hoạch 2GB đấy để dùng được?

```C
const char* PI_TEXT = "3.14";
```

Với khai báo trên thì vùng nhớ bạn dùng được compiler biết rõ luôn là 5 bytes. Hay còn gọi
là `cấp phát tĩnh`. Vùng này sẽ được cấp khi chương trình bắt đầu chạy, và giải phóng khi
chương trình kết thúc. Đơn giản, dễ chơi, dễ trúng thưởng.

---

Giả dụ như bạn cần chỗ lưu tên user.

```C
char name[1024];
```

Thế là user của bạn tên chỉ dài tối đa 1023 ký tự.

Bạn nghĩ rằng giới hạn tên của user là không hay, đọc bài này xong bạn sẽ tin điều ngược lại.
Khi mà ta không thể biết trước cần bao nhiêu bộ nhớ để chứa data, lúc mà data lại được cung
cấp khi chương trình chạy. Hay nói cách khác là compiler nó bó tay thì bạn phải tự quản lý.

```C
// static allocated,
char RAM[2*GB];
int used_bytes = 0;
// asks user how many chars his name is,
int name_length = // read from keyboard
// starts from unused location,
char *name = RAM + used_bytes;
// marks it as used,
used_bytes += name_length + 1;
```

Và C hỗ trợ việc trên cho bạn thông qua hàm malloc.

```C
char *name = malloc(name_length + 1);
```

**Hàm này trông có vẻ rất tiện, nhưng chuẩn MISRA-C nó cấm không cho dùng :)**

# Malloc không phải miễn phí

Trường hợp lý tưởng, khi mà user của bạn cứ tăng lên mãi thì việc cấp phát bộ nhớ đơn giản
là cộng cái `used_bytes` cho đến khi nào hết RAM thì thôi. Tuy nhiên thực tế sẽ có một bộ
phận user không sử dụng hệ thống nữa, và ta cần tái sử dụng lượng tài nguyên này để phục vụ
các user mới.

Tức là ta cần một cái gì đó để ghi nhớ lại xem những chỗ nào đã được dùng, và chỗ nào vẫn
còn trống. Giả dụ bạn lưu thông tin này vào linked list, thì chi phí khi malloc sẽ không
còn là O(1) như trên nữa mà là O(N). Vì bạn phải search để tìm chỗ trống. Cứ tưởng tượng
mỗi khi có user mới thì ta lại search một lần, ừ thì cũng chưa vấn đề gì lắm. Nhưng không
phải mỗi lúc đấy bạn mới malloc, thực tế là bạn malloc tùm lum nên chương trình chạy như rùa.

# Cấp phát bằng stack

Giả dụ bạn có cái hàm đếm số user.

```C
int num_users(int num, user_t* users)
{
	user_t head = users[0];
	if (head.name == NULL) {
		return num; // NULL terminated
	} else {
		user_t* rest = users + 1;
		return num_users(num + 1, rest);
	}
}
```

Tất nhiên là ta nên dùng `for loop`, ở đây tôi ví dụ minh hoạ thôi. Anw, ta cần 2 biến là
`head` với `rest`, thì câu hỏi là những biến đó được cấp phát vào đâu? Rõ ràng là ta không
thể biết trước được cái hàm `num_users` trên sẽ đệ quy bao nhiêu lần. Tức là thế này à?

```C
int num_users(int num, user_t* users)
{
	user_t *head = malloc(sizeof(user_t));
}
```

Đùa tý, về cơ bản là bộ nhớ sẽ được cấp phát vào cái gọi là `stack`, đây là một vùng hoạt
động theo kiểu last-in, first-out. Đúng với cách mà việc gọi hàm hoạt động. Khi ta gọi một
hàm, hàm đó cần thêm một lượng bộ nhớ, và chỉ giải phóng khi nó return. Nếu bên trong hàm
đó lại tiếp tục gọi những hàm khác thì về cơ bản là quá trình trên lặp lại.

```C
char STACK[4*KB];
int used_bytes;

{ // level 1
	user_t *l1_head = STACK + used_bytes;
	used_bytes += sizeof(user_t);

	{ // level 2
		user_t *l2_head = STACK + used_bytes;
		used_bytes += sizeof(user_t);

		{ // level 3
			user_t *l3_head = STACK + used_bytes;
			used_bytes += sizeof(user_t);

			// ...

			used_bytes -= sizeof(user_t); // deallocate l3_head
		}

		used_bytes -= sizeof(user_t); // deallocate l2_head
	}

	used_bytes -= sizeof(user_t); // deallocate l1_head
}
```

Đặc thù của stack cũng như việc gọi hàm là vòng đời của các biến là có thứ tự, khi L3
return ta giải phóng biến ở L3, L2 return ta giải phóng tiếp L2 và tương tự. Khác với việc
dùng malloc (hay con gọi là `heap`), với các biến có vòng đời ngẫu nhiên được cấp phát và
giải phóng không theo thứ tự nào.

Ưu điểm của việc cấp phát bộ nhớ trên stack là nó nhanh, cần bao nhiêu bytes cứ cộng thẳng
cái counter lên là xong. Rời khỏi hàm thì trừ đi chứ khỏi phải search như malloc. Tuy nhiên
vấn đề là lượng bộ nhớ mà nó sử dụng tỷ lệ thuận với số level. Với trường hợp không giới hạn
như đệ quy thì ta sẽ hết chỗ, với cái lỗi thần thánh là `stack overflow`.

# Auto-release pool

Với những hệ thống cần tốc độ, cách cấp phát kiểu stack như trên là tối ưu nhất. Mà trong
trường hợp ông nào lập trình iOS thì chắc đã từng nhìn thấy cái gọi là `@autoreleasepool`.
Không có gì đặc biệt ngoài đánh dấu một vùng code để sao cho bộ nhớ được cấp phát theo kiểu
**giống như là stack**. Tất nhiên là nó dùng heap nên không bị giới hạn vài KB như stack.

```C
@autoreleasepool
{ // level 1
	user_t *l1_head = automalloc(sizeof(user_t));

	@autoreleasepool
	{ // level 2
		user_t *l2_head = automalloc(sizeof(user_t));

		@autoreleasepool
		{ // level 3
			user_t *l3_head = automalloc(sizeof(user_t));

			// ...
		}
	}
}
```

Thì nó hoạt động kiểu như thế này:

```C
char POOL[many*MB];
int used_bytes;

@autoreleasepool
{ // level 1
	int marked = used_bytes;

	user_t *l1_head = POOL + used_bytes;
	used_bytes += sizeof(user_t*);

	@autoreleasepool
	{ // level 2
		// ...
		@autoreleasepool
		{ // level 3
			// ...
		}
	}

	used_bytes = marked; // free them all
}
```

Auto-release pool đặc biệt hữu dụng trong trường hợp ta cần cấp phát gấp một lượng lớn object
có vòng đời ngắn, ví dụ như xử lý string, bắn message hay đơn giản kiểu tôi cần chỗ dùng tạm,
dùng xong thì vứt đi hết nên khỏi phải quản lý làm gì cho mất thời gian. Nó giống như cái thùng
rác ý. Các object bạn tạo ra tính chất nó là rác, hết ngày bạn mang cả thùng đi đổ nên chả ai
dở hơi đi xếp rác trong thùng sao cho đẹp cả.

# Một số usecases

Đội làm game / GUI engine có vòng lặp kiểu ntn.

```C
while (running) {
	get_input();
	update_logic();
	render_to_screen();
}
```

Mỗi frame tạo ra rất nhiều rác, kiểu string("hello") + string("kitty").

```C
while (running) {
	@autoreleasepool
	{
		get_input();
		update_logic();
		render_to_screen();
	}
}
```

Hết frame hiện tại ta đổ hết đi là xong, next frame không cần.

## Tối ưu với webserver

Bản chất của webserver là phục vụ theo request, nên các webserver có trò tối ưu theo request
nốt, nhận được request thì spawn một cái pool mới, xử lý xong trả lời user rồi thì drop cả
pool đi luôn. Trong quá trình đó không giải phóng bộ nhớ, cách tối ưu này rất hiệu quả trong
trường hợp phục vụ các request có thời gian không dài ở front-end. Cầm đi làm back-end chạy
vài ngày đứt.

## Lập trình tên lửa

Đọc đâu đó không nhớ, đại khái có một chú mới join công ty làm tên lửa hành trình cho Israel.
Thanh niên này có background lập trình Java, có trọn bộ chứng chỉ AWS, chứng chỉ Oracle và
Visa Mẽo 10 năm. Đại khái là siêu nhân theo chuẩn công ty F. Vừa động vào quả code base của
công ty mới anh ý nhận thấy ngay là hệ thống phần mềm tự lái này nó đang bị leak bộ nhớ nặng.
Thế là ngay lập tức anh ý báo cáo với lãnh đạo và đề xuất chuyển sang dùng Eclipse.

\- Chú tính được nó leak 10KB mỗi giây à, ông sếp hỏi.

\- Dạ vâng thưa quý anh, em đề xuất dùng incremental GC cho nó realtime.

\- Thôi khỏi, chú tìm bọn hardware bảo nó lắp thêm 20MB RAM cho anh.

**Cần gì phải thêm Garbage Collector, bay đến đích nó tự collect mà.**
