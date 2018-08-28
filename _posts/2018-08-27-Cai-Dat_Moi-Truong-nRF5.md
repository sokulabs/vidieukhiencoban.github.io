---
layout: post
title: Cài Đặt Môi Trường Phát Triển Cho nRF5
categories: nRF5
---
Trong bài viết này, mình sẽ hướng dẫn các bước để cài đặt môi trường phát triển firmware cho dòng chip nRF5 của Nordic. nRF51, nRF52 là dòng chip có giao thức ble được ưa chuộng sử dụng và nó khá ổn định để áp dụng cho các bài toán thực tế.

Mình sử dụng Linux để  là môi trường ưa thích để phát triển các firmware, cụ thể là Archlinux. Mình sẽ hướng dẫn các bạn cài các tool cần thiết để phát triền firmware cho nRF5 family trên linux. 
## 1. Toolchain
Để biên dịch được source code cho nRF5 chip ta cần có toolchain để build source code. Toolchain là tập hợp các công cụ gồm trình biên dịch C/C++, trình debug,.. cho dòng vi xử lý mà nó hỗ trợ. 
nRF5 có lõi là 1 Cotex-M4 của ARM, có nhiều toolchain để sử dụng như armgcc, keil arm,... Mình sẽ sử dụng GNU Toolchain từ ARM, để cài đặt ta cần download tại [GNU ARM Toolchain](https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads)
```bash
$ cd ~/
$ wget https://developer.arm.com/-/media/Files/downloads/gnu-rm/7-2018q2/gcc-arm-none-eabi-7-2018-q2-update-linux.tar.bz2
$ tar -xvf ~/gcc-arm-none-eabi-7-2018-q2-update-linux.tar.bz2 -C /opt
$ rm ~/gcc-arm-none-eabi-7-2018-q2-update-linux.tar.bz2
```

## 2. NRF5 SDK
nRF5 SDK là 1 bộ Software Development Kit chứa source code driver và example để phát triển ứng dụng cho dòng chip nRF5.
Phiên bản mới nhất của nRF5 SDK là 15.0, có thể tải về tại trang download của Nordic [https://developer.nordicsemi.com/nRF5_SDK](https://developer.nordicsemi.com/nRF5_SDK/).

### 2.1. Tải về SDK

```bash
$ cd /opt
$ wget https://developer.nordicsemi.com/nRF5_SDK/nRF5_SDK_v15.x.x/nRF5_SDK_15.1.0_a8c0c4d.zip
$ unzip nRF5_SDK_15.1.0_a8c0c4d.zip
```

### 2.2. Chỉnh sửa đường dẫn đến Toolchain trong SDK
    
Để SDK nhận được Toolchain, chúng ta cần setup lại nó trong file `Makefile.posix` tại đường dẫn `$PATH-TO-SDK/components/toolchain/gcc`. Sửa lại đường dẫn đền Toolchain ở biến `GNU_INSTALL_ROOT` trông giống như dưới đây.

```
GNU_INSTALL_ROOT ?= /opt/gcc-arm-none-eabi-7-2018-q2-update/bin/
GNU_VERSION ?= 6.3.1
GNU_PREFIX ?= arm-none-eabi
```

### 2.3. Kiểm tra SDK

Để kiểm tra xem các cài đặt của chúng ta đã đúng hết chưa ta thử build 1 example có trong SDK.
```bash
$ cd /opt/nRF5_SDK_15.0.0_a53641a/examples/peripheral/blinky/pca10040/blank/armgcc
$ make 
```
Nếu xuất hiện log build và thông báo build thành công đã xuất được file hex là các cài đặt của chúng ta đúng hết rồi. 
```
mkdir _build
cd _build && mkdir nrf52832_xxaa
Assembling file: gcc_startup_nrf52.S
Compiling file: main.c
Compiling file: boards.c
Compiling file: app_error.c
Compiling file: app_error_handler_gcc.c
Compiling file: app_error_weak.c
Compiling file: app_util_platform.c
Compiling file: nrf_assert.c
Compiling file: nrf_strerror.c
Compiling file: system_nrf52.c
Linking target: _build/nrf52832_xxaa.out
text	   data	    bss	    dec	    hex	filename
2076	    108	     28	   2212	    8a4	_build/nrf52832_xxaa.out
Preparing: _build/nrf52832_xxaa.hex
Preparing: _build/nrf52832_xxaa.bin
DONE nrf52832_xxaa
```

## 3. Segger JLink
Segger JLink là 1 công cụ debug cho các dòng vi điều khiển có lõi cortex. Segger JLink có support debug cho rất nhiều dòng vi điều khiển từ các hãng khác nhau trong đó có nrf5-family của Nordic. Trên Development Kit của Nordic cũng sử  dụng JLink làm thành phần debugger cho vi điều khiển chính. 

Nếu bạn đang sử dụng Ubuntu hoặc 1 hệ điều hành based Debian thì chỉ cần tải file deb về và tiến hành cài đặt file deb.
```bash
$ cd ~
$ wget https://www.segger.com/downloads/jlink/JLink_Linux_x86_64.deb
$ sudo apt install ./JLink_Linux_x86_64.deb
```

Đối với Archlinux thì cần phải tải về file nén tar và cài đặt bằng tay, cũng có thể sử dụng cách này để cài đặt trên những linux distro khác.
```bash
$ cd ~
$ wget https://www.segger.com/downloads/jlink/JLink_Linux_x86_64.tgz
$ tar -xvf JLink_Linux_x86_64.tgz -C /opt
$ sudo cp /opt/JLink_Linux_x86_64/99-jlink.rules /etc/udev/rules.d/
$ sudo ln -s /opt/JLink_Linux_x86_64/libjlinkarm.so /usr/lib/libjlinkarm.so
$ sudo ldconfig
```

## 4. nRF Connect và nRF Tools
### 4.1. nrfjprog
Nordic cung cấp cho các developer công cụ để có thể đọc, ghi, xóa flash của nRF5 chip là `nrfjprog`. Chúng ta có thể tải tool này tại [đây](https://www.nordicsemi.com/eng/nordic/download_resource/51426/29/92582723/94917)
```bash
$ cd ~
$ wget https://www.nordicsemi.com/eng/nordic/download_resource/51426/29/92582723/94917 -O nrftools.tar
$ tar -xvf nrftools.tar -C /opt
$ echo "PATH=$PATH:/opt/nrfjprog/" > ~/.profile
```

Log out và Log in lại vào hệ thống, thử call `nrfjprog` tool trong terminal, nếu terminal nhận được command là đã ok.
![Test nrfjprog]({{ site.baseurl }}/images/2018-08-27-nrfjprog-version.png "nrfjprog output")

### 4.2. nrf connect
[nrfconnect](https://www.nordicsemi.com/eng/nordic/Products/nRF51822-Development-Kit/nRF-Connect-Linux/56417) là 1 tập hợp các tool phục vụ cho việc test và kiểm tra kết nối ble của firmware chúng ta đang phát triển với thiết bị khác. Hay nói cách khác, chúng ta có thể sử dụng 1 module nrf5 trên tool này để làm thiết bị test cho device của chúng ta. 

```bash
$ wget https://www.nordicsemi.com/eng/nordic/download_resource/56417/18/30661060/108237 -O /opt/nrfconnect-2.5.0.AppImage
$ sudo chmod +x /opt/nrfconnect-2.5.0.AppImage
```
Để khởi động ứng dụng ta chỉ cần di chuyển terminal đến thư mục chứa ứng dụng và khởi chạy
```bash
$ cd /opt
$ ./nrfconnect-2.5.0.AppImage
```
hoặc tạo 1 symbolic link vào trong thư mục `/usr/bin` để có thể gọi trực tiếp bất cứ đâu
```bash
$ sudo ln -s /opt/nrfconnect-2.5.0.AppImage /usr/bin/nrfconnect
```
![nrfconnect GUI]({{ site.baseurl }}/images/2018-08-27-nrfconnect-gui.png "nrfconnect GUI")

## 5. Kết
Như vậy là đã cài xong những công cụ cần thiết để có thể phát triển firmware cho nRF5 trên môi trường Linux. Điều phải làm tiếp theo là bạn cần có 1 trình editor để viết code hoặc 1 trình IDE. Chúng ta có thể sử dụng Geany, Vim, Visual Studio Code hoặc Eclipse,...

Bài [tiếp theo]() sẽ hướng dẫn cài đặt Eclipse IDE cho nRF5.