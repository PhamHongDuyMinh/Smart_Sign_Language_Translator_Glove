# Găng Tay Phiên Dịch Ngôn Ngữ Ký Hiệu (Smart Sign Language Translator Glove) 

> **Đồ án môn học:** Kỹ thuật Vi xử lý
> **Trường:** Đại học Bách Khoa Hà Nội (HUST)
> **GVHD:** Hàn Huy Dũng
>
> **Nhóm sinh viên thực hiện:**
> - Phạm Hồng Duy Minh   - 20234025
> - Dương Ngọc Hoài Anh  - 20233991
> - Trần Phi Anh Nhật    - 20234029

## Giới thiệu (Overview)
Dự án này là một thiết bị đeo thông minh (Wearable Device) có khả năng chuyển đổi các cử chỉ tay của ngôn ngữ ký hiệu thành văn bản hiển thị trên màn hình OLED. Hệ thống sử dụng vi điều khiển **ESP32** kết hợp với thuật toán học máy **Random Forest (Edge AI)** để phân loại cử chỉ trong thời gian thực với độ trễ thấp (< 200ms).

###  Tính năng nổi bật
- **Edge AI:** Chạy mô hình Random Forest trực tiếp trên ESP32 (Offline).
- **Độ chính xác cao:** Kết hợp cảm biến uốn (Flex) và cảm biến gia tốc (IMU).
- **Tiết kiệm năng lượng:** Tối ưu hóa với FreeRTOS và chế độ ngủ.
- **Hiển thị trực quan:** Màn hình OLED hiển thị ký tự và dung lượng Pin.

##  Phần cứng & Kết nối (Hardware)

### Danh sách linh kiện
1.  **MCU:** ESP32 Dev Kit V1 (38 chân).
2.  **Cảm biến uốn (Flex Sensors):** 3 chiếc (Ngón Cái, Trỏ, Giữa).
3.  **IMU:** MPU6050 (Gia tốc & Góc nghiêng).
4.  **Hiển thị:** OLED SSD1306 (0.96 inch, I2C).
5.  **Nguồn:** Pin Li-ion 3.7V + Mạch sạc TP4056.

### Sơ đồ nối dây (Pinout)

| Linh kiện | Chân linh kiện | Chân ESP32 | Ghi chú |
| :--- | :---: | :---: | :--- |
| **MPU6050** | SDA | GPIO 21 | Giao tiếp I2C |
| | SCL | GPIO 22 | Giao tiếp I2C |
| **OLED** | SDA | GPIO 21 | Chung bus I2C |
| | SCL | GPIO 22 | Chung bus I2C |
| **Flex 1** | Output | GPIO 34 | Analog Input (Ngón cái) |
| **Flex 2** | Output | GPIO 35 | Analog Input (Ngón trỏ) |
| **Flex 3** | Output | GPIO 32 | Analog Input (Ngón giữa) |
| **Battery** | V_Div | GPIO 33 | Đo điện áp Pin |

##  Cài đặt & Nạp Code (Installation)

Dự án được phát triển trên nền tảng **ESP-IDF**. Vui lòng làm theo các bước sau để biên dịch.

### Yêu cầu phần mềm
- **Visual Studio Code**.
- **Espressif IDF Extension** (trên VS Code).
- **Python 3.8+** (để chạy script training AI).

### Các bước thực hiện

Mở Terminal và chạy lần lượt các lệnh sau để tải dự án, cấu hình và nạp chương trình vào ESP32:

    # 1. Tải dự án về máy
    git clone [https://github.com/PhamHongDuyMinh/Smart_Sign_Language_Translator_Glove.git](https://github.com/PhamHongDuyMinh/Smart_Sign_Language_Translator_Glove.git)

    # 2. Di chuyển vào thư mục dự án
    cd Smart_Sign_Language_Translator_Glove

    # 3. Cấu hình menuconfig (Tùy chọn)
    idf.py menuconfig

    # 4. Biên dịch, nạp code và mở Serial Monitor
    idf.py build flash monitor

##  Hướng dẫn vận hành (Hardware User Manual)

Để hệ thống hoạt động chính xác nhất, người dùng cần tuân thủ quy trình khởi động để cảm biến tự cân chỉnh (Calibration).

### 1. Quy trình khởi động (Startup Sequence)
1.  **Đeo găng tay:**
    * Đeo găng tay vào tay phải (hoặc tay trái tùy theo thiết kế).
    * Đảm bảo các **cảm biến Flex** nằm chính xác dọc theo sống lưng các ngón tay.
    * Siết dây đai cổ tay để cố định hộp xử lý trung tâm.
2.  **Bật nguồn:** Gạt công tắc (Switch) trên hộp thiết bị sang vị trí **ON**.
3.  **Cân chỉnh tự động (Calibration - QUAN TRỌNG):**
    * Ngay sau khi bật nguồn, màn hình OLED sẽ hiện logo khởi động.
    * **Giữ bàn tay DUỖI THẲNG và THẢ LỎNG trong 3-5 giây.**
    * Lúc này, cảm biến MPU6050 sẽ lấy mẫu môi trường để xác định góc "0" (Zero point). Khi màn hình hiện chữ **"READY"** (hoặc giao diện chính), bạn có thể bắt đầu sử dụng.

### 2. Cách thực hiện ký tự
* **Thao tác:** Uốn các ngón tay và xoay cổ tay theo bảng chữ cái ngôn ngữ ký hiệu đã được học (A, B, D, G...).
* **Hiển thị:**
    * Ký tự nhận diện được sẽ hiển thị to rõ giữa màn hình OLED.
    * Nếu hệ thống chưa nhận diện được (độ tin cậy thấp), màn hình có thể hiện dấu `?` hoặc giữ nguyên ký tự cũ.

### 3. Sạc Pin (Charging)
Hệ thống tích hợp module quản lý sạc **TP4056**.
* **Cắm sạc:** Sử dụng cáp Micro-USB cắm vào cổng sạc bên hông hộp.
* **Đèn báo:**
    *  **Đèn Đỏ:** Đang sạc.
    *  **Đèn Xanh:** Pin đầy (Ngắt sạc để bảo vệ pin).

##  Lưu ý an toàn
* **Cảm biến Flex:** Rất nhạy cảm với việc uốn ngược chiều. **Tuyệt đối không bẻ ngược** ngón tay găng về phía sau, có thể làm gãy mạch than bên trong cảm biến.
* **Kết nối:** Kiểm tra kỹ các jack cắm Jumper trước khi sử dụng để tránh tín hiệu bị chập chờn.
* **Nguồn điện:** Không sử dụng găng tay khi đang cắm sạc để đảm bảo an toàn tuyệt đối.

##  Cấu trúc thư mục (Folder Structure)

    ├── main/                  # Mã nguồn Firmware ESP32 (C/C++)
    │   ├── model.h            # Mô hình Random Forest đã huấn luyện
    │   └── test_oled.cpp      # Chương trình chính (Main Logic)
    ├── training/              # Mã nguồn Python & Dữ liệu AI
    │   ├── gesture_data.csv   # Dataset (Dữ liệu thô)
    │   └── train_ai.py        # Script Training (Scikit-learn)
    ├── CMakeLists.txt         # Cấu hình Build ESP-IDF
    └── README.md              # Tài liệu hướng dẫn

