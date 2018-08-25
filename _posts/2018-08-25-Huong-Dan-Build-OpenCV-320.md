---
layout: post
title: Hướng dẫn build OpenCV 3.2.0
---

# Hướng dẫn build OpenCV 3.2.0

## Công cụ cần thiết
Để build OpenCV cần có trình biên dịch GNU GCC và CMake. Cài đặt 2 công cụ trên bằng lệnh

`
$ sudo pacman -S gcc cmake
`

## Cấu hình và generate Makefile
OpenCV sử dụng Cmake để quản lý source code. Mình sẽ sử dụng Cmake cùng GUI để cấu hình cho source code của opencv.

1. Tải source code của OpenCV 3.2.0 tại https://opencv.org/opencv-3-2.html
2. Giải nén và Di chuyển vào thư mục chứa source code. Giả sử là **OpenCV-320**.
3. Tạo 1 thư mục để build source code. Giả sử là **build-linux**.
4. Khởi chạy cmake:

    `
    $ cmake-gui
    `

5. Từ giao diện của cmake-gui, lựa chọn thư mục chứa source code và thư mục build. Sau đó bấm *Configure*.
6. Trong cửa sổ config trình biên dịch mới mở ra. Lựa chọn mặc định và bấm *Finish*.
7. Sau khi quá trình configure thành công. Có thể bấm tiếp *Generate* để sinh ra Makefile hoặc lựa chọn cài đặt thêm các biến define. Trong trường hợp này, mình muốn build openCV để có thể sử dụng cùng bộ QT5, nên mình sẽ cấu hình thêm.
8. Cấu hình QT5.
    8.1. Bật flag WITH_QT.
    8.2. Set đường dẫn cho các module của QT5: ...
9. Bấm *Generate*

## Build Source Code
Sau khi đã Generate Makefile thành công, ta có thể build source code rồi. Sử dụng make để build source code. 

`
$ make -j8
`

Chú ý rằng option *-j8* là lựa chọn số thread mà CPU sử dụng để build. Sử dụng option này để tăng tốc quá trình build source code. Mặc định là 1 thread. 