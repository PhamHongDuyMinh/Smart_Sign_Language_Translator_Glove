# Găng Tay Phiên Dịch Ngôn Ngữ Ký Hiệu (Smart Sign Language Translator Glove) 🖐️🤖

> **Đồ án môn học:** Kỹ thuật Vi xử lý  
> **Trường:** Đại học Bách Khoa Hà Nội (HUST)  
> **Sinh viên:** Phạm Hồng Duy Minh - 20234025
> **GVHD:** [Điền Tên Thầy Cô Vào Đây]

---

## 📖 Giới thiệu (Overview)
Dự án này là một thiết bị đeo thông minh (Wearable Device) có khả năng chuyển đổi các cử chỉ tay của ngôn ngữ ký hiệu thành văn bản hiển thị trên màn hình OLED. Hệ thống sử dụng vi điều khiển **ESP32** kết hợp với thuật toán học máy **Random Forest (Edge AI)** để phân loại cử chỉ trong thời gian thực với độ trễ thấp (< 200ms).

### 🚀 Tính năng nổi bật
- **Edge AI:** Chạy mô hình Random Forest trực tiếp trên ESP32 (Offline).
- **Độ chính xác cao:** Kết hợp cảm biến uốn (Flex) và cảm biến gia tốc (IMU).
- **Tiết kiệm năng lượng:** Tối ưu hóa với FreeRTOS và chế độ ngủ.
- **Hiển thị trực quan:** Màn hình OLED hiển thị ký tự và dung lượng Pin.

---

## 🛠️ Phần cứng & Kết nối (Hardware)

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

---

## 💻 Cài đặt & Nạp Code (Installation)

Dự án được phát triển trên nền tảng **ESP-IDF**. Vui lòng làm theo các bước sau để biên dịch.

### Yêu cầu phần mềm
- **Visual Studio Code**.
- **Espressif IDF Extension** (trên VS Code).
- **Python 3.8+** (để chạy script training AI).

### Các bước thực hiện

**1. Clone dự án về máy:**
```bash
git clone [https://github.com/PhamHongDuyMinh/Smart_Sign_Language_Translator_Glove.git](https://github.com/PhamHongDuyMinh/Smart_Sign_Language_Translator_Glove.git)
cd Smart_Sign_Language_Translator_Glove
