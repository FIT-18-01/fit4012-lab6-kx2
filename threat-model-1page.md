# Threat Model - Lab 6 AES-CBC Socket

## Thông tin nhóm

- Thành viên 1: Nguyễn Trung Kiên
- Thành viên 2: Hoàng Nhật Anh

## Assets

Các tài sản cần bảo vệ gồm plaintext trước khi mã hóa, AES key, IV, ciphertext đang truyền trên mạng, file đầu vào sample_input.txt, file đầu ra sample_output.txt và các file log chứa thông tin hệ thống. Nếu attacker đọc được AES key hoặc IV thì toàn bộ ciphertext có thể bị giải mã.

## Attacker model

Đối tượng tấn công được giả định có khả năng nghe lén mạng LAN, bắt gói tin TCP giữa Sender và Receiver hoặc sửa đổi packet trong quá trình truyền. Attacker cũng có thể replay lại packet cũ hoặc đọc file log nếu có quyền truy cập máy tính. Ngoài ra attacker có thể thử gửi ciphertext giả mạo đến Receiver.

## Threats

- Key disclosure do AES key và IV được gửi plaintext qua key channel.
- Tampering do attacker sửa ciphertext khiến dữ liệu giải mã sai hoặc gây lỗi padding.
- Replay attack do packet cũ có thể bị gửi lại nhiều lần.
- Log leakage do key và IV bị ghi vào log debug.
- No authentication do Receiver không xác thực danh tính Sender.

## Mitigations

- Không gửi AES key plaintext trong hệ thống thật.
- Sử dụng TLS hoặc cơ chế trao đổi khóa an toàn như Diffie-Hellman.
- Sử dụng AES-GCM hoặc thêm HMAC để kiểm tra tính toàn vẹn dữ liệu.
- Hạn chế ghi key và IV vào log trong môi trường production.
- Thêm nonce hoặc timestamp để giảm nguy cơ replay attack.
- Thêm cơ chế xác thực Sender trước khi nhận dữ liệu.

## Residual risks

Hệ thống vẫn chưa đủ an toàn để triển khai thực tế vì key channel chỉ mang tính mô phỏng học tập và chưa có TLS. Ngoài ra hệ thống chưa có xác thực hai chiều, chưa chống replay đầy đủ và chưa bảo vệ log khỏi truy cập trái phép.
