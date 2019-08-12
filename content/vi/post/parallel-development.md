+++
title = "Phát triển song song"
date = 2019-07-28
draft = false
authors = ["Nguyen, Giang (Yakiro)"]

tags = ["architecture", "productivity"]
summary = "Đừng miss deadline vì chờ đợi!"
+++

Đây là bài đầu tiên có bàn đến **năng suất**, chủ đề nóng ở công ty F.

Sống ở trên đời, ai mà không miss deadline? Đồng ý, nhưng đôi khi lý do nó ntn:

* Thằng làm GUI chưa xong, không integration test được.
	- Spec ngu, đang confirm.
	- Khó ramp-up, hội sinh viên nó đang nghỉ mát.
	- Trời nóng, một ngày cháy nhà 50 lần.
* Vừa nhận được Service IF tối qua, ship tận guest house.
* JIG mới dùng 2 năm đã tèo, dell làm được GUI.
* Chưa có JIG, đang nằm ở hải quan.
	- CFO khách tên Mohamad, trong black-list.
	- Công ty F tài trợ khủng bố ah?
* etc.

Đại khái là cái gì mà ta không chịu trách nhiệm, khách quan.

{{< figure src="/img/parallel_development/salt_bae.gif" title="Phải thật là tinh tế" >}}

Vấn đề nằm ở đâu?

# Hạn chế Waterfall thôi

Waterfall là quy trình phát triển truyền thống trong đó chia công việc thành nhiều bước
theo thứ tự phải làm, rất phù hợp để làm bánh mỳ hay nấu phở nếu một mai người ta không
còn làm phần mềm nữa.

{{< figure src="/img/parallel_development/waterfall_right.png" title="Waterfall ở trường" >}}

Hợp lý quá, dù sao cũng chỉ có một người làm.

{{< figure src="/img/parallel_development/waterfall_left.png" title="Waterfall ngoài xã hội" >}}

Bắt đầu thấy hip hop.

{{< figure src="/img/parallel_development/waterfall_so_left.png" title="Waterfall ở công ty F" >}}

---

Nói chung tinh thần làm việc tốt là không đổ lỗi cho người khác, tức là thằng nào sida kệ
cmn, việc của mình thì cứ phải nghĩ cách mà làm cho xong. Dù sao đến lúc chết dự án thì khách
hàng cũng chả quan tâm lỗi của thằng nào.

Những thứ cần hướng tới:

* Dev và test app mà không cần đợi GUI.
	- Integration test khi chưa có GUI.
* Dev và test app bằng GUI dạng prototype.
	- GUI này PHẢI chạy trên PC của developer.
	- **Chạy trên JIG là ăn l\*z ngay**.
	- Fast food stack: Qt, WPF, React etc.
* Dev và test app khi chưa có service.
	- Stub service PHẢI chạy trên PC của developer.
	- Fast food stack: Node, .NET, Python etc.
	- Integration test khi chưa có service.
* Dev và test GUI khi chưa có app.
	- Test màn hình bất kỳ, không quan tâm transitions.
	- Integration test khi chưa có app.
* Bước tích hợp cuối cùng effort bỏ ra là tối thiểu.
	- Làm sao chạy luôn là ngon nhất.

**Cuối cùng và quan trọng nhất, làm gì thì làm, chi phí phải thấp hơn.**

# MVC vs MVVM

Bài toán đặt ra thì khá là mênh mông, nhưng nếu chỉ nói đến phát triển app thì điều kiện
tiên quyết là kiến trúc phần app phải tách được tầng nghiệp vụ ra khỏi tầng giao diện. Lý
do thì có nhiều nhưng ở đây nó giúp 2 thằng này có thể phát triển song song mà không phụ
thuộc hay chờ đợi lẫn nhau.

Tức là lại quay về cái máng MVC vs MVVM.

{{< figure src="/img/parallel_development/mvc.png" title="Model - View - Controller" >}}

Ối giời ơi, View vs Controller dính vào nhau thế kia thì bảo sao.

{{< figure src="/img/parallel_development/mvvm.png" title="Model - View - ViewModel" >}}

Đấy, nó phải như thế này, View và Controller không còn phụ thuộc vào nhau nữa mà ở giữa là
ViewModel. Thông qua cơ chế `binding` thần thánh khiến cho việc phát triển app trở nên chân
thực hơn bao giờ hết, giờ mà đập cục View đi thì dễ như ăn kẹo. Nhưng mà khoan đã, cho phép
nhìn lại cái.

{{< figure src="/img/parallel_development/mvc_mvvm.png" title="Khác nhau chỗ nào?" >}}

Khi HMI spec thay đổi thì:

* Actions / values cũng thay đổi.
* ViewModel cũng thay đổi nốt.
* Làm thế quái nào Controller giữ nguyên?

---

Thú thật là chả có cái pattern nào mà tôi thấy hay bị hiểu nhầm / dùng nhầm như MVVM. Khi
nghĩ về MVVM người ta hay nghĩ đến mấy cái đồ chơi kiểu `binding`, thứ hoàn toàn không phải
cốt lõi của MVVM mà phải là decoupling.

{{< figure src="/img/parallel_development/mvc_mvvm_renamed.png" title="Đặt lại tên cho đỡ nhầm" >}}

Điều khiến bao nhiêu anh tài phải ôm hận là 2 thằng MVC và MVVM nó lại có vẻ tương tự. Cách
đặt tên khá giống nhau khiến cho người ta hay có xu hướng ngộ nhận về bản chất của các khối,
và thường xuyên thay nó bằng cái gì đó đã quen thuộc từ MVC. Cái này phải sửa, `user actions`
và `HMI values` là thứ phụ thuộc vào HMI spec, trong khi `VM actions` và `VM values` thì không.

{{< figure src="/img/parallel_development/mvc_mvvm_state_diff.png" title="Khác biệt giữa `VM values` và `user/HMI values`" >}}

Tức là phần View của MVVM phức tạp hơn của MVC.

{{< figure src="/img/parallel_development/mvvm_reloaded.png" title="Thực ra View của MVVM nó ntn" >}}

View của MVVM có thêm khối Code Behind, và hoàn toàn chủ động trong việc hiển thị, từ nhận data,
convert sang các giá trị tương ứng để hiển thị cho đến kích hoạt các action. Controller ở đây hoàn
toàn không biết gì về View, và không tự set cái gì vào View như trong MVC nhé.

---

Đến đây chắc không ai thắc mắc vì sao MVVM decouple được View?

Một vài vấn đề của MVVM:

* Hiểu sai, dùng sai như cơm bữa.
	- Cái này chịu, đọc báo nhiều vào.
* Design một cái ViewModel tốt từ đầu không dễ.
	- Cần review kỹ từ các senior / expert.
	- Cực kỳ quan trọng, cái này mà sai là đứt.
* Độ phức tạp của View cao hơn vì thêm cục Code Behind.
	- Cũng không hẳn, thực ra là chuyển từ Controller sang thôi.
	- Nhưng khiến cho người làm View cần kỹ năng cao hơn.
	- Có thể dễ hơn nếu HMI framework / tool ngon.
* Chưa đả động gì đến quản lý state.
	- Đang nói về `state`, không phải `state machine` nhé.
	- State được phân bố rải rác ở nhiều nơi.
	- Thường hay đi kèm với two-way binding hell.
	- Khó phán đoán, khó tái hiện, khó debug etc.

# Redux

Một món ăn chơi mới được phát minh bởi các functional programming fan-[boy]s.

Về Redux thì các vị anh hùng có thể tìm đọc thêm trên internet, nhớ chọn lọc nhá.

**Nếu đọc phần sau và thấy bấn loạn vì chả hiểu gì:**

* Google về Redux, đọc tiếp.
	- Làm khoá Erlang xong hiểu luôn (j/k).
	- Sorry, dạy Redux ngoài phạm vi bài viết.
* Ngoài ra thì có thể PM tôi, nhưng.
	- Tác giả hiện không ở VN.
	- Jet-lag, mạng mẽo giới hạn.
	- Tự kỷ không đọc tin nhắn etc.

## Keypoint của Redux

{{< figure src="/img/parallel_development/redux.png" title="Reduce, reduce, reduce..." >}}

* HMI View được luận ra từ State.
	- Giống kiểu từ ViewModel ý.
* State phải **IMMUTABLE**.
	- State được reduce từ Actions.
	- State mới là hệ quả của một Action mới.
	- View không chọc vào sửa State như MVVM nhá.
* Có thêm middleware để filter các Actions.
	- Side-effects thì cho vào đây.
	- Redux thunk, redux saga các kiểu.
* Action và State có thể được serialize thành data.
	- Thường dùng luôn JSON luôn cho nó tiện.
	- Đưa cho thằng khác được (process khác, lên server etc).
	- Deterministic (để EN google cho dễ).
		+ Chuỗi Action giống nhau cho ra State giống nhau.
		+ Tất nhiên là dính vào side-effects thì chịu.
		+ Log lại các Action là tái hiện ngon.
* Data chảy về cùng một hướng.
	- Tốt cho mắt, không thành Ninja loạn thị bm.
	- Dễ hiểu, nhìn phát hiểu luôn?

Thôi ngắn gọn là nó ntn:

* Không có giao tiếp lằng nhằng.
* Không có đường tắt để thay đổi State.
	- Đi tắt sau debug vỡ thớt.
* State không bị rải rác mỗi chỗ một tý.
	- Rải rác debug cũng vỡ thớt nốt.

{{< figure src="/img/parallel_development/messy_communication.png" title="Xin các anh đừng như vậy nữa!" >}}

# Redux ở công ty F

Nếu bạn đang làm web, hoặc không thì tệ nhất cũng là mobile thì xin chúc mừng, bạn có thể
lướt qua mục này luôn. Tuy nhiên nếu bạn mới từ trên núi xuống, bạn không thích Javascript,
hay thậm chí bạn đang làm ở công ty F. Tóm lại là bạn muốn dùng Redux cho một cái hệ thống
với chi phí phát triển hàng trăm triệu mỹ kim mà tình trạng là đến dùng đúng MVVM cũng có
thể coi là một bước nhảy vọt của nhân loại thì ... mời đọc tiếp.

---

Tiền đề là giữ nguyên phần HMI, ta cần một kiến trúc để adapt nó theo Redux.

Kiến trúc COMPASS được xây dựng nhằm mục đích ứng dụng Redux vào làm Automotive trên những
hệ thống được xây dựng từ thời Napoleon. COMPASS là viết tắt của Controller, Object, Model,
Presenter, Arbitrator, Service, System là cách hiểu không đúng. Thực ra đơn giản là ta lấy
cục HMI làm trung tâm sau đó đặt tên các khối còn lại theo phương hướng thôi. Về cơ bản là
ta sẽ tái cấu trúc lại các khối hiện tại dựa theo COMPASS, và thêm mẹ nó mấy cái keyword
kiểu South, North, Middle East etc. vào là xong. Một số khách hàng rất thích điều này vì nó
cho thấy tính kế thừa ngay từ khâu đặt tên, kiểu JohnSouthController, MariaNorthController etc. 

{{< figure src="/img/parallel_development/compass.png" title="COMPASS architecture" >}}

Về cơ bản, ta wrap phần HMI cũ trong một cái Container (term của Redux). Như vậy khi nhìn
từ ngoài vào thì ta không còn thấy cục HMI đấy nữa và chỉ thấy một cái View đúng chuẩn Redux
là nhận vào State, hiển thị GUI và dispatch các Action. Container này gồm 2 khối là South và
North, đến đây các vị anh tài có thể dừng lại và map luôn sang hệ thống mình đang làm xem cái
nào là South, cái nào là North, thậm chí vừa South vừa North nhá.

* South Controllers.
	- Input: State (được reduce từ Action).
	- Output: Form, on-screen etc.
* North Controllers.
	- Input: HMI events.
	- Output: Actions.

Theo Redux nên phần Container này nên là pure-function nhé.

## View state thì để ở đâu?

Lấy ví dụ ta có một cái app chơi nhạc kiểu ntn.

{{< figure src="/img/parallel_development/view_state_usecase.png" >}}

Về tính năng, ta có thể chuyển qua lại giữa 2 màn hình `Song List` và `Now Playing`. Đang xem
Song List thì vẫn nghe nhạc bình thường nhé. Tức là `now playing song` và `selected song` là
hai thuộc tính khác nhau. now playing song thì tất nhiên là State, nhưng selected song thì có
phải State không? Ngoài ra, nếu user nó bấm `delete` đúng ngay cái bài đang được chọn, tức là
selected song không còn đúng nữa thì code giải quyết vụ đấy để ở đâu?

Đối chiếu sang MVVM, nếu coi State là ViewModel thì selected song không phải State. Nên nhớ là
nghiệp vụ của cái app này là chơi nhạc, tức là chọn bài, phát tiếng, xoá bài chẳng hạn. Còn
chuyện hiển thị giao diện ra cho user nó xem kiểu gì không liên quan, thậm chí ta còn không chắc
được là có tồn tại một cái List View hay không, vì đấy là phần hiển thị. Suy ra là ta cũng không
biết có cái selected song nốt. Nếu cho selected song vào State thì nó sẽ bị couple vào HMI spec,
hệ quả là sau HMI spec thay đổi thì rủi ro phải đập thêm một số thứ.

Giải pháp là đặt View state ở South Controllers.

{{< figure src="/img/parallel_development/compass_view_state.png" >}}

Về cơ bản, ta thêm một Redux store khác được dùng nội bộ bởi South Controller để chứa View state,
từ ngoài nhìn vào thì vẫn như cũ thôi. Store này nhận vào một Action `NEW_APP_STATE`, bên trong
chứa State mới của phần nghiệp vụ. Như vậy tại South Reducer ta có cả `app state` lẫn `view state`
từ đó reduce ra được view state mới. Những logic xử lý như view state cũ không phản ánh đúng với
app state mới cũng nằm ở đây luôn (dangling selected song). Khi HMI spec thay đổi ta chỉ sửa lại
cục South, không ảnh hưởng đến phần còn lại.

<iframe height="600px" width="100%" src="https://repl.it/@yakironvg/paralleldevhmistore?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

South Controller đã trở thành stateful, tuy nhiên cũng không tệ lắm:

* HMI state chỉ lưu hành nội bộ trong South Controller.
* Không có side-effects, hoạt động của khối South vẫn dự đoán được.
* Data vẫn chảy về cùng một hướng, vẫn là Redux.

**Ngoài ra ta có thể lưu lại South Store để khôi phục View sau khi bị cắt điện.**

# Sword of the Far East

*SOTFE là tên một ban nhạc JP bé bé chơi violon mà tôi khá thích.*

Chủ đề của ngày hôm nay là phát triển song song, và lý do tôi nhắc đến Redux là vì nó cực kỳ thích
hợp cho vấn đề này. Khi làm phần mềm cho những hệ thống nhúng hoặc suýt-nhúng thì sẽ có 2 môi trường
là target (/ board, device etc.) và host (PC), đại loại là phát triển chéo. Và hầu hết chúng ta đều
ít nhất một lần ước rằng làm với board cũng sướng như làm với PC. Để có thể phát triển song song được
thì cần thiết phải tạo ra nhất nhiều phần code / tool hỗ trợ, và con board của chúng ta thì không thích
điều này.

{{< figure src="/img/parallel_development/parallel_war.png" title="Parallel War" >}}

Văn học mà nói thì nó là một cuộc chiến giữa Đế Chế Cận Đông (đã cận thị lại còn đông) và Đế Chế Viễn
Đông (viễn thị và cũng sắp đông nốt). Trong COMPASS ta gọi khối PC là `Far East`, tức là cứ cái gì có
chữ Far East vào thì ta hiểu là nó sẽ chạy trên PC hoặc hỗ trợ các khối chạy trên PC.

Là một lập trình viên trung kiên của Đế Chế Viễn Đông, tất nhiên ta không dại gì mà phi vào đánh nhau
ở khu Trung Đông, nơi đóng chốt của những tên trùm như Arbitrator, Mode, State Machine etc. Danh dự
của những người kế thừa chủ nghĩa fast food không cho phép chúng ta làm như vậy.

{{< figure src="/img/parallel_development/sotfe.png" title="Chạm vào app lịch sự như một quý ông" >}}

Thay vào đó, ta chỉ quan tâm đến Action và State, dựa trên `Store Bridge`. Nhiệm vụ của khối này là
xuất ra 2 web API để can thiệp vào app. Post một Action mới và theo dõi State Stream. Với 2 API này ta
có thể tạo ra được rất nhiều tool phục vụ phát triển song song như prototype HMI hoặc stub service.

## Nghe có vẻ tốn công sức nhỉ?

Hoàn toàn không, ở công ty F thì Redux Store của app được định nghĩa bằng một ngôn ngữ đặc tả đơn giản
là UBDL, ngôn ngữ này sẽ định nghĩa State và các Action mà app có. Tức là khi dùng UBDL ta sẽ có 2 API
trên để làm tool trên PC một cách miễn phí, không tốn thêm tý effort nào.

{{< figure src="/img/parallel_development/ubdl.png" title="Unbindable description language" >}}

**Source**: https://github.com/yakiro-nvg/unbindable

# Fun Facts

## UBDL lúc đầu thiết kế cho MVVM

Đại khái nhu cầu cần một ngôn ngữ đặc tả, pattern hướng đến là MVVM, nên đặt tên nó là Universal Bindable
Description Language. Sau đó vì một số lý do chuyển sang Redux và đổi lại tên thành Unbindable Description
Language, vẫn là UBDL đỡ phải sửa lại package name, tuyệt vời.

## COMPASS là tên một dòng dự án R&D

COMPASS được đặt cho pattern liên quan đến phát triển song song như các vị anh hùng đã thấy ở trên, tuy
nhiên nó còn được đặt cho dòng dự án nghiên cứu về phát triển song song tại công ty F. Nên nếu ai có tình
cờ nghe thấy `Project COMPASS` thì tức là đang nói về nó, không phải cái pattern đơn giản vãi lúa trên.

## Có cái gọi là Ban Công Nghiệp

Công Nghệ vs Công Nghiệp ở chỗ tôi có nghĩa khác nhau. Công Nghệ là đi giải quyết mấy bài toán cách mạng
kiểu xe tự hành, blockchain etc. Đại khái cứ cái gì đang hot trend trên báo thì quy về Công Nghệ. Còn Công
Nghiệp là giải quyết mấy bài toán kiểu phát triển hệ thống to vãi chưởng cả về quy mô lẫn chi phí. Có thể
đã chạy từ 20 năm trước và xác định chạy tiếp 20 năm sau. Mấy bài toán kiểu COMPASS là thuộc Ban Công Nghiệp,
còn Ban Công Nghệ đang đi làm cái khác, bao giờ có thời gian chia sẻ vs các vị anh hùng sau.