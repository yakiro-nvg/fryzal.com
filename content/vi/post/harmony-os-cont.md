+++
title = "Vén màn Harmony OS (phần 2)"
date = 2021-06-07
draft = false
authors = ["Nguyen, Giang (G. Yakiro)"]

summary = "Kẻ tiên phong một cách bất đắc dĩ"
+++

# Thương chiến Mỹ-Trung

Không cần phải đến khi Trump cấm cửa Huawei thì người ta mới nghĩ đến một thế giới
ít xoay quanh Android hơn. Ngay cả khi IoT, thứ được kỳ vọng sẽ thay đổi cuộc chơi
vẫn còn chưa biết bao giờ mới cập bến, thì những nỗ lực làm suy yếu ảnh hưởng của
Android đã không phải hiếm.

Ngay trên sân nhà của Android là smart phone, thì WeChat như một mini-OS mà hệ sinh
thái nó cung cấp khiến Android trở nên ít liên quan. Tầm nhìn về một siêu ứng dụng
mà người dùng cuối "chỉ dùng mỗi nó" là cái mà rất nhiều gã khổng lồ vẫn đang sử
dụng để níu giữ và lan tỏa các giá trị trong hệ sinh thái của mình. Có thể kể đến
như Facebook Messenger, Alipay, MS Office v.v..

{{< figure src="/img/harmony_os_2/wechat_starbucks.gif" title="Ứng dụng của Starbucks trên WeChat" >}}

Lẽ ra câu chuyện sẽ rất xuôi chèo mát mái, cơm ai người nấy ăn. Google giỏi làm OS
cho smart phone, giỏi cloud v.v.. thì chả ai dở hơi tự dưng lao vào so găng với nó
làm gì cả. Cũng có vài kẻ thách thức như combo Nokia + Microsoft hay BB thì sản phẩm
cũng "mồ yên mả đẹp" hết rồi. Ông nào còn ôm chí lớn thì vẫn chậm rãi R&D chờ ngày
IoT bùng nổ. Trong vô hình tiếng mài dao vang lên không ngớt.

Lác đác có vài món item được mang ra ánh sáng như Tizen, WebOS v.v.. và cũng gặt hái
được một số thành công nhất định. Tuy nhiên về cơ bản chừng nào smart phone vẫn còn
mạnh thì người ta vẫn còn phải ngại Android.

Thế nhưng một ngày đẹp trời anh Trump bảo Android là công nghệ Mỹ, tao thích cho
thằng nào dùng là quyền của tao. Thử đặt mình vào vị thế của Huawei, một công ty mà
mảng smart phone đã có lúc thị phần đứng đầu thế giới và đóng góp gần một nửa vào
doanh số của họ thì phải làm thế nào?

# Hệ sinh thái Android

Trên lý thuyết, Android Open Source Project (AOSP) là mã nguồn mở và hoàn toàn miễn
phí. Điều đó cho phép một nền tảng di động mở và không bị độc quyền bởi một hãng duy
nhất, ví dụ như Apple. Tuy vậy trên thực tế thì nơi duy nhất mà điều này còn đúng
lại là... Trung Quốc.

Hãy tạm chưa nói đến YouTube, Google Maps v.v.. những thứ rõ ràng là nếu bị cấm thì
đã đủ tai hại cho Huawei. Hãy nói về một thứ thoạt nghe có vẻ rất cơ bản là push-notification.
Nếu như trước đây ta có thể đơn giản là mở một socket và chờ thông báo từ server. Thì
ngày nay cách duy nhất để wake-up một ứng dụng dựa trên event từ bên ngoài là thông
qua cloud API của Google.

Công bằng mà nói, push-notification liên quan khá sâu đến cách OS lập lịch cho các
chương trình, nếu làm không tốt thì sẽ ảnh hưởng đến thời lượng sử dụng pin cũng như
giảm độ mượt mà của hệ thống. Việc bắt buộc sử dụng một cách duy nhất và tốt nhất sẽ
mang lại trải nghiệm tốt hơn hẳn là thả rông cho các ông dev thích làm kiểu gì thì làm.

{{< figure src="/img/harmony_os_2/huawei_hms_core_4.0.jpg" title="Không có GMS, thì phải làm HMS" >}}

Thế nhưng khi Trump cấm Huawei chơi với Google, thì tức là cái cloud API kia cùng các
service đi kèm dành cho push-notification (GMS) nó cũng thành "công nghệ Mỹ" nốt. Tất
cả những ứng dụng muốn hoạt động trên thiết bị của Huawei, dù cho vẫn là Android thì
cũng phải lập trình lại. Đó là lý do mà Huawei phải cấp tốc đẩy nhanh nền tảng thay
thế là HMS và đốt thêm kha khá tiền mồi chài.

# Kẻ tiên phong bất đắc dĩ

Nếu không dính chưởng công nghệ Mỹ thì tôi tin là Huawei chẳng dại gì mà thách thức
Android vào lúc này. Yếu tố "thời điểm" là rất quan trọng trong kinh doanh. Vẫn sẽ có
HMS và Harmony OS nhưng nó sẽ chỉ đánh vào IoT khi mà thị trường đã sẵn sàng chấp nhận.
Họ đang bị buộc phải vào quá sớm và đốt tiền R&D cũng như để educate thị trường. Rất
dễ lăn ra chết vì hết tiền trong khi thành quả để thằng khác vào sau nó hưởng.

Vậy nên mặc dù bản thân tự nhận là fan của Huawei nhưng thật thà mà nói tôi cũng không
lạc quan lắm vào tương lai của họ. Hiện tại thì Huawei là một tay chơi cũng khá có số
má khi dẫn đầu một trong những công nghệ bản lề của IoT là 5G. Trung Quốc cũng đang nổi
lên là một thiên đường công nghệ.

{{< figure src="/img/harmony_os_2/harmony_os_1_8_n.jpg" title="Seamless AI Life strategy" >}}

Mảng smart phone nói riêng và IoT nói chung của Huawei thì thuộc về nhóm ngành hàng tiêu
dùng (Consumer BG) với chiến lược "1 + 8 + N" và thông điệp "seamless AI life strategy".
Trùng hợp là hồi 2019 tôi làm cái proposal cho hệ thống thông tin giải trí trên ô tô thì
cũng nhấn mạnh vào "seamless" =]

Ý tưởng của "1 + 8 + N" như sau: "1" ở đây là smart phone; "8" là máy tính bảng, PC, các
thiết bị thực tế ảo (VR), các thiết bị đeo (wearables), màn hình / tai nghe / loa thông
minh và hệ thống giải trí trên ô tô (head units); cùng với "N" là vô vàn các thiết bị IoT
khác. Bài toán là mang lại trải nghiệm xuyên suốt + AI.

Tôi cũng không chắc lắm tại sao chiến lược của họ vẫn xoay quanh "1", có lẽ thời điểm này
doanh số từ smart phone vẫn quá quan trọng với Huawei nên họ muốn giữ nguyên hiện trạng
nhất có thể. Tôi thì quan điểm hơi khác là smart phone thì cũng là "thing" và xu hướng sẽ
"ngu" dần đi để nhường chỗ cho 5G + edge.

{{< figure src="/img/harmony_os_2/dark_cloud.png" title="Chiến lược 'đám mây đen' (gần mái nhà hơn do nhiều nước)" >}}

Tất nhiên proposal trên của tôi là cho một ISP ở Việt Nam, nên nó hơi couple vào business
của họ là OTT. Kiểu âm mưu bán TvBox để growth xong thành công thì lại quay về bán head
unit cho ô tô. Nhưng trời đụ không hiểu sao trình bày xong họ kêu là mấy cái em nói bọn
anh có thiếu gì đâu, "hệ điều hành bọn anh cũng tự làm được rồi", tên cũng đặt xong rồi
luôn. Tự tin có giác gì "bờ-cạp" với cái công ty mà ai cũng biết là công ty nào ấy đâu.
Nên đành ngậm ngùi đi về với kết luận riêng cho bản thân là thị trường quá non và xanh
còn mình thì tiến có đèo.

# Nhanh nhưng cần đủng đỉnh

Nếu như Samsung có thể tạm yên tâm chờ đến ngày IoT bùng nổ, thì chiến lược của Huawei
lại phải là một sự kết hợp giữa nhanh và chậm. Quá chậm thì hệ sinh thái của họ không
tạo ra được lợi thế cạnh tranh nào so với Android. HMS sẽ không thể lớn đủ để tạo thế
chân vạc với GMS và iOS, thị phần của họ vì vậy sẽ dần bị thu hẹp so với thậm chí là
chính các đối thủ tại quê nhà như Xiaomi.

Còn nếu như quá nhanh, ví dụ như ông nào xui bảo bỏ Android cho nó oai thì dù cho có
làm được thì rủi ro cũng quá lớn. Đây có thể coi là lựa chọn giữa chết luôn hay chết
từ từ. Còn nếu không muốn chết thì phải rất linh hoạt.

Riêng với Harmony OS, chiến lược của họ tôi thấy là khó mà có thể làm tốt hơn được.
"1 + 8 + N" sẽ giúp tránh được rủi ro tới mảng smart phone và ưu tiên mang tới thật sớm
những giá trị mới mẻ từ "8 + N". Bên cạnh đó, điều cần thiết là làm suy yếu ảnh hưởng
của Android mà HMS là bắt buộc nhưng vẫn chưa đủ.

Thành công của Android không thể thiếu sự ủng hộ của cộng đồng dev. Google rất hiểu
dev và tạo ra những công cụ tuyệt vời. Bài toán này theo một cách hiểu nào đó thì cũng
khá giống với legacy transformation. Huawei có thể bắt đầu với rehosting, tức là giữ
nguyên hiện trạng tầng ứng dụng để tận dụng lại những công cụ đó. Tương tự .NET core
dù có chạy trên Linux thì người ta vẫn dùng Visual Studio để dev chứ chả ai dở hơi mà
chuyển qua Eclipse. Nhưng họ sẽ cần những vũ khí riêng để thoát Google và "modernize"
sang bức tranh IoT của mình.
