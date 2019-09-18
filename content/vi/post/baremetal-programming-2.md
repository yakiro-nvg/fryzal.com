+++
title = "BMPM #2: C, ABI và ARMv8"
date = 2019-09-18
draft = true
authors = ["Nguyen, Giang (G. Yakiro)"]

tags = ["bare-metal", "fundamental", "baby-programmer"]
summary = "Cách code C thực sự hoạt động trên CPU"
+++

Tôi có nhận được inbox là sao tên series lại là lập trình "suýt nhúng".

Lý do là tôi không phải dân làm nhúng gạo cội gì cho cam, hay nói cách khác, về nhúng thì
tôi cũng chỉ suýt pro, tý thì pro thưa các bạn. Tôi có khoảng 6 năm kinh nghiệm làm về low
level. Mà trong khoảng thời gian đó tôi cũng không thực sự tập trung làm về mảng này mà có
đánh đấm thêm một số món khác nên nếu nói về nhúng thì tôi không dám nhận là pro.

**May là tôi biết assembly từ hồi đi crack phần mềm nên võ vẽ cũng tàm tạm.**

# Giờ còn học assembly?

Chơi với C mà không biết ASM thì là điều đáng tiếc. Lý do thì không phải để code chạy cho
nó nhanh như nhiều người vẫn lầm tưởng. Giờ hiếm khi code bằng ASM mà chạy nhanh hơn code
bằng ngôn ngữ bậc cao lắm. Trừ khi bạn là thánh Mike, bố đẻ của LuaJIT, mà cả thế giới
cũng chỉ có mỗi một ông Mike thôi.

Tôi cũng không ủng hộ việc code bằng ASM trừ khi bất khả kháng. Tuy nhiên ta không muốn
code bằng ASM không có nghĩa là ta cũng không cần biết ASM nốt. Việc hiểu biết ASM sẽ giúp
cho ta đỡ được những pha kiểu như thế này:

[Debug qua đường email và cái kết](https://fryzal.com/post/extreme-bug-hunting/)

Khi chương trình bạn viết hay hệ thống bạn chạy không hoạt động, vì một lý do nào đó mà bạn
chính là người chịu trách nhiệm cấp cứu cho nó trong khi chả có gì trong tay thì cái bạn cần
có thể là hiểu biết về ASM. Không cần quá giỏi nhưng ít nhất đủ biết cái gì đang xảy ra.

# Chuẩn bị môi trường

Để đi tiếp thì ta cần 2 thứ, `compiler` và `emulator`.

## Cross compiler

C là ngôn ngữ bậc cao, với những khái niệm như hàm, kiểu dữ liệu, cấu trúc etc. Từ C dịch
sang ASM thì tuỳ loại CPU và platform mà bạn dùng thì kết quả sẽ khác nhau. Trong bài này
ta sẽ sử dụng kiến trúc ARMv8, khác với kiến trúc x86_64 mà phần lớn máy tính bạn đang code
sử dụng. Lý do dùng ARM là vì nó được dùng nhiều trên các thiết bị di động cũng như trên xe
ô tô.

Còn tại sao `v8` thì do tôi biết mỗi `v8` thôi, bạn thích học `v7` thì tôi chịu.

Để compile được code cho chip ARMv8 thì bạn cần compiler có khả năng dịch ra mã ASM cho nó.
Nhắc lại lần nữa là không phải compiler để dịch app chạy trên cái máy bạn đang code nhá, hay
bạn code bằng điện thoại thì tôi cũng chịu nốt.

[Download compiler ở đây, chọn 'aarch64-elf']
(https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-a/downloads)

Thời điểm viết bài thì ARM chỉ cung cấp compiler chạy trên Linux và Windows. Đoạn này hơi bị
hip-hop tý, nhưng tôi nghĩ cần thiết làm rõ là compiler của bạn chạy trên máy x86_64, tức là
nó được viết bằng mã x86_64. Nhưng chương trình nó dịch ra thì lại là ARM, điều này được gọi
là "phát triển chéo" (cross-compile).

Nếu bạn vẫn không cài được compiler phù hợp thì inbox tôi.

## Có nên mua board xịn?

Nhiều người hỏi tôi học lập trình nhúng thì mua board gì.

Tôi không hiểu idea học hành của các bạn là gì, nhưng kinh nghiệm cá nhân tôi là phàm cái trò
học hành mà cứ `gear oriented` là chỉ có mất tiền ngu, học gì cũng thế. Tôi từng mua cỡ 3-4 cái
guitar điện rồi amply, fuzz các kiểu bày đầy nhà thời còn tập tành chơi rock. Uh thì cũng đánh
được vài bài cơ bản của Metallica, giờ ngẫm lại thấy trình độ tôi thì chơi cái đàn cỡ 10 củ là
thừa mẹ nó rồi.

Chưa kể gu nhạc giờ sang JangNara 2018 nên đàn đóm đắp chiếu hết.

JangNara 2018 mạnh vãi, khéo phải gấp đôi T-ara, thật!

Lại còn cũng tên là Jang luôn.

Có người thì bảo mua con board càng đắt càng tốt, về học cho nó sướng, cho bản thân có động lực
các kiểu. Mặc dù tôi không phải chuyên gia về `động lực học`. Nhưng tôi xin phép lại chia sẻ tiếp
thì chi tiền để mà mua đam mê là trò đùa rất kém sang, mà thậm chí chả buồn cười tý nào. Một khi
bạn đã chán rồi thì có trăm củ thì bạn cũng đắp chiếu, thế thôi.

## QEMU emulator

Để bắt đầu tôi cực kỳ khuyến khích bạn dùng mẹ QEMU cho nó nhanh, miễn phí và có sẵn debugger.
Thay vì đi mua mấy con board vài ngàn mỹ kim về xong còn chưa có JTAG, và bạn còn chả biết phải
làm gì với nó ngoài cài Android.

[Link tải QEMU, cài xong không chạy inbox tiếp.](https://www.qemu.org/download/)

# Hello world in ASM

Với chương trình đầu tiên này, ta cần 2 thứ.

## Linkerscript

Bởi vì ta đang compile cho một chương trình dạng baremetal, tức là chạy không có OS. Nên ta cần
chỉ định rõ cho compiler biết cần load chương trình vào địa chỉ nào. Địa chỉ là thứ được CPU dùng
để tham chiếu đến nhiều phần cứng khác nhau, không chỉ là RAM. Cứ tưởng tượng như kiểu bạn đang ở
trong một khu phố, và RAM chỉ là một khu nhà kho có địa chỉ xác định. Phần còn lại như các cổng
giao tiếp và các bộ điều khiển cũng có địa chỉ riêng của nó.

Trong bài viết này thì QEMU sẽ đánh địa chỉ cho RAM bắt đầu từ `0x40000000`, tuy nhiên 2MB đầu tiên
đã được sử dụng bởi QEMU để lưu DTB (tôi sẽ giới thiệu sau). Cho nên ta sẽ load chương trình vào địa
chỉ `0x40200000`.

Vùng `.text` là nơi chứa mã assembly của chương trình.

```
ENTRY(entry_point)

SECTIONS {
    	. = 0x40200000;

    	.text : {
        	*(.text*)
    	}
}
```

## Source code hàm entry_point

```
.global entry_point

entry_point:
    	ldr x0, =0xcafebabe // load 0xcafebabe to x0
    	b .                 // infinity loop
```

Cơ bản về ASM như sau, chương trình gồm một chuỗi các instruction, là các lệnh để điều khiển CPU. Mỗi
lệnh là một dòng, bắt đầu bằng tên instruction. Ví dụ ở trên ta có 2 lệnh là `ldr` (load address), được
dùng để load giá trị từ một địa chỉ vào thanh ghi, và `b` (branch) thì hoạt động như kiểu `goto`.

Chương trình trên sẽ load giá trị `0xcafebabe` vào thanh ghi `x0`. Để tăng tốc độ thì CPU không làm
việc trực tiếp với RAM, mà khi khởi động ta cũng chưa có RAM để dùng, hoặc có hệ thống còn chả có RAM
luôn. Thay vào đó CPU sẽ có một bộ các thanh ghi để lưu trữ dữ liệu, kiến trúc `aarch64` (ARMv8 64 bits)
có 31 thanh ghi dạng general purpose được đánh số từ `x0-x30`.

Khác với hello world bình thường, thời điểm này ta vừa khởi động nên không có `printf`, vì vậy
ta sẽ kiểm tra giá trị của thanh ghi x0, nếu nó là 0xcafebabe thì tức là chương trình đã chạy ngon.

## Compile ra file binary

Sử dụng lệnh sau để compile:

```bash
$ aarch64-elf-gcc entry_point.S -Tlink.lds -o bmpm.bin -nostdlib -ffreestanding
```

Kiểm tra cấu trúc địa chỉ:

```bash
$ objdump bmpm.bin -h

bmpm.bin:	file format ELF64-aarch64-little

Sections:
Idx Name          Size     Address          Type
  0               00000000 0000000000000000
  1 .text         00000010 0000000040200000 TEXT
  2 .symtab       00000090 0000000000000000
  3 .strtab       00000050 0000000000000000
  4 .shstrtab     00000021 0000000000000000
```

Kiểm tra mã được generate ra:

```bash
$ $ objdump bmpm.bin -d

bmpm.bin:	file format ELF64-aarch64-little

Disassembly of section .text:

entry_point:
40200000:	40 00 00 58 	ldr	x0, #8
40200004:	00 00 00 14 	b	#0 <entry_point+0x4>

$d:
40200008:	be ba fe ca 	.word	0xcafebabe
4020000c:	00 00 00 00 	.word	0x00000000
```

## Load vào QEMU và test thử

Sử dụng lệnh sau:

```bash
$ qemu-system-aarch64 \
    	-M virt,gic_version=3 \
    	-cpu cortex-a53 \
    	-smp 4 -M type=virt -m 512M \
    	-chardev socket,id=qemu-monitor,host=localhost,port=7777,server,nowait,telnet \
    	-mon qemu-monitor,mode=readline \
    	-nographic -no-reboot -s \
    	-kernel bmpm.bin 
```

Thời điểm này ta sẽ thấy QEMU treo và không in ra cái gì cả. Nếu nó in ra gì đó thì tức là
quá trình bạn làm có vấn đề và có gì đó không hoạt động. Inbox tôi nếu cần thiết. Giờ ta
sẽ dùng gdb để remote vào QEMU xem chương trình chạy đến đâu.

Trong một terminal khác:

```bash
$ gdb bmpm.bin
Reading symbols from bmpm.bin...
(gdb)
```

Sau khi vào được console của gdb, sử dụng lệnh sau để remote:

```bash
(gdb) target remote :1234
Remote debugging using :1234
0x0000000040200004 in entry_point ()
```

Và dùng `p/x` để kiểm tra giá trị của thanh ghi:

```bash
(gdb) p/x $x0
$1 = 0xcafebabe
```

Có thể chuyển sang `layout asm`:

```bash
(gdb) layout asm
```

# Tham khảo

[Source code](https://github.com/yakiro-nvg/bmpm_source/tree/part.2)