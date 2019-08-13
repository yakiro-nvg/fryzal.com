+++
title = "Khai phá dữ liệu trên xe"
date = 2019-08-13
draft = false
authors = ["Nguyen, Giang (Yakiro)"]

tags = ["decision", "innovation"]
summary = "SEAMLESS, kiến trúc để vượt qua giới hạn"
+++

Bài trước: [Tương lai của ngành xe](https://fryzal.com/post/our-cockpit-vision/)

Trong bài trước, chúng ta đã văn học ra viễn cảnh về một thế giới đậm chất công nghệ. Khi
mà xe tự lái ra đời, cho phép hệ thống thông tin giải trí trên xe được nâng cấp lên một
tầm cao mới. Tuy vậy, cái gì xa vời quá thì nó thành viển vông.

**Thế cho nên là giờ ta phải tổ lái nó về với hiện thực.**

# Rào cản ở đây là gì?

Để vượt rào thành công thì phải nhìn thấy rào đã. Thời điểm cách đây khoảng 6 năm, khi tôi
mới chuyển ngành sang automotive thì cảm giác đầu tiên là như thể vừa du hành về thời kỳ đồ
đá vậy. Tại sao làm phần mềm cho mấy cái hệ thống giải trí trên xe lại không thể sướng như
làm cho điện thoại?

Sau đó tôi nhận ra cái cục mình đang làm là để phục vụ cho lái xe, đối tượng phải chịu trách
nhiệm về an toàn của cái xe khi xe đang chạy. Về lý thuyết, cái gì càng phức tạp thì càng kém
an toàn. Và hệ thống giải trí trên xe thì cũng vậy, đang lái xe thì mọi tương tác gây mất tập
trung đều bị cấm. Mà tốt nhất cái giao diện cũng càng xấu càng tốt, để lái xe họ chán dell
thèm nhìn luôn ý thì hoàn hảo.

{{< figure src="/img/our_cockpit_vision_2/buick.jpg" title="Đẹp quá nên chỉ là concept, General Motors / Buick" >}}

Vậy nên ta có những giới hạn về thiết kế. Nó không thể quá đẹp gây mất tập trung, cũng không
được quá phức tạp vì thế thì khỏi lấy chứng chỉ an toàn luôn. Mọi sáng tạo hầu như đi vào ngõ
cụt và các hãng xe thì chỉ còn nước ganh đua nhau về phần cứng kiểu chip khoẻ hơn, màn hình
to hơn, nét hơn, trình chiếu AR hay công nghệ AI etc.

Thỉnh thoảng ta thấy những phát kiến kiểu như thế này.

{{< figure src="/img/our_cockpit_vision_2/ar-adblock.jpg" title="Chặn biển quảng cáo bằng AR, <a href='https://wayray.com/sdk/challenge'>WayRay</a>" >}}

Mặc dù nó cũng sáng tạo, nhưng tôi cho rằng đây là minh chứng cho việc cạn kiệt ý tưởng, rảnh
quá không có việc gì khác để làm mới phải nghĩ ra cái này (j/k). Trong khi chờ đợi xe tự hành,
thứ có vẻ dài hạn thì ngay trong ngắn hạn ta vẫn cần một không gian để có thể triển khai dần
các ý tưởng về một hệ thống giải trí hiện đại. Vừa thêm được giá trị cho chiếc xe, lại vừa sẵn
sàng cho tương lai.

**Thành Rome không thể xây trong vòng một ngày.**

Sẽ ít khả năng xe tự hành ra đời, và ngay ngày hôm sau hệ thống giải trí trên xe nghiễm nhiên
được lên đời từ cục đá sang thành iPhone. Sự tiến hoá là một quá trình tích luỹ, đòi hỏi sự chuẩn
bị ngay từ hôm nay.

# Hệ thống mạng trên xe

Chưa có một tiêu chuẩn chính xác nào cho việc định danh các thế hệ của thiết kế mạng trên xe,
như tiêu chuẩn mạng viễn thông mà ta thường thấy (2G - 5G). Nên ở đây ta sẽ định danh nó một
cách err... đại khái thế.

## VN1 (vehicle network 1) - mạng cho xe thô sơ

Thời kỳ này thì chả có gì để nói vì xe cũng chưa phức tạp, như mạng điện trong nhà ý, vài cái
công tắc, vài con cảm biến, nối trực tiếp với nhau dạng Peer-to-Peer, và thông qua những giao
thức đơn giản kiểu có điện hay không có điện etc.

## VN2 - mạng broadcast với CAN, LIN etc.

Với số lượng ECU trở nên rất lớn trên xe và nhu cầu trao đổi dữ liệu phức tạp. Cách kết nối
kiểu Peer-to-Peer là không hiệu quả, vì ta không thể cứ mỗi khi phát sinh nhu cầu truy cập dữ
liệu từ ECU nọ sang ECU kia thì lại kiếm một sợi dây để nối chúng lại với nhau. Những chuẩn
kết nối dựa trên broadcast như CAN ra đời, dù có nhiều topology khác nhau nhưng bản chất chúng
là những mạng "ít dây". Các ECU trao đổi dữ liệu kiểu cứ đẩy lên bus, tất cả những ECU khác
trong mạng đều nhận được và tự quyết định xem có phải cái mình cần không.

{{< figure src="/img/our_cockpit_vision_2/gen-2.png" title="Mạng thế hệ thứ hai" >}}

**Thiếu cục OBD, cục này cắm vào tất cả các mạng trên xe, vẽ vào rối.**

Vì giới hạn tốc độ của giao thức cũng như thêm phần an toàn thì mạng trên xe sẽ được chia nhỏ
thành nhiều sub-network cho từng domain. Giữa các domain sẽ có một khối gọi là `gateway`, làm
trung gian để chia sẻ thông tin (message broker) và bảo vệ dữ liệu (firewall).

Với những domain có độ rủi ro cao do tiếp xúc với các nguồn không đảm bảo như Connectivity hay
Infotainment, và thường cũng không real-time thì khối gateway sẽ được tách thành một ECU riêng
chạy RTOS.

## VN3 - mạng với Central Gateway

Có thể các bạn đã thấy ở VN2 là gateway của khối Connectivity khá đặc biệt khi nó kết nối với
tất cả các mạng khác trên xe. Thực ra hồi đầu khi chưa có cục Telematic, tức là xe chưa có
internet thì thứ duy nhất giống kiểu vậy là cục OBD. Còn có cục Telematic vào thì hiểu đơn giản
là nó kiểu `OBD Over-The-Air`.

* Download, cài cắm firmware mới cho các ECU.
* Chuẩn đoán lỗi, thu thập thông tin đẩy lên cloud.
* Bật điều hoà bằng điện thoại khi chuẩn bị đi cho mát.
* Mất xe, tìm xe đi lạc, xoá dữ liệu nhạy cảm ⇨ AkaDayroi.

Chịu ảnh hưởng của IoT, người ta nhận thấy khối gateway này là nơi tuyệt vời để làm trung gian
truyền nhận dữ liệu giữa tất cả các domain trên xe và nhận làm những task không tên, không biết
cho vào đâu như Analytic, hay ứng dụng mang tính kịch bản kiểu đỗ xe ngoài bãi, trời mưa thì tự
kéo của kính lên chẳng hạn.

{{< figure src="/img/our_cockpit_vision_2/gen-3.png" title="Mạng thế hệ thứ ba" >}}

**Một số ECU chỉ sử dụng Ethernet, nhưng vẫn có đường CAN để wake-up.**

Trong mạng VN3, về cơ bản các gateway riêng cho từng domain sẽ biến mất, thay vào đó là một cục
Central Gateway cho tất cả các domain. Khối này cũng sẽ đảm nhận luôn nghiệp vụ của cục Telematic
truyền thống. Tức là giờ đây khối Connectivity trở nên rất "dumb".

Central Gateway cũng được thiết kế để chạy các ứng dụng cộng thêm, hiện thực hoá câu chuyện `car
as software platform`. Với phần cứng vượt trội và được tính toán dư địa để mở rộng về sau, hệt như
cách ta làm một cái điện thoại vậy. Phải khoẻ, phải thông minh và chạy ngon trong ít nhất là 3-5 năm.

Tuy nhiên, VN3 vẫn tồn tại nhược điểm ở chính tính tập trung. Nó tạo ra một điểm chết cho toàn hệ
thống (single point of failure). Tức là thằng Central Gateway này mà lăn ra chết thì thôi chết cả
lũ. Vì vậy một thiết kế chấp nhận được sẽ nằm đâu đó giữa VN2 và VN3, để có các kênh dự phòng cho
trường hợp Central Gateway dừng hoạt động, những tính năng an toàn cơ bản nhất vẫn chạy.

---

Hiện nay thì xe trên thị trường vẫn đang là VN2, VN3 thì được push nhiệt tình bởi Silicon Vendors
nên cũng sắp trình làng trong cỡ 1-2 năm nữa. Đến thời điểm đó ta có thể chứng kiến những chiếc xe
thực sự thông minh và giàu tính năng.

# Phía nam biên giới

Cuộc đối thoại trong quán cafe:

\- Cách đây đã lâu em đọc được về nó ở đâu đó, chắc là từ hồi em học cấp hai. Em không còn biết ở
trong cuốn sách nào nữa, chỉ nhớ chắc chắn đó là thứ bệnh mà nông dân làng F hay mắc phải. Anh cứ
tưởng tượng mình là một người nông dân và anh sống một mình trên thảo nguyên. Và ngày nào, ngày nào
anh cũng làm ruộng. Hoang mạc ngút tầm mắt. Không có gì, hoàn toàn không có gì ở xung quanh anh.
Phía Bắc, đường chân trời, phía Nam, đường chân trời, phía Đông, đường chân trời, và phía Tây, vẫn
là đường chân trời. Chỉ thế thôi. Mỗi sáng, khi mặt trời hiện ra phía trên đường chân trời phía Đông,
anh ra đồng làm việc, và khi mặt trời lên đến đỉnh, anh ngừng tay để ăn trưa. Khi mặt trời biến mất
sau đường chân trời phía Tây, anh về nhà đi ngủ.

\- Anh nghĩ cuộc sống đó rất khác với ở công ty mà ai cũng biết là công ty nào đấy.

\- Cái đó thì hẳn rồi, Ichini-san mỉm cười nói, rồi nàng hơi nghiêng đầu. Quả thật là rất khác. Và
ngày nào cũng thế, cả năm.

\- Nhưng vào mùa hè thì nóng lắm, không thể đi cày ở làng F được.

\- Tất nhiên rồi, mùa hè thì anh đi onsite. Anh làm việc ở những nước mát mẻ; rồi, khi mùa thu tới,
anh lại về làng F làm. Anh sống một cuộc sống như thế đấy. Anh cứ tưởng tượng anh là một nông dân ở
làng F đi.

\- Xong rồi, em kể tiếp đi.

\- Một ngày đẹp trời, trong sâu thẳm con người anh có cái gì đó chết đi.

\- Chết? Cái gì chết?

Ichini-san lắc đầu.

\- Em không biết. Cái gì đó. Cái gì đó gãy đi trong người anh và chết, khi mà suốt đời anh cứ nhìn
mặt trời hiện ra phía trên đường chân trời phía Đông, hoàn thành cái vòng cung di chuyển của nó và
đi ngủ sau đường chân trời phía Tây. Khi đó anh sẽ vứt bàn phím xuống đất, và không nghĩ ngợi gì nữa,
anh đi thẳng về phía Tây. Về phía Tây mặt trời. Và anh cứ đi như thế hàng ngày trời không ăn không
uống, như thể bị bỏ bùa, và cuối cùng anh gục xuống đất và chết. Chứng Hysteria F-ville là như thế đó.

\- Nhưng ở phía Tây mặt trời có gì? Tôi hỏi.

\- Cái đó thì em chịu. Có thể là không có gì. Có thể là có cái gì đó. Dù sao thì cũng rất khác với
phía Nam biên giới.

\- Thế ở phía Nam biên giới có gì?

\- Phía Nam biên giới là Mexico anh ạ.

Thôi vứt mẹ Mexico của em đi, đã dốt không làm được thì bảo luôn lại còn bày đặt văn học. Phía Nam
biên giới là cục MCU Gateway, đi xa hơn nữa ta gặp CAN bus và lấy được data em cần nhé. Thế rồi nàng
lăn ra khóc lóc thảm thiết, nhưng mà anh ơi em không biết lập trình MCU, lại cũng không biết đọc CAN
spec, ngày mai release rồi em biết phải làm sao, huhu.

# SEAMLESS architecture

Điều mà tôi rất không thích về kiến trúc của xe hiện nay là nó phân định rạch ròi thành nhiều domain,
trong mỗi domain lại chia làm nhiều ECU khiến cho việc khai thác dữ liệu trở nên rất khó khăn. Nhất là
khi bạn đứng ở góc độ một người lập trình ứng dụng thông thường, với hiểu biết hạn chế trong một vài
loại ECU.

Trong khi đó, tầm nhìn của chúng ta trong trung và dài hạn lại là `car as software platform`. Tức là
ai cũng có thể viết phầm mềm cho xe, như lập trình điện thoại bây giờ, chứ không giới hạn ở một nhóm
các công ty lớn. Tức là 2 cái trên đang mâu thuẫn với nhau.

## Companion app hell

Đội làm xe hay có cái phong cách thiết kế rất lởm là `companion app`. Giả dụ ta đang làm một cái
ứng dụng dẫn đường trên Cluster, mà lại cần lấy thông tin chỉ dẫn kiểu rẽ trái, rẽ phải, đi thẳng 100m
từ Head Unit. Vậy thì chia buồn, nó là 2 cục khác nhau, và ta phải tạo ra thêm một cái ứng dụng nữa
trên Head Unit để làm mỗi một trò rất ngu si là lấy dữ liệu để vứt cho cái ứng dụng trên Cluster.

Thôi, ngu tý cũng được, khổ cái là nó chưa dừng lại ở đấy. Hôm sau lại có một thằng nữa là thằng tự
hành, thằng này cũng có nhu cầu lấy chỉ dẫn lộ trình để biết đường mà lái. Thế là cháu nó cũng làm
ngay một cái ứng dụng nữa trên con Head Unit, tất nhiên là cộng thêm một số loại dữ liệu khác để ta
không thể dùng cái "cluster companion app" kia được. Đến đây hoặc là ta sửa lại cái ứng dụng trên Head
Unit và đổi tên nó thành "cluster and autonomous driving companion app". Hoặc là ta cứ thêm cái nữa
cho vui thôi. Cách thiết kế này được gọi là thiết kế hướng kiểu như l\*z thưa các bạn.

{{< figure src="https://media.giphy.com/media/l3V0slU7u7quJuZ3O/giphy.gif" >}}

Mà thôi, từ giờ không gọi nó là companion app nữa, mà phải gọi là `backdoor app`, vì nó có nghiệp vụ
vẹo gì đâu mà bày đặt companion. Thiết kế kiểu trên sẽ tạo ra rất nhiều backdoor trên xe. Quan trọng
nhất là không phải ai cũng đủ hiểu về hệ thống xe để đi viết backdoor, hoặc cứ cho là ta pro ta viết
được đi thì ta cũng rất có thể không được phép vì cái cục đấy là close source.

---

Tất nhiên cũng có trường hợp ta cần companion app thật, có nghiệp vụ đàng hoàng chứ không đơn thuần
chỉ là expose một cái gì đó ra. Giả dụ ta làm một cái ứng dụng vừa vẽ lên Head Unit lại vừa vẽ lên được
Cluster, xong trên mỗi cục lại tính toán cái gì đấy. Thì nó lại còn lởm hơn nữa thưa quý ông, về cơ bản
ta có hai cái ứng dụng phân tán với giao tiếp chồng chéo qua lại lẫn nhau.

**Trong khi lẽ ra ta chỉ cần 1 ứng dụng duy nhất.**

## Heterogeneous app framework

SEAMLESS là kiến trúc được tạo ra để nhằm giải quyết bài toán trên. Nhìn xa hơn chút thì hệ thống thông
tin giải trí trên xe dần dần sẽ gồm rất nhiều "cục" khác nhau chứ không đơn thuần là mỗi Head Unit với
Cluster nữa. Khi mà càng nhiều unit được thêm vào, và nhu cầu tương tác giữa chúng càng lớn, thì ranh
giới giữa chúng lại càng được xoá nhoà. Cuối cùng, ta không thể cứ mãi nhìn vào từng cục và bảo ta cần
một cái ứng dụng ở đây, một cái ứng dụng ở kia nữa. Cái ta nhìn thấy cuối cùng là một hệ thống, một hệ
thống SEAMLESS.

**SEAMLESS hiểu đơn giản là no-boundary.**

Đứng từ góc độ một người lập trình ứng dụng thông thường, ta nhìn thấy đây là cái xe, hết. Sẽ không còn
những câu hỏi baby kiểu "phía Nam biên giới là cái gì". Một heterogeneous app sẽ bao gồm nhiều component.
Component khác nhau có thể chạy trên các ECU khác nhau tuỳ vào nhu cầu. Việc cài cắm, update và đảm bảo
tính tương thích giữa các version của những component hợp thành app được thực hiện tự động. Vấn đề chia
sẻ dữ liệu thì được giải quyết bằng thiết kế micro service, cung cấp API thông qua heterogeneous binder.
Các tay mơ nếu có nhu cầu lấy thông tin gì thì chỉ cần có 2 thứ, đầu tiên là API, và thứ hai là quyền sử
dụng API đó. Chả cần quan tâm đến MCU hay CAN bus gì hết.

## Heterogeneous binder

Về binder thì có 2 bài toán thôi.

1. Chọn giao thức truyền dẫn hiệu quả.
2. Đảm bảo security cho API với permission.

Nói về security, ta cần định danh được ứng dụng đang gọi API là thằng nào, đã được grant những permission
gì. Thông tin này không thể tự ứng dụng đưa cho API được để tránh giả mạo. Với những OS truyền thống như
Android hay iOS thì điều này khá đơn giản, quyền của ứng dụng là read-only, được nhúng vào ROM và đưa cho
service thông qua kernel. Với hệ thống SEAMLESS thì câu chuyện phức tạp hơn một chút vì ta có rất nhiều
ECU chạy những OS khác nhau. Việc đảm bảo định danh chính xác một ứng dụng chạy trên ECU nọ trong khi nó
gọi một service ở ECU kia cần một cơ chế vượt trên mức OS.

### Phương án 1, dùng proxy

{{< figure src="/img/our_cockpit_vision_2/binder-over-proxy.png" >}}

Phương án này là gần nhất với hướng tiếp cận của binder trên Android. Ta dựng một con firewall giữa ECU
và gateway. Con đường duy nhất để đi ra ngoài của các ứng dụng trên ECU là chạy qua một cục proxy, được
cài đặt trên cùng OS nên có thể dùng những cơ chế của OS để định danh ứng dụng mà không thể giả mạo.

Lưu ý là "firewall" ở đây có tính minh hoạ, giao thức giữa ECU và gateway có thể không phải Ethernet, và
ECU cũng có thể quá dumb nên không có firewall. Mấu chốt ở đây là kết nối giữa ECU và gateway phải được
giới hạn sao cho chỉ đường đi qua proxy. Nếu kết nối đó là serial thì đơn giản là cấp quyền sao cho duy
nhất cái proxy được dùng thôi chẳng hạn.

**Ưu điểm:**

* Mì ăn liền, dễ hiểu, dễ implement.

**Nhược điểm:**

* OS không có cơ chế để proxy định danh app thì chịu.
* Data buộc phải route hết qua proxy ⇨ nặng.
	- Tuỳ trường hợp, thường data ko lớn.

### Phương án 2, dựa trên authentication

{{< figure src="/img/our_cockpit_vision_2/binder-auth-based.png" >}}

Ta đặt permissions database trên gateway, khi ứng dụng kết nối với gateway thì sẽ phải authenticate, hay
nói nôm na là đăng nhập để gateway định danh. Cách này khá giống với những web server truyền thống, nói
chung là chả biết anh từ đâu chui ra, con ông nọ, cháu bà kia gì đấy không quan tâm, muốn dùng dịch vụ
thì mời anh show token ra.

**Ưu điểm:**

* Portable, không cần OS hỗ trợ.
* Permissions được lưu tập trung ở gateway.
	- Dễ update, revoke, grant thêm quyền nếu cần.
	- Attack surface nhỏ hơn nên có vẻ an toàn hơn tý.

**Nhược điểm:**

* Single point of failure.
* Yêu cầu phải là mạng VN3.

### Phương án 3, các biến thể khác

Tuỳ vào các ràng buộc khác nhau mà hệ thống có thể kết hợp cả phương án (1) lẫn phương án (2), vì thực
ra chúng nó cũng không phải loại trừ lẫn nhau. Ngoài ra thì cũng có nhiều biến thể khác cho phù hợp với
thực tế, ví dụ như thay vì data route qua proxy thì ta có thể cho phép gateway pull permission database
từ trusted process trên ECU chẳng hạn. Miễn sao API đưa ra cho người lập trình ứng dụng vẫn đồng nhất.
Tức là từ góc độ của họ thì bên dưới cơ chế security vận hành thế nào họ không cần quan tâm.

**Đừng có tự dưng đổi mẹ cái API đi là được.**

# Fun Facts

## Bài viết có đạo tý văn của Murakami

Đã cảm ơn và tín dụng bằng cách mua sách, không đọc chùa nhé.

## SEAMLESS là một dòng dự án

Ngoài việc được đặt cho tên kiến trúc heterogeneous app, nó còn được dùng làm tên dự án Cockpit thế hệ
mới ở công ty F. Lúc đầu vì uống nhiều thuốc của các anh Silicon Vendors quá nên định đặt là `hyper
application framework`. Nhưng sau nhận ra là mặc dù Hypervisor là platform tuyệt vời để làm mấy trò này,
nhưng thực tế nó không couple vào Hypervisor. Tức là ta vẫn có thể làm heterogeneous app mà không cần
đến Hypervisor nên thôi đổi tên cho đỡ hiểu nhầm.