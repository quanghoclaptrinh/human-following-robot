# Human Following Robot V9

Robot hai bánh bám một người đã chọn. Raspberry Pi 4 chạy YOLO NCNN, ByteTrack và OSNet. ESP32 DDMR V9 nhận PWM hai bánh và trả dữ liệu encoder qua USB Serial 115200 baud.

## Cấu trúc

- `pi/` chứa chương trình nhận diện, chương trình bám người và bộ điều khiển V9.
- `esp32/` là dự án PlatformIO cho `esp32dev`. Build và nạp từ thư mục này.

## Chạy thử trên Raspberry Pi

Dùng môi trường Python đã chạy được OpenCV, Ultralytics, NumPy, PySerial, PyTorch, torchvision và torchreid. Đặt thư mục YOLO NCNN 320 và checkpoint OSNet-x0.25 MSMT17 cạnh các file Python trong `pi/`.

```bash
cd pi
python pi4_person_osnet_gallery_motor_v9_tuy_chinh.py \
  --camera 0 --model yolo26n_ncnn_model --imgsz 320 \
  --serial none --reid osnet --flip horizontal
```

Sau khi nạp V9 và kiểm tra chiều bánh trên giá đỡ, chạy lại với `--serial /dev/ttyUSB0` và các tham số dưới đây. Dùng đúng cổng USB của ESP32.

```text
--drive-mode follow --pwm-limit 55
--turn-min 55 --turn-max 55 --forward-min 55 --forward-max 55
--reverse-min 0 --reverse-max 0 --target-distance 1.5 --distance-deadband 0.25
--rotate-first 0.20 --turn-exit 0.10 --arc-delta 5 --encoder-kp 0 --encoder-ki 0
```

Click người mục tiêu và chờ GALLERY READY. Đứng cách camera 1,5 m, nhấn 2, chờ CALIB OK rồi nhấn r. Dùng s để dừng và hủy ARM. Trần firmware V9 là 120. Lệnh mẫu dùng trần 55 và tắt bù PI để thử ga trực tiếp.

Firmware có điểm truy cập Wi-Fi thử nghiệm với thông tin mặc định trong main.cpp. Khi dùng trên xe, đặt mật khẩu riêng và kiểm tra quyền truy cập giao diện điều khiển.

Model, venv và ảnh gallery được quản lý riêng trên Pi.
