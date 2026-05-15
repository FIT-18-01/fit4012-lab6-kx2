# Report 1 page - Lab 6 AES-CBC Socket

## Thông tin nhóm

- Thành viên 1: Nguyễn Trung Kiên
- Thành viên 2: Hoàng Nhật Anh

## Mục tiêu

Mục tiêu của bài lab là xây dựng hệ thống gửi và nhận dữ liệu qua TCP socket bằng cơ chế mã hóa AES-CBC. Hệ thống được tách thành hai kênh riêng gồm key channel để truyền AES key và IV, và data channel để truyền ciphertext. Nhóm triển khai thêm PKCS#7 padding để đảm bảo dữ liệu phù hợp với block size của AES. Ngoài việc gửi và nhận dữ liệu thành công, bài lab còn yêu cầu viết test kiểm tra padding, key channel, data channel, tình huống sai key và chỉnh sửa ciphertext. Qua đó nhóm hiểu rõ hơn về cách hoạt động của socket, mã hóa đối xứng và các điểm yếu bảo mật khi truyền key dạng plaintext.

## Phân công thực hiện

- Nguyễn Trung Kiên phụ trách chính phần sender.py, receiver.py, xử lý AES-CBC, padding PKCS#7, socket communication và hoàn thiện README.
- Hoàng Nhật Anh hỗ trợ kiểm tra test case, chuẩn bị sample input/output và rà soát log.
- Cả hai cùng thảo luận về threat model, ethics và kiểm tra kết quả chạy thực tế.

## Cách làm

Sender tạo AES key và IV ngẫu nhiên bằng os.urandom, sau đó mã hóa plaintext bằng AES-CBC thông qua thư viện PyCryptodome. Dữ liệu trước khi mã hóa được thêm PKCS#7 padding để đảm bảo độ dài là bội số của 16 byte. Key channel sử dụng packet có cấu trúc gồm 4 byte độ dài key, tiếp theo là key và IV. Data channel sử dụng 4 byte length header để xác định kích thước ciphertext trước khi nhận dữ liệu. Receiver lắng nghe lần lượt ở KEY_PORT và DATA_PORT, sau đó parse packet, giải mã ciphertext và ghi nội dung ra file output nếu được yêu cầu.

## Kết quả

Hệ thống chạy thành công trong môi trường local với hai terminal riêng cho Sender và Receiver. Receiver nhận đúng AES key, IV và ciphertext, sau đó giải mã lại chính xác plaintext ban đầu. File sample_output.txt khớp với dữ liệu trong sample_input.txt. Các log sender_success.log và receiver_success.log ghi lại quá trình gửi nhận dữ liệu và thông tin packet. Bộ test pytest chạy thành công với các trường hợp kiểm tra padding, parse header, key channel, data channel, wrong key, tamper ciphertext và sender-receiver local.

## Kết luận

Qua bài lab, nhóm hiểu rõ hơn về quy trình truyền dữ liệu mã hóa qua socket TCP và cách triển khai AES-CBC trong Python. Việc tách key channel và data channel giúp minh họa luồng truyền dữ liệu trong hệ thống mạng. Tuy nhiên, bài lab cũng cho thấy chỉ dùng AES-CBC là chưa đủ an toàn vì key vẫn bị gửi plaintext và ciphertext chưa có cơ chế xác thực toàn vẹn. Trong hệ thống thực tế cần sử dụng TLS hoặc AES-GCM để tăng mức độ bảo mật.
