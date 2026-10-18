# 🌿 Báo Cáo & Tài Liệu Dự Án GreenTech (Session 10 - Ex 01)

> **Dự án:** GreenTech — Hệ thống Nông nghiệp Thông minh  
> **Lớp:** [Tên Lớp của bạn] | **Môn Học:** [Tên Môn Học] | **Mã Bài Tập:** `Session10_Ex01`

---

## 📌 PHẦN 1: PHÂN TÍCH PLAIN TEXT VS RICH TEXT & GIT DIFF

### 1. Hai Sai Lầm Trong Tư Duy Khi Dùng Rich Text (`.docx`) Cho Dự Án Mã Nguồn
* **Sai lầm 1: Nhầm lẫn giữa tệp nhị phân mã hóa (Binary/Rich Text) và văn bản mã nguồn thuần túy (Plain Text).**
  File `.docx` thực chất là một lưu lưu trữ ZIP chứa nhiều file mã hóa XML, cấu hình định dạng, và siêu dữ liệu (metadata). Git được thiết kế để theo dõi thay đổi theo từng dòng chữ (*line-by-line*). Do file `.docx` là dạng nhị phân mã hóa, Git không thể đọc hay so sánh nội dung bên trong. Khi sửa dù chỉ một dấu phẩy, toàn bộ cấu trúc mã hóa bị thay đổi khiến Git hiểu lầm toàn bộ file đã bị xóa và thay mới, gây ra **Conflict (Xung đột) trên toàn bộ file** khi hợp nhất nhánh.
* **Sai lầm 2: Phụ thuộc vào phần mềm độc quyền để xem/đánh giá nội dung.**
  Tệp Rich Text yêu cầu phần mềm chuyên dụng (Microsoft Word, Google Docs) để mở và render giao diện. Trình duyệt web cũng như giao diện GitHub không thể tự động render nhanh nội dung `.docx`, làm gián đoạn quy trình làm việc nhóm vì thành viên buộc phải tải file về máy local mới xem được.

---

### 2. So Sánh Tính An Toàn Và Dung Lượng giữa Markdown và Word (`.docx`)

| Tiêu chí | Microsoft Word (`.docx` - Rich Text) | Markdown (`.md` - Plain Text) |
| :--- | :--- | :--- |
| **Bản chất dữ liệu** | Tệp nhị phân phức tạp (ZIP/XML). | Văn bản thô thuần túy kèm ký hiệu định dạng. |
| **Dung lượng tệp** | Nặng (vài chục KB đến vài MB). | Rất nhẹ (vài KB). |
| **Lỗi đứt gãy dữ liệu** *(Mất 1 byte do sự cố mạng)* | **Hỏng hoàn toàn (Corrupted):** Phá vỡ cấu trúc ZIP/XML khiến phần mềm không thể giải nén/mở file. | **An toàn tuyệt đối:** Chỉ ảnh hưởng đúng 1 ký tự bị mất, toàn bộ nội dung còn lại vẫn hiển thị bình thường. |
| **Hỗ trợ Git Diff** | Không (Chỉ coi là file Binary). | **Có (Hoàn hảo):** Theo dõi chính xác từng dòng được thêm, bớt hoặc chỉnh sửa. |

---

## 📌 PHẦN 2: HOÀN THIỆN TÀI LIỆU DỰ ÁN GREENTECH

# 🌿 GreenTech - Hệ Thống Nông Nghiệp Thông Minh

**GreenTech** là giải pháp IoT tự động hóa chăm sóc cây trồng nhằm tối ưu hóa tài nguyên nước, giám sát vi khí hậu và hướng tới nền nông nghiệp bền vững.

---

## 📋 Giới thiệu chung
Dự án được nghiên cứu và phát triển bởi nhóm sinh viên **Học viện Công nghệ Bưu chính Viễn thông (PTIT)**. Hệ thống tích hợp các cảm biến thu thập dữ liệu môi trường và điều khiển thiết bị ngoại vi tự động qua Cloud.

---

## ✨ Tính năng cốt lõi

- [x] **Giám sát thời gian thực:** Thu thập nhiệt độ, độ ẩm không khí và độ ẩm đất 24/7.
- [x] **Tự động hóa thông minh:** Tự động bật/tắt máy bơm tưới dựa trên ngưỡng độ ẩm cài đặt.
- [x] **Cảnh báo qua ứng dụng:** Gửi thông báo khẩn cấp khi vi khí hậu vượt ngưỡng an toàn.
- [ ] **Dự đoán thời tiết:** Tích hợp mô hình AI dự báo lượng mưa (Đang phát triển v2.0).

---

## 📊 Bảng thông số kỹ thuật phần cứng

| Linh kiện / Thiết bị | Mã hiệu (Model) | Chức năng chính | Thông số vận hành |
| :--- | :--- | :--- | :--- |
| **SoC Điều khiển** | ESP32 NodeMCU | Xử lý trung tâm & Kết nối Wi-Fi/Bluetooth | 3.3V DC, 240MHz |
| **Cảm biến độ ẩm đất** | Capacitive v1.2 | Đo hàm lượng nước trong đất | Signal Analog (0 - 3.3V) |
| **Cảm biến vi khí hậu** | DHT22 | Đo nhiệt độ và độ ẩm không khí | -40 đến 80°C (±0.5°C) |
| **Rơ-le điều khiển** | Relay 2 Kênh 5V | Đóng/mở nguồn máy bơm tưới | 10A - 250V AC |

---

## ⚙️ Quy trình xử lý dữ liệu (Mô hình DIKW)

1. **Data (Dữ liệu thô):** Cảm biến gửi các giá trị số `ADC: 2400`, `Temp: 34°C`.
2. **Information (Thông tin):** Hệ thống xác định độ ẩm đất đang ở mức **35%** (Rất khô) và nhiệt độ môi trường **34°C** (Nóng).
3. **Knowledge (Tri thức):** Phân tích nhận định cây trồng đang bị mất nước nghiêm trọng và cần được bổ sung nước ngay lập tức để tránh héo.
4. **Wisdom (Trí tuệ):** Kích hoạt máy bơm phun sương tự động, đồng thời lên lịch tưới bổ sung vào lúc 18:00 để tránh sốc nhiệt cho cây.

---

## 🛠️ Hướng dẫn cài đặt nhanh

```bash
# Clone dự án từ GitHub
git clone https://github.com/nam-ptit/greentech.git

# Di chuyển vào thư mục dự án
cd greentech
```