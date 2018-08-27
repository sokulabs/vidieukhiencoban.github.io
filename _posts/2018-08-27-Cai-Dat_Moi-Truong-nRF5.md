---
layout: post
title: Cài Đặt Môi Trường Phát Triển Cho nRF5
categories: nRF5
---
Trong bài viết này, mình sẽ hướng dẫn các bước để cài đặt môi trường phát triển firmware cho dòng chip nRF5 của Nordic. nRF51, nRF52 là dòng chip có giao thức ble được ưa chuộng sử dụng và nó khá ổn định để áp dụng cho các bài toán thực tế.

## 1. Môi trường làm việc và công cụ cần thiết
Mình sử dụng Linux để  là môi trường ưa thích để phát triển các firmware, cụ thể là Archlinux. Mình sẽ hướng dẫn các bạn cài các tool cần thiết để phát triền firmware cho nRF5 family trên linux. 
### 1.1 Toolchain
Để biên dịch được source code cho nRF5 chip ta cần có toolchain để build source code. Toolchain là tập hợp các công cụ gồm trình biên dịch C/C++, trình debug,.. cho dòng vi xử lý mà nó hỗ trợ. 
nRF5 có lõi là 1 Cotex-M4 của ARM, có nhiều toolchain để sử dụng như armgcc, keil arm,... Mình sẽ sử dụng GNU Toolchain từ ARM, để cài đặt ta cần download tại [GNU ARM Toolchain](https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads)
```bash
$ cd ~/
$ wget https://developer.arm.com/-/media/Files/downloads/gnu-rm/7-2018q2/gcc-arm-none-eabi-7-2018-q2-update-linux.tar.bz2
$ tar -xvf ~/gcc-arm-none-eabi-7-2018-q2-update-linux.tar.bz2 -C /opt
$ rm ~/gcc-arm-none-eabi-7-2018-q2-update-linux.tar.bz2
```

### 1.2 NRF5 SDK
nRF5 SDK là 1 bộ Software Development Kit chứa source code driver và example để phát triển ứng dụng cho dòng chip nRF5.
Phiên bản mới nhất của nRF5 SDK là 15.0, có thể tải về tại trang download của Nordic [https://developer.nordicsemi.com/nRF5_SDK](https://developer.nordicsemi.com/nRF5_SDK/).
1. Tải về SDK
    ```bash
    $ cd /opt
    $ wget https://developer.nordicsemi.com/nRF5_SDK/nRF5_SDK_v15.x.x/nRF5_SDK_15.1.0_a8c0c4d.zip
    $ unzip nRF5_SDK_15.1.0_a8c0c4d.zip
    ```
2. Chỉnh sửa đường dẫn đến Toolchain trong SDK
    
    Để SDK nhận được Toolchain, chúng ta cần setup lại nó trong file `Makefile.posix` tại đường dẫn `$PATH-TO-SDK/components/toolchain/gcc`. Sửa lại đường dẫn đền Toolchain ở biến `GNU_INSTALL_ROOT` trông giống như dưới đây.
    ```Makefile
    GNU_INSTALL_ROOT ?= /opt/gcc-arm-none-eabi-7-2018-q2-update/bin/
    GNU_VERSION ?= 6.3.1
    GNU_PREFIX ?= arm-none-eabi
    ```
### 1.3 Segger JLink
### 1.4 nRF Connect 

## 2. Cài Đặt Eclipse Làm IDE