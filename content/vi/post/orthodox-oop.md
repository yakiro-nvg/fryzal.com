+++
title = "OOP chính thống giáo"
date = 2020-08-15
draft = false
authors = ["Nguyen, Giang (G. Yakiro)"]

tags = ["architecture", "COBOL"]
summary = "Bạn đã thực sự hiểu về lập trình \"hướng đối tượng\"?"
+++

Nếu ai hay theo dõi những nội dung mà tôi viết thì chắc cũng biết tôi có máu anti-OOP.
Và điều đó cũng thường xuyên làm phật lòng nhiều người :-) Bên cạnh đó tôi cũng thường
xuyên lấy Java làm phản ví dụ cho những thứ tốt đẹp. Em xin lỗi các anh Java fanboy,
nhưng mà nhìn các anh code chán vloz.

Vì Java tôi ghét OOP hay vì OOP tôi ghét Java? chịu.

Hôm nay đẹp ngày đổi gió thử khen OOP phát.

*(hôm khác xấu ngày khen Java sau).*

# Hướng đối tượng hiện đại

OOP hiện đại có 9 tính chất cơ bản, đây là con số rất phong thủy.

Theo quan niệm xưa, số lẻ thuộc hành dương, số chẵn thuộc hành âm. Trong đó, `9` là
số lớn nhất trong dãy số dương nên còn được gọi là cực dương và chứa đựng nhiều ý nghĩa
tốt lành.

## 1. Tính đóng gói (encapsulation)

```C#
class Meo
{
    void Meow();
}
```

Method trong OOP được gói giữa 2 dấu ngoặc nhọn, đây là ngôn ngữ có tính đóng gói cao. Trái
lại một số ngôn ngữ như python thì ít đóng gói hơn vì dùng tab và space, có thể gây lỗi biên
dịch.

## 2. Tính đa hình (polymorphism)

```C#
class Meo extends Dog
{
    void Idle()
    {
        Say("Meow\n");
    }
}
```

Thiên địa bất nhân, dĩ vạn vật vi sô cẩu. Ok, một số bạn có thể bảo là tôi dùng sai, ở trường
dạy các bạn cả hai con đều là đa hình của class "động vật". Đây là biểu hiện của thiếu kinh
nghiệm thực tế, họ dạy bạn thế để bạn còn chịu học OOP thôi.

Theo hệ tư tưởng X, thì vạn vật đều do Đạo mà sinh ra, mà tôi hỏi ở đây đã ông nào nhìn thấy
Đạo chưa? Có gì đảm bảo con mèo không phải là đa hình của chó, tức là một loại chó đặc biệt!?

https://fryzal.com/post/why-not-oop/

## 3. Tính trừu tượng (abstraction)

```C#
class Meo extends Dog
{
    override void Idle()
    {
        _sayable.Say("Meow\n");
    }
}

class YouDontSay implements ISayable
{
public:
    void Say(string what)
    {
        if (isHateSpeech(what)) {
            what; // doesn't kill you makes you hurt
        } else {
            // TODO: it's complex, ...
        }
    }
}
```

## 4. Tính kế thừa (encapsulation)
## 5. Tính kế thừa (encapsulation)
## 6. Tính kế thừa (encapsulation)
## 7. Tính kế thừa (encapsulation)
## 8. Tính kế thừa (encapsulation)
## 9. Tính kế thừa (encapsulation)

# Ngược theo dòng lịch sử

Đoạn trên đùa tý, 4 tính chất của OOP ông nào đi phỏng vấn xin việc chả biết rồi.

Người tiên phong trong việc phổ cập khái niệm OOP là Alan Kay, cũng là người được trao
giải Turing cho thành tựu này. Khi tôi biết bố đẻ của OOP là Alan Kay, đó thực sự là một
cú shock nhẹ. Điều đó thôi thúc tôi tìm hiểu hơn về OOP, để biết nguyên nhân vì sao một nhà
khoa học như Alan, ý tôi có tên là Alan lại phát minh ra thứ mà tôi ghét.

Những người có tên là Alan tôi biết đều là những người tiên phong và có tầm ảnh hưởng với
những phát minh tuyệt vời. Ví dụ như Alan Turing, Alan Walker hay Alan Musk. Tôi tin rằng
Alan Kay cũng không ngoại lệ, phải có gì đó nhầm lẫn ở đây, chả lẽ là lại nhớ nhầm tên?

## OOP có thực sự tự nhiên?

OOP được biết đến với tính dễ hiểu, rằng nó là cách tự nhiên nhất để mô hình hóa thế giới.
Nói cách khác nó map thẳng sang thế giới thực. Ví dụ ngoài đời ta có con mèo thì trong OOP
ta cũng có đối tượng con mèo. Con mèo có thể chạy, nhảy, bay, kêu meow meow. Ta có đối tượng
ngôi nhà, trong ngôi nhà ta thả vào đó hai con mèo.

```C#
Cat a = new Cat("black")
Cat b = new Cat("blue")
Home home = new Home()
home.add(a); home.add(b)
```

Sau đó ta cho hai con mèo chạy quanh ngôi nhà.

```C#
a.runAround()
b.runAround()
```

Implementation của hàm runAround.

```C#
void runAround()
{
    while (!IsHungry) {
        // thinking where to move
        // while not at the destination, move
    }
}
```

Sau đó bạn nộp bài và thầy giáo cho bạn điểm zero vì chỉ có một con mèo hoạt động. Bạn đi tìm
sư huynh để xin hướng dẫn, sư huynh bảo bạn dùng thread. Tất nhiên có ông bảo không dùng thread
vì overhead. Thay vào đó nên cho con a chạy tý rồi lại cho con b chạy tý, cứ thế lặp lại. Cái đấy
là scheduling, và bản chất là bạn vẫn đang có 2 thread, tự schedule lấy thôi.

Thì câu hỏi ở đây, nếu nói OOP là mô hình của thế giới thực, vậy thì ở thế giới thực thread là
cái quái gì? Không, ngoài đời không có thread đâu bạn ạ, thread là khái niệm do bọn làm hardware
với OS nó tạo ra.

*Có ông bảo thread là object :-) thế thì tôi cũng chịu.*

# Real-OOP by Alan Kay

> OOP to me means only messaging, local retention, and protection and hiding of state-process,
> and extreme late-binding.
>
> -- <cite>Alan Kay</cite>

## Extreme late-binding

Late binding hay còn gọi là dynamic binding, là cơ chế để object quyết định xem nên gọi method
nào khi chương trình đang chạy (runtime). Late binding được thể hiện trong C++ bởi virtual method
table (vtable).

Phát biểu của Alan Kay có nói đến late binding, nhưng nhấn mạnh là `extreme` late-binding. Tức là
không phải late binding như phần lớn ngôn ngữ hiện nay. Trong rất nhiều những ngôn ngữ tôi từng
sử dụng thì có lẽ chỉ một vài cái tên đạt được. 

```objectivec
@interface Cat
- (void) feedWithFood:(Food*)food;
@end
```

Objective-C là một ví dụ, khai báo trên nói rằng Cat sẽ nhận vào message "feed", với tham số "food".
Với C++, thì compiler sẽ biết luôn khi compile "feed" là cái gì, nằm ở đâu và runtime chỉ việc gọi.
Với Objective-C, bạn thực sự tạo và gửi đi một message, và Cat sẽ quyết định khi nhận được message
như vậy thì phải làm gì.

```objectivec
[cat feedWithFood:fish]
```

Câu lệnh trên sẽ gửi message "feed" kèm con cá. Đây không phải là lời gọi hàm, việc gọi hàm chỉ là
hệ quả khi bạn gửi message đó đi thôi. Bạn hoàn toàn có thể gửi một message mà Cat không hiểu, và
chương trình vẫn compile bình thường, nhưng sẽ chết ở runtime.

Ok, vậy late-binding thì có gì hay? Tại sao lại chết ở runtime trong khi với C++ thì nó sẽ báo lỗi
luôn để còn biết mà sửa lúc compile? Ở đây có hai vấn đề:

### Trade-off giữa dynamic và static

Đôi khi cái gì kêu như con vịt thì coi như là con vịt. Không phải mọi thứ đều biết được khi chương
trình chưa chạy. Có thể object sẽ react lại message sau khi truy cập vào database và đọc ra cái gì
đó. Hoặc giả dụ bạn chơi game, chạy lòng vòng và bắn ra đạn, viên đạn trúng người sẽ trừ máu, trúng
cái thùng thì thùng sẽ nổ, còn trúng cửa thì không có gì xảy ra, cửa không react lại message này.

Không phủ nhận lợi ích của static typing, nhưng adaptability và resilence to change của nó không tốt,
sẽ nói thêm về vấn đề này ở phần sau. Thực tế cho dù dùng cái gì thì cũng ít nhất một vài lần bạn sẽ
thấy dynamic là tốt hơn. Service discovery trong SoA là dynamic chẳng hạn. Nó có thể lỗi, nhưng vẫn ok.

### Object được chọn cách react với message

Đây là vấn đề quan trọng hơn, với Java hay C++, khi bạn gọi một hàm hay đọc một property tức là bạn
đang buộc object phải phục vụ bạn ngay lập tức một cách rất bất lịch sự. Bạn ném cá cho con mèo lúc
nó đang ngủ?

Cách nói khác thì đây là khác biệt giữa push và pull. Object nhỏ kiểu con chó con mèo thì có thể ta
khó nhìn ra được vấn đề. Giữa hai service với nhau thì dễ hình dung hơn. Thông thường bạn sẽ không gọi
hàm của một service. Bởi vì đó là bạn đang push nó phải xử lý request của bạn, trong khi có thể nó
đang trong trạng thái không thể xử lý được. Nếu ngon thì nó sẽ phải queue request đó lại và bảo là
không thấy tao đang bận à, cầm cái ticket này ra kia sếp hàng đi tý tao gọi. Còn không bạn có thể làm
corrupt state và gây ra lỗi.

Tức là về bản chất bạn gửi message, và nó thì late binding, có cái ở đây là bạn phải tự implement, và
không phải là mô hình mặc định. Object sẽ chọn thời điểm phù hợp để pull request về và xử lý khi nó
có thể xử lý được. Hoặc batch nhiều request và xử lý cùng một lúc để có performance tốt nhất chẳng hạn.
Tóm lại nó được chọn, bạn chỉ request bằng message thôi, đúng như cuộc sống. Hãy sms một cách lịch sự,
đừng hiếp dâm.

> I'm sorry that I long ago coined the term "objects" for this topic because it gets many people to
> focus on the lesser idea. The big idea is "messaging"
>
> -- <cite>Alan Kay</cite>

Ngoài ra, bằng việc gửi message, object có thể forward nó, ví dụ lên class cha, hoặc lên server. Bạn
gửi và nó chọn, dù lựa chọn là gì thì cách giao tiếp ở đây vẫn không thay đổi. RPC? no difference.

*Android Service là case study về late binding.*

*Có ông bảo Android vẫn gọi hàm bình thường? hàm đây là của cái proxy!*

## Local retention, state hiding

Bạn gọi một hàm cho con mèo ăn, vì đang ngủ nên nó queue lại và chưa ăn. Thế xong rồi bạn hỏi nó đã no
chưa. À không, bạn get property là mày đã no chưa? Và nhận kết quả là chưa no, bạn lại vứt cho nó con
cá nữa.

State của object phải được bảo vệ và giấu thật kỹ. Để biết con mèo đã no hay chưa, hãy gửi message cho
nó, nó chưa trả lời là chưa có kết quả, không phải là nó đã no rồi :) Tức cái getter ở đây là message.

State protection cho phép một thứ hay hơn nữa, vì bạn không phi thẳng vào state của object nên ở đây
chỉ có duy nhất một mutator (là chính cái object đấy). Tức là sao? tức là khi truy cập vào state ta
không cần lock. Mô hình trên được gọi là actor model, nó cho phép lập trình concurrent một cách đơn giản.

{{< figure src="/img/orthodox_oop/actors.png" >}}

Nghĩ về một con server với 16 cores, các actor sẽ được schedule chạy trên core bất kỳ mà vẫn hoàn toàn
transparent với phương pháp lập trình. Scheduler có thể tự balance các actor lên những core khác nhau
dựa trên tải CPU, hoặc thậm chí balance giữa các node trong mạng distributed.

https://www.brianstorti.com/the-actor-model/

# OOP trong COBOL

Từ 2002 COBOL standard bắt đầu được thêm OOP, tức là khá muộn.

```CBL
invoke cat "feedWithFood" using fish
```

COBOL có khái niệm method, nhưng thực ra vẫn bắn message đi như thường, khá giống Obj-C. Overhead hơn
là dùng string, không phải selector. Khoản late binding thì chưa rõ lắm vì tôi cũng chưa được sờ tận
tay vào ISO của nó. Nhưng cho phép bắn string đi thì chắc ít nhất cũng ngang Obj-C.

## OOP trong Fpt COBOL (tên project: OCP)

Như [bài viết trước](https://fryzal.com/post/garbage-collector-for-business/) nói về quản lý bộ nhớ tự
động thì tôi có nói đến hướng phát triển GC của COBOL theo actor model, tương tự như Erlang (hướng 2).
Thực ra vấn đề ở đây không chỉ là GC, mà còn do cách schedule (non-blocking I/O proposal), và do tôi
cũng chưa thấy ok lắm với COBOL standard.

Thực tế theo tôi biết thì OOP của COBOL cũng chưa phổ biến, và các vendor cũng implement rất fragment
theo kiểu mạnh ai nấy chạy, có ông còn chả có GC :-) Vì vậy với những bài toán migration hay modernization
thông thường thì khả năng chắc còn chả dùng đến OOP.

Tuy nhiên với thiết kế GC theo actor model thì constraint là phải decomposite hệ thống theo actor model.
Heap to sẽ làm tăng latency do GC, và cũng có thể ảnh hướng đến scheduler do tối ưu cho nhiều actor chạy với
latency thấp, chứ không phải một vài actor to và chạy lâu. Cái này tất nhiên config được thì tốt hơn nhưng
mục tiêu thế thì hơi bị xa.

## Orthodox OOP

Viết tắt là OOOP, bạn có thể đọc theo kiểu đội Nhật đọc Osaka, mồm kéo dài ra chắc cũng không đến nỗi
nghe nhầm? Ý tưởng ở đây là "abuse" actor model để làm object. Tức mỗi object ở đây là một actor, có thread
riêng, hidden state và giao tiếp với nhau qua message. Như vậy ngoài những hệ thống transaction kiểu CICS
(vốn đã gồm nhiều transaction nhỏ), thì khi xây dựng một ứng dụng với OOOP ta đồng thời cũng decompose nó
theo actor model luôn.

*Tiện thể mang OOP thời Alan Kay về với mainstream.*

```CBL
class-id. Meo

method-id. Run-Around
    perform until Is-Hungry
        *> thinking where to move
        *> while not at the destination, move
        yield *> cooperative yield
    end-perform
end-method Run-Around

end-class Meo
```

Đoạn code ngây thơ và ăn zero đầu bài viết nay đã chạy.

```CBL
invoke       Cat   "New" returning cat-A
invoke       Cat   "New" returning cat-B
invoke       Home  "New" returning home
invoke       home  "Add" using cat-A
invoke       home  "Add" using cat-B
invoke async cat-A "Run-Around"
invoke async cat-B "Run-Around"
```

Syntax? đây là COBOL (ko thích thì chế language khác target CAM cũng dc).

COBOL phân biệt giữa value và reference. Tức là struct vs class trong C# ý. Thì ở đây cần làm rõ là mặc dù
ta muốn abuse actor model. Nhưng vẫn cần phân biệt giữa data (struct) và object (class). Ví dụ như string,
float, integer hay list, array gì đó nó là data, không phải object, và cũng không phải actor nốt.

Object là thứ có thể tư duy và hành động độc lập, ra quyết định real-time chứ không phải bị động. Điều này
sẽ nói cụ thể hơn ở phần sau vì có liên quan đến GC. Nhưng nguyên lý ở đây cái gì chủ động, tự tư duy hoặc
cần non-blocking thì sẽ là object (không có khái niệm thread). Ví dụ như Downloader là object.

*Ông nào bảo có cái list tự sort lúc rảnh thì tôi cũng chịu.*

## Active / passive object và GC

Đây là một topic khá "mới" (cỡ 30 năm) và cũng chưa có nhiều nghiên cứu.

Lý thuyết GC truyền thống dựa trên đặc điểm là mọi object đều passive, khi một object được chạy thì là do
nó được transfer (bằng lời gọi hàm) từ các object khác. Điều này dẫn đến khi ta trace từ rootset (stack) mà
không thấy một thằng thì có nghĩa là nó sẽ không bao giờ chạy được nữa, kết luận nó là garbage.

Với active object (thread). Khi từ rootset không trace ra nó không đồng nghĩa với việc nó sẽ không còn hoạt
động. Bởi vì nó có luồng suy nghĩ riêng, vẫn được scheduler cho chạy và tạo ra output. Chỉ có nó tự lăn ra
chết chứ GC không kill được. Thông thường bằng cách gửi cho nó message là Poison-Pill. Btw, nó mà không chịu
cắn thì cũng vẫn sống nhăn.

Tức object ở đây là unmanaged resource! Thậm chí không thể dispose.

Erlang thì quản lý bán tự động, ta có thể link thread đến owner của nó (Erlang không có class). Khi owner
dẹo object sẽ nhận được message để có thể tự sát theo. Kiểu này thực ra cũng khá ok, tất nhiên không chịu suy
nghĩ về ownership hoặc life-cycle thì sẽ leak :-)

Quản lý tự động thì garbage cần thỏa mãn tất cả những điều sau:

1. Không thuộc rootset (tất nhiên).
2. Không thể send message đến object thuộc rootset.
3. Không thể nhận message từ rootset (kể cả gián tiếp).

Định nghĩa rootset bao gồm tất cả những thể loại gì có thể gây ra side-efects, ví dụ như bạn có một cái
actor có thể đọc data từ TCP socket thì nó thuộc rootset. Bởi vì nó có thể tạo ra output có thể nhận thấy
được. Còn mấy đứa bắn message cho nhau xong tính toán cho vui thì vẫn là garbage.

http://wcl.cs.rpi.edu/salsa/salsa101/node27.html
https://arxiv.org/ftp/arxiv/papers/1201/1201.2312.pdf

Tuy nhiên tôi không thấy khả thi lắm, để xác định được các object thỏa mãn cả 3 yếu tố trên là rất khó. Ví
dụ như distributed, object có reference đến object ở node khác. Late-binding, object reference có thể được
load từ database ra. Và quan trọng nhất là nó phải lock toàn bộ hệ thống, chưa có incremental algorithm.
Cho nên ở đây ta sẽ không collect object, chỉ collect data.

# Một số vấn đề khác

## Tại sao C++ không như Alan Kay nghĩ?

> I made up the term object-oriented, and I can tell you I did not have C++ in mind.
>
> -- <cite>Alan Kay</cite>

Cần nhớ rằng C++ là bản "nâng cấp" của của C. Tức là thêm những concept của OOP trong khi không phá
hoại những gì đang có. Hòa nhập chứ không hòa tan, nên thiết kế của C++ là có thể hiểu được. C++ được
định vị là gần nhất có thể với hardware, nó có concept gì ta cũng phải map sang một cách native. Với
càng ít abstraction và overhead càng tốt. C/C++ là Assembly, phiên bản cao cấp hơn.

## Late binding (dynamic dispatch) chậm?

Thực ra cũng không chậm lắm, nếu dùng selector làm method id như [Obj-C](https://www.mulle-kybernetik.com/artikel/Optimization/opti-9.html)
thì gần như có thể so với lời gọi hàm thông thường vì nó cache được. Cái này chưa bao giờ là vấn đề với
iOS mà OSX, vốn dựa hoàn toàn trên dynamic dispatch.

## Late binding không sợ FBI

```C++
class Base
{
    int _a;
};

class Derived : public Base
{
    int _x;
};
```

Một ngày kia ta sửa cái Base thành như thế này.

```C++
class Base
{
    int _a;
    int _b;
};

class Derived : public Base
{
    int _x;
};
```

Nhìn qua tưởng không sao, nhưng thằng Derived của ta không còn đúng nữa. Vì C++ nó bind lúc compile,
nên khi kích thước cái Base thay đổi thì thằng Derived không biết. Nếu đó là cùng một project, thì
không sao, recompile lại là xong.

Nhưng nếu bạn link (dynamic) third-party vào thì? Dẹo.

Vấn đề này được gọi là fragile binary interface (FBI), và nó thốn tận rốn.

Obj-C không gặp vấn đề này, nó có thể late-binding các biến dựa trên cơ chế base + offset, one-time fix
tại runtime khi load shared library. Còn gọi method thì vốn đã là late binding. Đây là một trong những
feature mà anh Apple rất thích. Tưởng tượng bạn làm framework cho cả một OS, như kiểu Cocoa chẳng hạn.
Rồi một ngày đẹp trời bạn sửa lại một class, và bùm.

Bảo mấy thằng viết app nó recompile lại hết cho bạn.

Và user thì update tất cả các app, **cùng một lúc** :-)

Tất nhiên không phải ai cũng như ông C++, tuy nhiên tất cả những ngôn ngữ hoặc pattern có thể miễn
dịch với FBI đều dựa trên late-binding, như composite-based, hoặc dynamic fix (C#) etc.

# Tái bút

Tôi từng định đặt tên con là Alan Nguyễn, và lúc hư thì tôi bảo với nó là mày không chịu học thì chỉ
có đi nhặt rác thôi con ạ (trong khi chúng bạn nó dùng ref count hết rồi).
