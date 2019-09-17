+++
title = "Đào thoát khỏi IDE"
date = 2019-05-16
draft = false
authors = ["Nguyen, Giang (G. Yakiro)"]

tags = ["tool", "architecture"]
summary = "Làm IDE ngay cả khi bạn không phải là kỹ sư của Microsoft?"
+++

Một vị tiền bối mà tôi rất kính nể đã từng nói rằng:

> Học thuộc được 300 bài thơ Đường thì cứ cho là mình biết ngâm thơ,
> nhưng bảo tự sáng tác lấy một bài thơ thì vị tất đã làm nổi.
>
> -- <cite>Duong PT</cite>

Tôi thường hay nhớ lại thời kỳ những năm 2009 sau công nguyên. Cái thời mà **game
engine** nó vẫn còn là cái gì đấy rất kỳ bí và kích thích trí tò mò. Làm gì đã có
Unity đâu, trong khi bản quyền của một con engine hạng xoàng cũng lên tới hàng trăm
ngàn mỹ kim. Và dell phải ai chúng nó cũng bán.

Các cụ dạy "ngưu tầm ngưu, mã tầm mã". Làm gì cũng cần phải có team work. Cùng với
đám thanh niên ngổ ngáo ở thôn, chúng tôi tự tụ tập thành một băng đua đòi nghiên
cứu phát triển game. Với thành phần nhìn trông cũng như thật mà tôi tạm gọi là băng
PHỐT (FOS). Tất nhiên, với kinh nghiệm non nớt và phần lớn thành viên vẫn còn là học
sinh trung học, ăn cơm nhà nấu thì thay vì đi nghiên cứu làm game, chúng tôi lại tập
trung nghiên cứu làm engine. Điều mà mãi sau này tôi mới nhận ra là chả liên quan
vẹo gì đến nhau.

---

Câu chuyện về băng PHỐT thì có thời gian tôi sẽ chia sẻ sau để tránh lan man. Đại
để là với background như vậy thì tôi khá ưu ái cho những vị hảo hán có kinh nghiệm
phát triển engine, hoặc không thì cũng phải khung sườn, tool/toy gì đấy. Tuy vậy,
hiện tại thú thật là tôi chả tuyển được ai như thế cả. Mặc dù đã phím cho các em HR
là phải đặc biệt lưu ý, nhưng không có gì thậm chí là 01 cái CV. Có lẽ thời điểm này,
khi mà Unity đã quá mạnh thì cũng chẳng còn mấy ai đi nghiên cứu cơ sở nữa.

Nhưng kệ, tôi vẫn có niềm tin là đâu đó vẫn có người quan tâm đến phát triển in-house
toolset, được may đo cho những bài toán đặc thù mà móng vuốt của Unity vẫn chưa, hoặc
có thể nói là không bao giờ chạm tới được.

# Mega editor là gì?

Các bộ toolset mà chúng ta thường gặp như Visual Studio, Unity, Eclipse v.v. đều
được thiết kế với xu hướng tích hợp tất cả vào một tool duy nhất, mà dịch nôm thì là
"siêu công cụ". Tiếp cận với chúng, tôi thường hay bị choáng với số lượng tính năng
được cung cấp, ý tôi là số lượng nút bấm mà tôi có thể nhìn thấy được luôn. Chưa biết
dùng tốt hay không nhưng chắc chắn tác giả của cái đống này phải rất là "trâu bò".

{{< figure src="/img/escape_from_modern_ides/godot.png" title="Godot Game Engine" >}}

Tuy nhiên quan điểm thiết kế của tôi thì lại thiên về một thứ gì đó đơn giản giống
như bộ tool của UNIX mà chúng ta vẫn hay sử dụng. Ví dụ như *sed*, *sort* hay *diff*
chẳng hạn. Chúng được làm nhỏ gọn, tối giản và tập trung vào giải quyết những vấn đề
đã được xác định rõ ràng. Ta có thể học dần từng tool một, và cũng có thể kết hợp
chúng lại với nhau để giải quyết những bài toán phức tạp hơn. Kiểu thiết kế này không
chỉ tránh được việc người dùng phải đi lạc giữa hàng tá menu và nút bấm, mà nó còn dễ
maintain và tái sử dụng hơn rất nhiều.

# Vấn đề của mega editor

Nghe có vẻ như là tôi đang anti-IDE? Thực sự là không, tính "tích hợp" là rất quan
trọng. Tất cả những điểm tôi vừa nêu sẽ chẳng có nghĩa lý gì khi mà những tool này
không thể chơi được với nhau một cách mượt mà. Vậy nên tôi sẽ không đổ thừa cho IDE,
mặc dù thực tế nó hay dẫn tới kiểu thiết kế mega editor. Cái sai đầu tiên của mega
editor và cũng quan trọng nhất, hãy thử nghĩ về cái tool nào mà bạn chỉ dùng hết có
tầm 10% tính năng nhưng lại mất đến 5 giây để ngồi đợi nó mở?

Để cho chính xác, kiến trúc phần mềm nói chung không đến mức hổ lốn như vậy. Chúng ta
biết rằng sự phụ thuộc quá lớn giữa các component là không tốt, và ta cũng ưu tiên làm
sao để tái sử dụng được. Tức là từ mặt kiến trúc phần mềm mà nói thì mega editor cũng
có thể được thiết kế rất là ngon. Thực tế là phần lớn back-end component đều đạt được
điều đó. Ta không cần mở Visual Studio lên chỉ để compile một cái app.

Nhưng, giả dụ ta muốn thay đổi vị trí một cái nút trong file layout. Điều này chắc chỉ
mất 1 giây. À không, mất 6 giây vì ta còn phải đợi cái IDE nó khởi động xong đã. Thế
quái nào mà layout editor lại bắt buộc phải nằm trong đấy. Khiến cho chúng ta mất tận
5 giây lãng phí chỉ để ngồi đợi những thứ mà ta chả dùng đến? Nếu bạn nghĩ thế là ít,
thì có thể mạnh dạn nhân nó lên vài trăm file, đôi khi bạn chỉ muốn mở nó ra xem. Đấy
là lý do vì sao lập trình viên có kinh nghiệm lại thích xem code bằng Notepad, vì đơn
giản 5 giây là lâu vãi đái, thề!

# Micro service & Micro frontend

Ý tưởng thì cũng đơn giản thôi. Nếu bạn chưa biết thì micro-service là những khối xử
lý nghiệp vụ được thiết kế tách bạch hoàn toàn, ví dụ như compiler. Áp dụng kiểu ngu
si sang GUI thì ta cũng có khái niệm micro-frontend. Tức là những phần GUI được thiết
kế để chạy độc lập. Nhờ đó ta có thể lập trình, test, triển khai nó mà không bị phụ
thuộc vào những phần GUI khác.

{{< figure src="/img/escape_from_modern_ides/micro-tools.png" title="Cách phát triển tool theo chiều dọc" >}}

# Nói thêm về "framework"

Đoạn này viết dễ là bị ăn gạch no vì cái tội gắt.

Các SA diện tập sự, thậm chí đã có kinh nghiệm hay có xu hướng xây dựng "framework".
Tất nhiên, nó là một bước nhảy vọt từ tư duy engineer dạng nông dân, chả bao giờ thèm
nghĩ nhiều về kiến trúc, năng suất hay là tái sử dụng. Tôi cũng thường khuyến khích
việc tạo ra framework. Nhưng chủ yếu vì mục đích giáo dục là chính. Thực lòng mà nói
phần lớn "framework" này đều thuộc dạng tào lao.

Vấn đề lớn nhất là, framework sẽ tạo ra các ràng buộc ở phạm vi rất rộng. Thường yêu
cầu tất cả các component phải tuân thủ theo một cái thư viện, lối suy nghĩ hoặc công
nghệ nào đấy. Điều này có thể trở thành vấn đề lớn trong những dự án quy mô, yêu cầu
khả năng mở rộng và linh hoạt cao, gần như không thể tiên lượng trước được. Lúc đầu
thì framework cũng ngon đấy, cải thiện khả năng tái sử dụng, tăng năng suất. Tuy vậy
về dài hạn thì cái bóng của framework sẽ trở nên quá lớn, vượt ngoài tầm kiểm soát mà
tôi hay gọi là "lời nguyền framework". Khiến cho toàn bộ source code bị phụ thuộc vào
nó, và bất cứ thay đổi nào cũng gần như là bất khả thi. Đơn giản là có quá nhiều thứ
sẽ không thể hoạt động được nữa và cần phải sửa lại cùng một lúc.

---

Hầu hết các IDE mà tôi gặp đều buộc các tool phải sử dụng một framework nào đấy. Ít
nhất là phải dùng chung một loại GUI. Nghe qua thì cũng không phải là tệ lắm, cho đến
khi bạn buộc phải dùng một cái GUI hoặc render engine khác vì lý do bất khả kháng như
độc quyền (proprietary) hoặc tích hợp những cái tool đã có từ trước (legacy). Thường
việc tích hợp này là vẫn khả thi, tuy nhiên độ khó nhằn thì khỏi phải nghĩ.

Tất nhiên, chúng ta không thể loại bỏ hoàn toàn framework, vì thực ra nó tốt cho việc
"tích hợp" các tool lại với nhau. Chúng ta muốn, trong trường hợp cần thiết là chúng
nó có thể giao lưu, hợp tác và cung cấp trải nghiệm mượt mà chả khác gì IDE cả. Mà nó
đúng là IDE luôn cho nó nhanh. Mặc dù vậy, ta nên cẩn thận và cố gắng để framework này
càng đơn giản và ngu si thì càng tốt.

Trong kiến trúc micro-tool, ta sẽ tách việc xử lý nghiệp vụ và tích hợp thành 2 tầng
khác nhau. Với tầng nghiệp vụ, cả front-end lẫn back-end thì đều không có ràng buộc về
framework gì hết, thích dùng cái gì cũng được. Web, .Net hay bất cứ cái gì miễn là phù
hợp nhất. Về tầng tích hợp, cái này được chuẩn hoá bằng IPC và có thể có framework cho
những nền tảng phổ biến. Tuy nhiên, việc sử dụng framework này là không bắt buộc.

# Khả năng tích hợp

Ta đã nói nhiều về "integrated" trong IDE, giờ là ví dụ cụ thể.

Thông thường, một ngày làm việc của bạn bắt đầu bằng việc dùng tool, hoặc một vài tool
để mở cái gì đó ra. Sau một lúc, có thể có thêm tool được chạy, và một số thì bị tắt đi
nhưng ta vẫn muốn những gì đang ở trên màn hình thì vẫn phải nói chuyện được với nhau.
Giả dụ như ta đang xem một cái GUI bằng tool Viewer đi. Thế rồi ta thấy là animation để
đóng mở sidebar của GUI này chưa sướng nên ta quyết định là phải sửa lại nó một chút.

Để lập trình lại cái animation này thì ta lại cần một cái tool khác vì lý do có vẻ hiển
nhiên? May thay, cái tool đấy đã được "tích hợp sâu" vào Viewer nên ta chỉ cần bấm một
cái nút và nó sẽ tự động chạy thêm tool Animator trong nửa nốt nhạc. Sau đó ta có thể
chỉnh sửa lại animation đồng thời xem kết quả luôn trên Viewer.

Tích hợp được vậy là cũng đã khá ngon rồi, nhưng sẽ thế nào nếu cái tool Animator đã
được mở sẵn từ trước? Có thể là ta nên dùng lại luôn instance này. Bởi vì các thay đổi
liên quan đến animation thì đã được auto-save nên việc dùng lại sẽ giúp user đỡ phải mở
thêm và quản lý quá nhiều instance. Chỉ cần thay đổi đối tượng đang được xử lý là xong,
kiểu như đổi context qua lại thôi. Kiểu UX này là chuẩn mực cho các IDE ngày nay.

{{< figure src="/img/escape_from_modern_ides/sa-luggage.jpeg" title="\"Tool nhiều để mà làm gì\". Photo by <a href=\"https://unsplash.com/@randomlies?utm_source=medium&utm_medium=referral/\">Ashim D'Silva</a> on Unsplash" >}}

Thế thằng nào quyết định là mở thêm instance hay dùng lại?

Ở trên cùng của toolset, ta cần một cái component gọi là "Workspace". Việc này là cần
thiết để đảm bảo được trải nghiệm người dùng tốt nhất. Nó chịu trách nhiệm kết hợp các
thành phần GUI của các tool khác nhau, chạy các policy, quản lý mode v.v.

---

Dần dần, khi số lượng tool tăng lên thì chúng ta lại gặp vấn đề khác là phân mảnh. Tức
là về logic thì có vài GUI component ít nhiều trông là giống nhau giữa các tool. Nếu cứ
phải mạnh thằng nào thằng đấy dùng thì không hiệu quả và cũng dek phải IDE nữa. Có cái
IDE nào mà mỗi tool lại có một chỗ ghi log riêng không? Chắc là không. Vậy thì làm sao
để có thể chia sẻ và tái sử dụng những GUI component này, khi mà thậm chí các tool còn
không sử dụng chung một stack công nghệ?

{{< figure src="/img/escape_from_modern_ides/sketch-inspector.png" title="Làm sao dùng chung Inspector như Sketch?" >}}

Bằng cách phân tách tiếp, ta có thể coi những GUI component này là micro-tool luôn. Nếu
nhìn ở mức độ trừu tượng nhất định thì cái Inspector cũng khá là common. Tất cả những gì
nó làm chỉ là object có những thuộc tính gì thì nó hiện hết ra và cho phép chỉnh sửa thôi.
Như vậy ta có thể tạo ra micro-inspector, làm việc dựa trên schema như JSON rồi cung cấp
IPC tương ứng. Đơn giản là ta bảo nó, "hey, đây là schema, và đây là data". Sau đó khi
user thay đổi gì thì nó lại báo lại là xong.

# Kiến trúc fail-safe

Khả năng chịu được lỗi là cực kỳ quan trọng với việc phát triển tool. Chả ai muốn bao
nhiêu công sức đổ sông đổ biển chỉ vì tự dưng cái tool chết tiệt nó lăn ra chết. Các IDEs
ngày nay phải đầu tư rất nhiều để đảm bảo là tool giao cho khách hàng sẽ không chết
bất đắc kỳ tử chỉ vì một cái bug ngu si nào đấy mà ai cũng có thể nhận ra.

Tuỳ vào định nghĩa thế nào là bug ngu si, tôi khá nghi ngờ về việc cái tool của tôi hay
của bạn nó sẽ là ngoại lệ. Hay nói đơn giản là kiểu dell gì cũng đầy bug ngu si cho mà xem.
Tất nhiên chúng ta phải cố làm sao để không để nó lọt đến tay người dùng. Nhưng mà chừng
nào nó vẫn còn được tạo ra bởi bàn tay còn người thì bug đơn giản là không thể tránh.

---

Để thực sự tự tin về khả năng là đã zero bug. Ta thường phải trả cái giá khá lớn cả về
thời gian lẫn tiền bạc để làm đủ thứ testing mà phần lớn các bạn chả tưởng tượng ra được
đâu. Thậm chí ở một số domain, khi mà chất lượng phải là trên hết thì việc test này nó
còn là cả một bộ môn khoa học mà có thể làm luận án tiến sĩ được luôn. Ấy vậy nhưng mà
dù cho có đầu tư bao nhiêu cho test đi chăng nữa, ta cũng chả bao giờ chắc chắn được là
zero bug. Mà chỉ cần một con bug thôi là cũng có thể đi tong cả hệ thống thì đầu tư vậy
là không khôn ngoan.

> Test có thể cho ta biết là chương trình có bug,
>
> nhưng không thể cho ta biết là chương trình không có bug.
>
> -- Edsger W. Dijkstra.

{{< figure src="/img/escape_from_modern_ides/fail-safe-bike-locking.jpeg" title="\"Khoá này mới gọi là an toàn\". Photo by <a href=\"https://www.flickr.com/photos/dustinq/\">Dustin Quasar</a> on Flickr" >}}

Cách tiếp cận tốt nhất để đạt được fail-safe trong việc phát triển IDE, có cân nhắc đến
hoàn vốn là "cứ để cho nó crash" (let it crash). Tất nhiên là tôi không bảo bạn đừng có
test làm gì và cứ thế cho người dùng sống chung với bug. Triết lý ở đây là ta tin rằng
chương trình kiểu dek gì mà chả có bug, thế cho nên là ta sẽ ưu tiên thiết kế sao cho kể
cả trường hợp xấu nhất xảy ra thì nó cũng không đến mức thành thảm hoạ. Còn test thì vẫn
phải làm thôi, nhưng sẽ không phải ưu tiên số một.

---

Vậy, làm sao để hệ thống tool an toàn trước crash?

Đầu tiên, ta cần thu hẹp phạm vi mà một crash bất kỳ có thể ảnh hưởng. Trong trường hợp
này thì nó là một OS process. Tuỳ vào stack công nghệ của mỗi tool mà phạm vi này có thể
nhỏ hơn. Ví dụ exception handling hoặc supevisor như Erlang. Tuy nhiên, ở mức kiến trúc
tổng thể mà nói thì ta chỉ quan tâm đến OS process vì thấp hơn là freestyle.

Sau đó, ta thậm chí có thể phân tách từng tool ra những process nhỏ hơn. Ít nhất thì
tầng xử lý dữ liệu nên được tách bạch khỏi GUI. Ngoài các lý do truyền thống, điều này
còn giúp tool có thể khôi phục lại khi chẳng may GUI bị crash. Bởi vì dữ liệu không đặt
ở GUI cho nên nó vẫn chưa bị tổn hại.


# Kết luận

Micro-tool là một hướng tiếp cận mới cho việc phát triển IDE mà theo tôi là có nhiều điểm
ưu việt so với kiến trúc monolithic thường thấy. Tuy vậy, việc điều phối và tích hợp các
micro-tool để đảm bảo trải nghiệm xuyên suốt vẫn là thách thức lớn, là điểm mấu chốt để
đưa kiến trúc này vào thực tế. Ở những bài viết tiếp theo, chúng ta sẽ đi sâu hơn vào vấn
đề này và thử làm một vài PoC. Chắc sẽ có nhiều giá trị hơn là chỉ lý thuyết.