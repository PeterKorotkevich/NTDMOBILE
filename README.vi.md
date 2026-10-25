# 💧 Nước Trong

**Ứng dụng giúp ai cũng có thể theo dõi và hiểu về nước mình đang dùng — ngay trên chiếc điện thoại.**

Nước Trong được làm cho những nơi ít có điều kiện tiếp cận phòng xét nghiệm và kiến thức chuyên môn về nước: vùng sâu, vùng xa, nông thôn, miền núi. Chỉ cần một chiếc điện thoại, bạn có thể chụp lại kết quả từ thiết bị đo, lưu lại kèm vị trí, hỏi trợ lý khi chưa hiểu, và đọc những hướng dẫn đơn giản bằng tiếng Việt.

> ⚠️ **Lưu ý quan trọng:** Nước Trong là công cụ hỗ trợ theo dõi và học hỏi, **không thay thế xét nghiệm chính thức**. Ứng dụng không thể khẳng định nước là an toàn để uống. Khi nghi ngờ nước bị ô nhiễm hoặc có người bị ảnh hưởng sức khỏe, hãy liên hệ cơ quan y tế hoặc đơn vị cấp nước địa phương.

---

## ✨ Ứng dụng làm được gì?

Ứng dụng có 5 mục ở thanh dưới cùng:

| Mục | Dùng để làm gì |
|---|---|
| 🏠 **Trang chủ** | Đọc các chủ đề dễ hiểu về nước (độ pH, khoáng chất trong nước uống...). Có ô tìm kiếm. |
| 🤖 **Trợ lý** | Hỏi đáp về chất lượng nước bằng chữ, có thể gửi kèm ảnh mẫu nước hoặc ảnh thiết bị đo. |
| 📷 **Quét** | Chụp ảnh (hoặc chọn ảnh có sẵn) từ thiết bị đo. Ứng dụng đọc chữ trong ảnh và cho bạn sửa lại nếu đọc sai. |
| 🕘 **Lịch sử** | Xem lại các lần quét đã lưu, xoá bản ghi, và xem vị trí đo trên bản đồ. |
| 📖 **Hướng dẫn** | Cách xử lý nước (đun sôi, lọc, khử trùng bằng clo, tia UV), cách quan sát màu, pH, mùi, độ đục, và cách sơ cứu khi nghi ngộ độc. |

### Cách một lần quét hoạt động

1. Mở mục **Quét**, chụp ảnh thiết bị đo (hoặc chọn ảnh từ thư viện).
2. Ảnh được gửi tới máy chủ để **nhận dạng chữ** (OCR).
3. Bạn xem kết quả và **tự chỉnh sửa** nếu có chỗ đọc sai.
4. Ứng dụng tự lấy **vị trí GPS** nơi chụp (nếu bạn cho phép).
5. Bấm lưu. Bản ghi nằm trong **Lịch sử**, kèm ảnh, giờ chụp và vị trí.
6. Trong **Lịch sử**, bạn có thể mở bản đồ để xem một điểm hoặc tất cả các điểm đã quét.

---

## 🧰 Công nghệ

- **React Native 0.85** và **TypeScript**, chạy trên Android và iOS
- Điều hướng: React Navigation (thanh tab và ngăn xếp màn hình)
- Camera: `react-native-vision-camera`
- Vị trí: `@react-native-community/geolocation`
- Bản đồ: Leaflet và OpenStreetMap, hiển thị trong `react-native-webview`
- Lưu trữ cục bộ: `@react-native-async-storage/async-storage`
- Gọi máy chủ: `axios`

---

## 🔌 Máy chủ (backend)

Ứng dụng cần một máy chủ riêng cho hai việc: nhận dạng chữ và trợ lý hỏi đáp. **Mã nguồn máy chủ không nằm trong repo này.** Ứng dụng đọc địa chỉ máy chủ từ biến `SERVER_URL` và gọi hai đường dẫn sau:

**`POST {SERVER_URL}/ocr`**
```json
// Gửi
{ "image_base64": "<ảnh mã hoá base64>" }
// Nhận
{ "text": "<văn bản nhận dạng được>" }
```

**`POST {SERVER_URL}/chat`**
```json
// Gửi (image_url là tuỳ chọn)
{ "message": "<câu hỏi>", "image_url": "data:image/jpeg;base64,..." }
// Nhận
{ "reply": "<câu trả lời>" }
```

---

## 🚀 Chạy thử trên máy của bạn

### Yêu cầu

- Node.js **22.11 trở lên**
- Môi trường React Native đã cài sẵn ([hướng dẫn chính thức](https://reactnative.dev/docs/set-up-your-environment)): Android Studio cho Android, Xcode và CocoaPods cho iOS

### Các bước

```sh
# 1. Cài thư viện
npm install

# 2. Tạo file .env ở thư mục gốc, ghi địa chỉ máy chủ của bạn
echo "SERVER_URL=https://dia-chi-may-chu-cua-ban" > .env

# 3. Chạy Metro
npm start

# 4a. Chạy trên Android (mở terminal khác)
npm run android

# 4b. Hoặc chạy trên iOS (lần đầu cần cài pods)
bundle install
bundle exec pod install --project-directory=ios
npm run ios
```

> Nếu đổi giá trị trong `.env`, hãy chạy lại với `npm run start:fresh` để xoá bộ nhớ đệm.

### Quyền ứng dụng cần xin

| Quyền | Lý do |
|---|---|
| Camera | Chụp ảnh thiết bị đo |
| Thư viện ảnh | Chọn ảnh có sẵn |
| Vị trí | Lưu nơi chụp vào lịch sử |
| Internet | Gọi máy chủ OCR, trợ lý và tải bản đồ |

---

## 📌 Giới hạn hiện tại

- **Cần mạng Internet** để nhận dạng chữ, dùng trợ lý và hiển thị bản đồ.
- Kết quả OCR là **văn bản thô**; ứng dụng chưa tự tách riêng từng chỉ số (pH, TDS, nhiệt độ...) và chưa đưa ra cảnh báo.
- Lịch sử quét được lưu **trên chính điện thoại**, chưa đồng bộ lên máy chủ.
- Bản đồ chỉ **hiển thị các điểm đã quét**, chưa phân tích khu vực ô nhiễm.
- Độ chính xác của OCR chưa có bộ kiểm thử đi kèm trong repo.

## 🗺️ Hướng phát triển

- Tách chỉ số từ văn bản OCR và đối chiếu với ngưỡng tham khảo
- Cảnh báo khi chỉ số bất thường
- Chạy được khi không có mạng
- Đồng bộ dữ liệu và tổng hợp theo khu vực
- Thêm ngôn ngữ cho đồng bào dân tộc thiểu số

---

## 👤 Tác giả

**Nguyễn Tấn Đạt** — Trường THPT Chuyên Đại học Vinh, Nghệ An, Việt Nam.

Dự án có liên quan tới đề tài nghiên cứu *"A Low-Cost OCR- and AI-Assisted Framework for Smartphone-Based Water Quality Monitoring"* (WaterOCR-AI++), là một hướng nghiên cứu riêng về đo màu que thử bằng điện thoại.
