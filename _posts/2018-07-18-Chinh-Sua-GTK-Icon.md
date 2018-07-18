-----
layout: post
title: Chỉnh sửa icon của ECLIPSE gọn đẹp hơn
-----


Có lẽ nhiều người thấy rằng Eclipse trên Linux sắp xếp các icon trên thanh công cụ qúa xa nhau, không gọn gàng như trên Window.

Để các icon này xít vào nhau hơn nhìn cho gọn mắt và thẩm mỹ thì chỉ cần thêm đoạn mã dưới đây vào file **~/.gtkrc-2.0**. Nếu file **.gtkrc-2.0** chưa có sẵn trong thư mục **~** thì ta tạo file mới sau đó thêm đoạn code trên

```
style "compact-toolbar"
{
    GtkToolbar::internal-padding = 0
    xthickness = 1
    ythickness = 1
}

style "compact-button"
{
    xthickness = 0
    ythickness = 0
}

class "GtkToolbar"                   style "compact-toolbar"
widget_class "*<GtkToolbar>*<GtkButton>"    style "compact-button"
```
