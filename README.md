# Website quà tặng tình yêu — trái tim đen đỏ

Website responsive cho điện thoại, có trái tim phát sáng, màn hình nhập mật khẩu, thư riêng tư được trả về từ Netlify Function và công cụ tạo QR sau khi xuất bản.

## Cần biết trước
- Mã QR chỉ chứa URL của website, không chứa mật khẩu.
- Mật khẩu và lời chúc được lưu ở biến môi trường phía máy chủ Netlify, không nhúng trực tiếp vào HTML.
- Đây là mẫu quà tặng riêng tư cơ bản, không phải hệ thống bảo mật cấp cao. Với website công khai, nên bật rate limiting/WAF ở nhà cung cấp để hạn chế đoán mật khẩu; không dùng mật khẩu quan trọng đang dùng ở nơi khác.
- Font và thư viện QR lấy từ CDN nên cần Internet. Nếu CDN bị chặn, phần tạo QR có thể không hoạt động.

## 1. Chuẩn bị
1. Giải nén file ZIP.
2. Tạo tài khoản Netlify và GitHub nếu chưa có.
3. Tạo repository GitHub mới, tải các file trong thư mục này lên repository (giữ nguyên `netlify/functions/unlock.mjs` và `netlify.toml`).
4. Trên Netlify chọn **Add new site → Import an existing project → GitHub**, chọn repository và triển khai.

> Không chỉ kéo-thả `index.html` vào Netlify Drop: cách đó thường không triển khai kèm function; cần import repository hoặc dùng Netlify CLI.

## 2. Đặt mật khẩu và lời chúc
Trong Netlify, mở site → **Site configuration** (hoặc **Project configuration**) → **Environment variables**. Thêm:
- `LOVE_PASSWORD` = mật khẩu bí mật của bạn (nên dài ít nhất 10 ký tự, không dùng mật khẩu tài khoản cá nhân).
- `LOVE_MESSAGE` = lời chúc của bạn. Có thể viết nhiều dòng; giữ nguyên dấu tiếng Việt.

Ví dụ cho `LOVE_MESSAGE` (hãy thay bằng lời của bạn):
---
Gửi em ❤️

Cảm ơn em vì đã xuất hiện trong cuộc sống của anh. Anh không biết tương lai sẽ ra sao, nhưng anh mong chúng ta luôn chân thành, biết lắng nghe và cùng nhau trưởng thành.

Mong em có thật nhiều niềm vui, luôn được là chính mình và nhớ rằng em rất đáng quý.

Thương em.
---
Sau khi đổi biến môi trường, hãy **trigger một lần deploy mới** để chắc chắn function nhận cấu hình mới. Không lưu mật khẩu thật trong README hay repository.

## 3. Lấy link và tạo mã QR
1. Mở website đã deploy, sao chép URL dạng `https://ten-site.netlify.app`.
2. Mở website trên điện thoại hoặc máy tính, nhấn **Tạo mã QR cho đường dẫn này**.
3. Nếu URL chưa tự điền, dán URL đã xuất bản rồi nhấn **Tạo lại mã QR**.
4. Quét thử bằng một thiết bị khác. Gửi URL/QR cho người yêu và gửi mật khẩu qua một kênh riêng.

## 4. Thay đổi giao diện/lời chúc
- Sửa tiêu đề, màu sắc, hiệu ứng trong `index.html`.
- Sửa lời chúc trong biến môi trường `LOVE_MESSAGE`; không cần sửa mã nguồn để thay nội dung thư.
- Sau khi chỉnh file, commit/push lên GitHub để Netlify tự deploy lại.

## 5. Kiểm tra nhanh
- URL mở được trên điện thoại.
- Nhập sai mật khẩu thì không hiện thư.
- Nhập đúng mật khẩu thì thư xuất hiện.
- QR trỏ đến đúng URL `https://...`, không trỏ đến `localhost` hay đường dẫn xem trước.
- Nếu gặp lỗi 503: kiểm tra `LOVE_PASSWORD` đã được đặt và đã deploy lại.
- Nếu gặp lỗi 404 ở endpoint: kiểm tra function nằm đúng đường dẫn `netlify/functions/unlock.mjs` và site được deploy bằng Netlify có Functions.

## Cấu trúc
- `index.html` — giao diện trái tim, form mật khẩu, thư và tạo QR.
- `netlify/functions/unlock.mjs` — xác minh mật khẩu phía máy chủ, trả lời thư khi đúng.
- `netlify.toml` — cấu hình deploy.
