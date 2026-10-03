# Bài 1 - Điều phối hàng đợi và tính doanh thu

## Phân tích lỗi

- `waitingQueue.pop()` lấy xe ở cuối mảng (`51K-678.99`), tức xe đến sau cùng được vào sạc trước. Hàng đợi FIFO phải lấy xe ở đầu mảng bằng `shift()`.
- Điều kiện `i <= completedSessionsKwh.length` cho phép vòng lặp chạy thêm một lượt với chỉ số bằng độ dài mảng. Giá trị đọc được khi đó là `undefined`; phép cộng `totalKwh += undefined` biến tổng thành `NaN`, kéo theo doanh thu cũng là `NaN`.
- Vòng lặp đã được sửa thành `i < completedSessionsKwh.length`, để chỉ truy cập các chỉ số hợp lệ.

## Test cases

| Trường hợp kiểm thử | Dữ liệu đầu vào | Kết quả sai thực tế | Kết quả đúng mong đợi |
|---|---|---|---|
| Hàng đợi nhiều xe và tính tổng ca mẫu | Hàng đợi `['29A-112.33', '30E-889.12', '51K-678.99']`; điện năng `[45.2, 30.5, 62.8, 28.0]`; đơn giá `4500` | Xe `51K-678.99` bị lấy trước; tổng điện năng và doanh thu thành `NaN` | Điều phối xe `29A-112.33`; tổng `166.5 kWh`; doanh thu `749250 VNĐ` |
| Mảng điện năng rỗng | `[]`; đơn giá `4500` | Vòng lặp có thể đọc `undefined` ở chỉ số `0`, làm tổng thành `NaN` | Tổng `0 kWh`; doanh thu `0 VNĐ` |
| Mảng điện năng có một phần tử | `[45.2]`; đơn giá `4500` | Vòng lặp chạy thêm lượt tại chỉ số `1`, đọc `undefined` và làm tổng thành `NaN` | Tổng `45.2 kWh`; doanh thu `203400 VNĐ` |
