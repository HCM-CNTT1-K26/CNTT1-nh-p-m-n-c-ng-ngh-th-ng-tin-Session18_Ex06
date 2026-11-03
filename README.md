# CNTT1-nh-p-m-n-c-ng-ngh-th-ng-tin-Session18_Ex06
### Phần 1. Phân tích 

Tích chọn **Disable Cache** trên tab Network rồi load lại trang  portal.greenlog.vn , lỗi chậm 20 giây lộ rõ ở 2 điểm:

* Hàng loạt request báo đỏ ( 404 Not Found ) do sai đường dẫn file script hoặc ảnh bản đồ, khiến trình duyệt mất thời gian chờ timeout.
* Các file ảnh bản đồ dung lượng quá lớn làm thanh biểu đồ Waterfall kéo dài dằng dặc, nghẽn luồng render giao diện.

---

### Phần 2. Triển khai

1. **Sửa lỗi 404:** Lọc các request lỗi đỏ trên tab Network, chỉnh lại URL chuẩn xác để trả về mã  200 OK.
2. **Nén hình ảnh:** Giảm dung lượng file ảnh bản đồ hoặc đổi sang định dạng WebP để tăng tốc độ truyền tải.
3. **Bật Browser Cache:** Cấu hình header phía server ( Cache-Control ) cho tài nguyên tĩnh (CSS, JS, ảnh) để lưu vào bộ nhớ đệm máy khách.

---

### Phần 3. Kiểm thử 

Khi tải lại trang mà nhận mã **304 Not Modified**, đây là tín hiệu rất tốt:

* Trình duyệt tự đối chiếu thấy dữ liệu lưu trong máy vẫn khớp với server, nên server chỉ trả về một gói header siêu nhỏ xác nhận chứ không cần truyền lại toàn bộ file.
* Thao tác này giúp shipper tiết kiệm tối đa băng thông 4G trên đường, đồng thời giảm tải trực tiếp cho máy chủ.
