+++
title = "Đâu phải hướng đối tượng?"
date = 2020-03-30
draft = false
authors = ["Nguyen, Giang (G. Yakiro)"]

tags = ["paradigm"]
summary = "Một số paradigm đáng học ngoài OOP"
+++

Tôi vừa đọc được một câu hỏi khá hay liên quan đến DOD và liệu nó có kill mất thằng OOP hay
không. Mặc dù tôi đã thử trả lời bằng một comment cũng khá dài dòng nhưng có vẻ nó cũng chưa
được rõ ràng lắm, document lại bằng một blog thì sẽ tốt hơn.

# OOP là cái gì?

OOP dịch sang Tiếng Việt là `lập trình hướng đối tượng`. Hiểu và thành thạo OOP là bước cơ
bản mà bất cứ lập trình viên nào cũng phải trải qua. Nhất là các bạn sinh viên được đào tạo
bài bản qua trường lớp thì hầu hết ai cũng biết OOP cả. Cảm hứng của OOP là mô phỏng thế giới
thực nơi chúng ta đang sống. Với các đối tượng riêng biệt thuộc các loài, lớp khác nhau. Các
đối tượng có thể chịch nhau và tạo ra các đối tượng mới kế thừa đặc điểm của cả hai. Ví dụ về
đối tượng như con mèo, chú vịt, bể cá, bố và cô giáo etc. Cách tiếp cận này được cho là dễ hiểu,
tự nhiên và dễ tiếp cận với người mới học lập trình nhất.

# Có phải chỉ cần OOP là đủ?

Về mặt chém luận mà nói, OOP là phổ biến nhất. Nhưng trong thời đại trăm hoa đua nở, học phái,
hệ phái nhiều như nấm sau mưa này thì OOP cũng chỉ là một lựa chọn thôi. Nó phổ biến nhất tại
thời điểm này không có nghĩa là nó hay nhất, đúng nhất trong mọi trường hợp. Anh em có thể
refer tình hình như kiểu Bách Gia Chư Tử thời Xuân Thu, Chiến Quốc của Tàu. Dần dần sẽ có một hệ
tư tưởng nào đó trở thành chủ đạo như Nho giáo. Lý do là nó dễ hiểu, dễ thấy kết quả. Nhưng qua
quá trình phát triển lịch sử thì nó cũng sẽ bộc lộ ra những điểm yếu. Buộc phải `gạn đục khơi
trong` hoặc biến mất. Nói xa thì chả ai dám nói đúng, nhưng một điều chắc chắn đúng là nên để
cho các hệ phái tự do phát triển. Cái này Phương Đông làm tốt hơn Phương Tây. Mấy ông Tây hơi
tý là thánh chiến vs tử vì đạo, dị giáo đồ bloh blah. Đập nhau bùm bụp suốt ngày là không hay.

---

Bước đầu tiên để nắm bắt vận mệnh, khai phá thiên cơ là ta phải đạt được trạng thái tâm tĩnh
như nước, sẵn sàng tiếp nhận những quan điểm thoạt nghe như củ cải. Không cùng đạo, khó nói chuyện
thì chỉ là hạng phàm phu tục tử. Phải khiêm tốn, anh em có thể tập thiền, tập yoga hay dán băng dính
vào mồm chơi DotA thì tuỳ. Như mình thì pro rồi nên không cần. Ngoài ra thì buông đao đồ tể, lập tức
thành phật cũng là một phương pháp. Anh em có thể học Java, trở thành fanboy OOP rồi một ngày bỗng
dưng nhận ra Java như loz.

Giữa các hệ phái luôn có sự giao thoa, kế thừa và phát triển. Cho nên khi ta dùng con mắt OOP để
nhìn code thì code nào cũng OOP hết. Cái này cần tránh, không phải cứ thấy có class, object, đóng
gói hay kế thừa thì đấy là OOP. Ngồi dưới cây bồ đề có thể là Phật, cũng có thể là bán hàng rong.
Muốn nhận diện chuẩn một chương trình theo những paradigm nào không dễ, cần học nhiều paradigm và
có nhiều kinh nghiệm thực chiến.

# Giới thiệu qua về ECS

ECS là entity-component-system. Một paradigm được tạo ra để giải quyết bài toán `cây kế thừa sâu`.
OOP được quảng cáo là thuận theo tự nhiên, mà trong tự nhiên thì nó có cái gọi là phả hệ. Tức là
các class hậu duệ sẽ kế thừa những đặc tính của class tổ tiên. Tuy nhiên các anh OOP quên mất một
điều rằng phả hệ nó giải quyết bài toán `chọn lọc tự nhiên`. Trong khi cái các anh giải quyết lại
là `khả tăng tái sử dụng`, tức là chả liên quan gì mấy.

Anw, việc tạo ra một cái phả hệ tốt thực ra nguyên lý cũng rất giống với chọn lọc tự nhiên. Giả dụ
ta có một xã hội với khởi nguồn từ một ông tổ duy nhất là Bàn Cổ. Các bạn có thể thay bằng Adam vs
Eva thì cũng thế, multiple root thôi. Vậy nếu như một ngày dân nó đốt rơm nhiều quá dẫn đến băng
ở hai cực nó tan sạch, nước biển nhấn chìm phần lớn bề mặt trái đất thì feature mới mà chúng ta
cần là `bơi`. Tốt, thế khả năng bơi này nên cho vào đâu trong phả hệ?

Giả dụ khả năng bơi này không bị thoái hoá, điều mà OOP không cho phép. Thì nếu muốn toàn dân biết
bơi tốt hơn hết ta nên cho Bàn Cổ học bơi. Khi đó thì toàn bộ con cháu của lão đều biết bơi hết. Còn
nếu ta chỉ cho một số hậu duệ của lão học bơi thì sẽ có trường hợp nhiều người sẽ rơi vào nhánh không
biết bơi. Trường hợp này vừa có tốt lại vừa có xấu. Mặt tốt là không biết bơi thì cũng chả sao nếu
bạn sống ở vùng hoang mạc. Thay vì biết bơi thì ta có tài nguyên để biết cái khác còn hơn, ai cũng
phải biết bơi là dư thừa, không cần thiết. Mặt không tốt là, vì có nhánh biết bơi có nhánh không nên ta
sẽ bị trùng lặp về code (mã di truyền?). Ngoài ra đột biến cũng khác nhau, kiểu có người biết bơi
ngửa, có người lại chỉ biết bơi ếch, hoặc như tôi là bơi phao.

Giả dụ tiếp là chúng ta có vô số vũ trụ song song, với phả hệ khác nhau. Nhưng điểm chung là đều biết
đốt rơm, tức là khả năng bơi là cần thiết thì vũ trụ có class Bàn Cổ nhưng biết bơi sẽ mạnh hơn các
vũ trụ mà Bàn Cổ chỉ biết đốt rơm. Về sau chiến tranh giữa các vũ trụ xảy ra thì nơi vũ trụ nào mà Bàn
Cổ không biết bơi dẫn đến ít dân thì sẽ thua và biến mất. Qua quá trình chọn lọc như vậy mãi thì ta
sẽ ra được cái gọi là phả hệ hoàn hảo.

---

Ok, lan man một lúc giờ quay lại bài toán OOP. Vì ta đang lập trình nên có khả năng thần thánh là
viết lại cây kế thừa (phả hệ) nếu ta thấy nó ngu, tức là refactoring. Tuy nhiên viết lại thế nào cho
ngon thì lại là câu hỏi khó. Ví dụ ta thêm feature thì nên thêm vào đâu, thêm cao quá thì thừa, nhiều
thằng nó không dùng. Thấp quá lại bị trùng lặp rồi sau đột biến lung tung. Mà có khi cái feature mới
ta thêm vào lại không tốt sau phải thoái hoá nó đi thì khó càng thêm khó. Chọn lọc tự nhiên là quá
trình dài và qua vô số phép thử sai mới ra được kết quả tốt nhất. Ta không có thời gian và nguồn lực
như vậy.

Đến đây một số domain sẽ phải tìm cách thay đổi để tồn tại. ECS được tạo ra như vậy, gốc của ECS là từ
nghành công nghiệp game. Game là cái tạo ra với mục đích giải trí, tức là làm sao chơi cho sướng. Mà
cái này thì nó rất vô cùng. Giả dụ hôm nay ông designer bảo game phải là first person shooter, cho
phép lái thằng nhân vật đi quanh map bắn nhau. Hôm sau ông ý lại bảo thằng nhân vật bắn rocket thì phải
cho lái cả quả rocket nữa. Bao nhiêu ông coder sẽ nghĩ ra từ đầu được là thằng nhân vật vs quả rocket
phải kế thừa từ cùng một class là FirstPersonShooterCamera? Làm gì có.

Vì vậy ECS ra đời, và scale bằng cách tổng hợp thay vì kế thừa. Trong ECS một entity (object) sẽ được
tổng hợp lại từ nhiều component. Ví dụ như ta có bức tường, thì nó là tổng hợp của một component để vẽ
ra bức tường và một component vật lý để ngăn nhân vật đi xuyên qua. Ngày mai thằng designer nó thích
ném lựu đạn sập tường thì thêm một component là máu. Vì đạn trừ máu, hết máu tức là bị phá huỷ. Đạn gây
sát thương làm mất máu, không quan tâm đấy là máu của thằng nào. Cần bơi thì viết thêm component bơi,
thằng nào muốn biết bơi add thêm vào là xong, không ảnh hưởng đến ai.

# Giới thiệu qua về DOD

DOD là `data oriented design`, hay bị nhầm với `data driven design`. DOD là triết lý lập trình trong đó
quan tâm đầu tiên là dữ liệu và cách tổ chức nó sao cho có thể truy cập hiệu quả nhất. Chứ không phải
đối tượng và các mối quan hệ logic giữa chúng. Motivate của việc này là CPU giờ chạy quá nhanh so với tốc
độ của RAM. Nên CPU phải thêm cache để tăng tốc, và chỉ truy cập RAM khi thực sự cần thiết. Tức là khi
cái ta cần nó không có trong cache.

[Latency Numbers Every Programmer Should Know](https://colin-scott.github.io/personal_website/research/interactive_latency.html)

Cache hoạt động theo line, tức là nó không cache từng byte mà là nhiều byte liên tục một lần. Vì vậy cách
hiệu quả nhất là ta tổ chức bộ nhớ sao cho những cái gì truy cập đồng thời thì nằm gần nhau. Nếu xa nhau
thì mỗi thằng sẽ cần một cái cache line riêng. Mà số cache line thì có giới hạn nên nếu đầy là nó vứt bớt
đi và phải load loại từ RAM.

# DOD vs ECS?

ECS hỗ trợ rất tốt cho DOD. Trong khi với OOP ta thường vô tình nhóm nhiều dữ liệu không liên quan lại gần
nhau bởi vì chúng cùng một đối tượng. Thì với ECS việc tổ chức dữ liệu ra sao được quyết định bởi thiết kế
riêng của từng system (system là cái cung cấp component). Ví dụ với tính toán va chạm vật lý thì ta sẽ có
PhysicSystem, thằng này sẽ cung cấp các component liên quan như collision cube, collision sphere etc. Đến
bước tính toán va chạm ta chỉ truy cập duy nhất đến những component này và không bị load nhầm những dữ liệu
rác. Thậm chí một số physic engine hỗ trợ xử lý trên GPU hoặc phần cứng riêng khác, đòi hỏi yêu cầu đặc biệt
về data layout thì với ECS điều này cũng trở nên đơn giản.

# Qt Quick là OOP hay DOD?

Qt Quick (QML) là một render engine khá mạnh dùng để tạo giao diện đồ hoạ cho những thiết bị máy móc và xe.
Với đặc thù trên thì yêu cầu về performance rất khắt khe (tạm chưa nói đến khả năng scale logic vì lan man).
Cách thiết kế của Qt Quick khá thú vị khi nó giải quyết được bài toán performance mà không hy sinh khả năng
dễ đọc, dễ dùng. Điều mà DOD hay bị blame vì tư duy `data oriented`.

Về cơ bản Qt Quick sử dụng hai bộ dữ liệu song song, một hướng đến high level logic, một hướng đến rendering.
Khi lập trình ta chỉ sử dụng bộ dữ liệu logic (QtQuickItem), với cấu trúc không tối ưu cho tốc độ nhưng lại
dễ hiểu. Đến khi chương trình thực thi, Qt sẽ đồng bộ và tạo ra dữ liệu render (QSGNode) với cấu trúc theo
DOD.

# OOP và DOD loại trừ lẫn nhau?

OOP về cơ bản ngược với DOD, vì nó tổ chức dữ liệu không tốt cho cache. Nhưng khi xây dựng hệ thống thì tuỳ
vào thiết kế và mục đích thì chúng vẫn có thể cùng tồn tại. Cũng như ECS thì điểm khác biệt với OOP là nó scale
logic theo chiều ngang, tạo ra thêm component chứ không dựa trên kế thừa phả hệ. Nhưng để implement một component
hay một system riêng lẻ ta vẫn có thể sử dụng OOP nếu phù hợp. Mấu chốt là không dùng OOP như công cụ chủ đạo
để scale. Ngoài ra giữa các `level of design` khác nhau cũng có thể sử dụng và kết hợp những paradigm khác nhau,
miễn là với ý đồ rõ ràng và không mâu thuẫn.