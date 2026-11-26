# CircuitCoach AI v10

CircuitCoach AI giúp học sinh xem sơ đồ lắp mạch, đối chiếu ảnh theo tiêu chí của bài và tự sửa theo gợi ý. Giáo viên có thể xem từng lượt làm, nhận xét, khoanh vùng lỗi và xác nhận an toàn trên mạch thật.

- [Sản phẩm đang chạy online](https://circuitcoach-stem-lab-2026.tartlapwing1.chatgpt.site/#/home)
- Mã nguồn bản online v10 ngày 26/11/2026, commit trên OpenAI Sites `c9ff5fc9fedb347abb604df3d517a9955d64f95b`.
- Gói này bổ sung bộ ảnh tham chiếu từ bản online và cách chạy dùng thử riêng trên máy.

## Chạy bản dùng thử trên Windows

1. Cài Node.js 24 và giải nén ZIP vào thư mục mới.
2. Nếu đã có tệp cấu hình riêng, đặt `.circuitcoach-demo.env` vào cùng thư mục với `CHAY_DEMO_CIRCUITCOACH.cmd`. Để dùng tài khoản mẫu trong bảng bên dưới, sao chép tệp `.circuitcoach-demo.env.example` thành `.circuitcoach-demo.env` bằng lệnh sau.

   ```powershell
   if (-not (Test-Path -LiteralPath '.circuitcoach-demo.env')) {
     Copy-Item -LiteralPath '.circuitcoach-demo.env.example' -Destination '.circuitcoach-demo.env'
   }
   ```

3. Mở `CHAY_DEMO_CIRCUITCOACH.cmd` rồi truy cập [CircuitCoach AI dùng thử](http://127.0.0.1:3191/#/home).
4. Chọn đúng vai trò và nhập thông tin từ tệp cấu hình đã dùng. Mở **Kho tham chiếu** để xem ảnh theo bài học.

### Tài khoản mẫu cho bản chạy local

Các tài khoản sau dùng được sau khi sao chép tệp cấu hình mẫu ở bước 2. Bản dùng thử chạy tại địa chỉ trên máy của bạn.

| Vai trò | Tên đăng nhập hoặc tên nhóm | Mã lớp hoặc mật khẩu |
| --- | --- | --- |
| Học sinh | `nhom-mau` | Mã lớp `DEMO920` |
| Giáo viên | `giao-vien-demo` | Mật khẩu `GVdemo26` |
| Quản trị viên | `quan-tri-demo` | Mật khẩu `QTdemo26` |

### Dựng từ mã nguồn trên GitHub

Nếu tải mã nguồn từ GitHub, chạy `npm ci` và `npm run build` tại thư mục mã nguồn trước khi mở tệp `.cmd`. Có thể chạy bằng PowerShell sau khi dựng.

```powershell
$env:LOCAL_PORT = '3191'
$env:CIRCUITCOACH_ONLINE_DEMO = '1'
node scripts/local-worker.mjs
```

Hệ thống tạo SQLite cho bản dùng thử trong `.data/online-demo/`. Bài làm mới được lưu trên máy chạy ứng dụng.

### Cấu hình phân tích AI

Khi chưa có khóa OpenAI, giao diện bật **Chế độ demo** và dùng kết quả mô phỏng. Để phân tích thật trên bản dùng thử, nhập khóa riêng ở **Cấu hình OpenAI** hoặc trong `.circuitcoach-demo.env`, theo mẫu `.circuitcoach-demo.env.example`. Khóa mặc định của bản online được quản lý trong OpenAI Sites.

## Dữ liệu lấy từ bản online

Thư mục `du-lieu-mau-online/` có **35 mục ảnh tham chiếu đã duyệt**, tương ứng **31 ảnh khác nhau**. Bản dùng thử tự nạp ảnh vào kho tham chiếu khi khởi động.

| Bài học | Mục ảnh đã duyệt |
| --- | ---: |
| LED cơ bản | 13 |
| Cảm biến siêu âm HC-SR04 | 6 |
| Servo | 9 |
| Bật/tắt LED bằng pin 9V và công tắc | 7 |
| Cảm biến độ ẩm đất | 0 |
| **Tổng** | **35** |

`du-lieu-mau-online/manifest.json` ghi nguồn, thời điểm lấy dữ liệu, tiêu chí, nhãn đúng hoặc có lỗi, vùng hình học được đánh dấu và mã SHA-256 của ảnh. Bản chia sẻ dùng ảnh công khai, nhãn và vùng khoanh. Tên người dùng, ghi chú tự do, ảnh bài nộp và dữ liệu lớp thật được lưu riêng.

## Dùng thử theo vai trò

- **Học sinh** chọn bài, xem hướng dẫn, gửi 1–3 ảnh cùng một mạch rồi chọn **Phân tích mạch**. Mở từng tiêu chí để xem bằng chứng hoặc gửi ảnh kiểm tra lại. Giữ mạch ngắt nguồn để giáo viên kiểm tra trực tiếp và xác nhận an toàn.
- **Giáo viên** chọn lớp rồi mở từng lượt làm để xem ảnh, nhận xét, gắn nhãn và xác nhận an toàn. Dùng **Tải ảnh theo tiêu chí** để lưu ảnh khoanh vùng và **Báo cáo thống kê** để xem theo lớp hoặc bài học.
- **Quản trị viên** mở **Quản trị** để xem tài khoản giáo viên và báo cáo tổng hợp. Bấm **Đăng xuất** trước khi chuyển vai trò.

## Cấu trúc và sao lưu

| Vị trí | Công dụng |
| --- | --- |
| `src/` | Giao diện, bài học và thao tác theo vai trò |
| `server/ai/` | Câu lệnh, đối chiếu ảnh và quy tắc an toàn |
| `worker/runtime.js` | API của bản OpenAI Sites |
| `public/images/` | Ảnh và sơ đồ đi kèm mã nguồn |
| `du-lieu-mau-online/` | Ảnh tham chiếu công khai đã đóng gói để chạy thử |
| `drizzle/` | Cấu trúc và nâng cấp database |
| `.openai/hosting.json` | Định danh Site và khai báo D1 `DB`, kho ảnh R2 `BUCKET` |

Chạy `npm test` để kiểm tra mã nguồn. Khi đưa lên GitHub, giải nén và đưa các tệp nguồn cùng `README.md` lên kho. Giữ dữ liệu chạy local và cấu hình đăng nhập trong máy. Git đã bỏ qua `.data/`, `.circuitcoach-demo.env` và `dist/`. Có thể đính kèm ZIP làm bản phát hành dựng sẵn.

Để triển khai bản riêng lên OpenAI Sites, tạo dự án đích và sử dụng định danh do Sites cấp. Thiết lập các biến API và phiên bằng mục cài đặt của Site. Bản online dùng D1/R2. Bộ dữ liệu kèm ZIP được nạp khi chạy bản dùng thử local.
