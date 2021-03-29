
### STEP1.  Cách 1: Đăng ký thiết bị sử dụng Web Google

1.1. Mở trang https://console.actions.google.com/u/0/project/project_id/deviceregistration/ với project_id là project_id vừa lưu ở 2.1.1.

và điền lần lượt từng mục

![ĐĂNG KÝ THIẾT BỊ](https://developers.google.com/assistant/sdk/images/console/device-models-aog.png)

![ĐĂNG KÝ THIẾT BỊ](https://user-images.githubusercontent.com/64348125/109378336-3f136d80-7904-11eb-808e-37bf5c726bf3.png)

1.1.1. Product Name: Gõ tùy ý

1.1.2. Manufacturer name: Gõ  tùy ý

1.1.3. Device Type: Chọn Speaker

1.1.4. Device Model ID: Để mặc định hoặc tùy chọn. Nhớ lưu lại thông tin để dùng sau

1.1.5. Bấm Register Model

1.2. Download file về máy

1.3. Cửa sổ mới mở ra, chọn Download OAuth 2.0 credentials

![ĐĂNG KÝ THIẾT BỊ](https://user-images.githubusercontent.com/64348125/109378347-56525b00-7904-11eb-9764-c2af673d9ac4.png)


1.4. File .json được lưu về máy, giữ nguyên File không đổi tên 

1.5. Copy file json vừa download được sang thư mục của loa thông minh tại đường dẫn
```sh
 /home/pi
```
1.6. Có thể lấy lại file json bằng cách vào lại bước 2.1, chọn Download OAuth 2.0 credentials

![LẤY LẠI FILE](https://developers.google.com/assistant/sdk/images/console/edit-model.png)

### STEP2. Kích hoạt Google Assistant trên loa thông minh

2.1. Truy nhập SSH của Raspberry Pi

2.1.1 Gõ lệnh sau

```sh
google-oauthlib-tool --scope https://www.googleapis.com/auth/assistant-sdk-prototype \
      --scope https://www.googleapis.com/auth/gcm \
      --save --headless --client-secrets /home/pi/client_secret_client-id.json

```
với client_secret_client-id.json là tên file json vừa lưu ở /home/pi theo bước 2.2.3.

2.1.2. Kết quả dòng lệnh sẽ trả về có dạng

```sh
Please visit this URL to authorize this application: https://...
```
2.2. Lấy mã từ Google

2.2.1. Copy toàn bộ đường link bắt đầu từ https:// sau đó dán vào trình duyệt trên máy PC, 

2.2.2. Cửa sổ đăng nhập hiện ra, đăng nhập vào tài khoản Google (Là tài khoản duy nhất từ Step 1) sau đó bấm Allow(Cho phép) và Tiếp tục(Continue) để cho phép quyền truy cập vào tài khoản từ App Google Action

2.2.3. Sau khi chấp thuận, một mã sẽ hiện ra có dạng

```sh
4/XXXX
```
2.2.4. Copy mã trên vào cửa sổ dòng lệnh còn đang chạy trên Raspberry Pi tại mục:

```sh
Enter the authorization code:

```
2.2.5. Nếu toàn bộ các bước trên đúng, hệ thống sẽ gen ra một file có tên là credentials.json, nằm trong thư mục ẩn .config tại đường dẫn /home/pi/.config/google-oauthlib-tool/

theo thông báo trên console

```sh
credentials saved: /path/to/.config/google-oauthlib-tool/credentials.json

```
Chú ý, không được xóa, đổi tên file này trong quá trình sử dụng Google Assistant

2.2.6. Trong trường hợp báo lỗi InvalidGrantError, là do mã copy vào theo bước 3.1.5. bị sai, cần phải lặp lại từ 3.1. Chú ý mã copy không có khoảng trắng, khi select bằng chuột có thể có khoảng trắng

2.3. Trong trường hợp muốn dùng Account Google khác

2.3.1. Thay Acc khác

Xóa thư mục trên bằng lệnh

```sh
sudo rm -rf/home/pi/.config/google-oauthlib-tool/credentials.json

```
Chạy lại toàn bộ các bước từ 1.1. đến 1.2 để tạo được file credentials.json mới

2.3.2. Dùng nhiều Acc

Đổi tên file .json hiện tại bằng lệnh

```sh
sudo cp /home/pi/.config/google-oauthlib-tool/credentials.json /home/pi/.config/google-oauthlib-tool/credentials_1.json

```
Chạy lại toàn bộ các bước từ 1.1. đến 1.2 để tạo được file credentials.json mới

Lặp lại bước 2.3.2 để tạo ra file credentials_x.json

Muốn dùng Acc nào thì tạo file credentials.json từ file đó

```sh
sudo cp /home/pi/.config/google-oauthlib-tool/credentials_x.json /home/pi/.config/google-oauthlib-tool/credentials.json

```

### STEP3. Kích hoạt thiết bị để chạy được Google Assistant

3.1. Chạy ứng dụng gốc để kích hoạt thiết bị như sau:

```sh
python3 googlesamples-assistant-pushtotalk --project-id ten_project --device-model-id ten_device_id
```
với project_id và device_model_id là giá trị đã lưu lại khi khởi tạo project

Chương trình sẽ hiển thị như sau
```sh
pygame 1.9.4.post1
Hello from the pygame community. https://www.pygame.org/contribute.html
INFO:root:Connecting to embeddedassistant.googleapis.com
WARNING:root:Device config not found: [Errno 2] No such file or directory: '/home/pi/.config/googlesamples-assistant/device_config.json'
INFO:root:Registering device
INFO:root:Device registered: xxxxxxx-xxxxxxx-xxxxxxx-xxxxxxx-xxxxxxx

```
3.2. Kiểm tra kết quả
Sau khi khởi chạy, hệ thống sẽ tạo một thư mục có file là device_config.json nằm trong thư mục googlesamples-assistant
```sh
/home/pi/.config/googlesamples-assistant/device_config.json
```
### STEP4. Kết thúc

6.1. Có thể chạy Google Assistant bằng lệnh:

```sh
python3 googlesamples-assistant-pushtotalk --project-id ten_project --device-model-id ten_device_id
```
với project_id và device_model_id là giá trị đã lưu lại khi khởi tạo project
