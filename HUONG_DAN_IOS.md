# Hướng Dẫn: Tạo & Cài App iOS Từ Windows (Không Cần Mac, Không Cần Tài Khoản Apple Developer Trả Phí)

Quy trình gồm 3 giai đoạn: **(A) Bọc app thành project iOS** → **(B) Nhờ máy Mac ảo miễn phí của GitHub build ra file .ipa** → **(C) Dùng AltStore để ký & cài .ipa vào iPhone (thay cho 3uTools)**.

⚠️ Lưu ý trung thực: đây là quy trình dùng công cụ cộng đồng (không phải kênh chính thức của Apple), có vài bước kỹ thuật. Mình đã chuẩn bị sẵn toàn bộ file cấu hình, nhưng vì không có máy Mac/Xcode để tự chạy thử tại đây, nếu gặp lỗi ở bước build bạn có thể copy dòng lỗi gửi lại để mình hỗ trợ tiếp.

---

## GIAI ĐOẠN A — Chuẩn bị project (làm trên Windows)

1. Cài **Node.js** (bản LTS) tại https://nodejs.org nếu máy chưa có.
2. Giải nén toàn bộ thư mục `ios_wrap` (mình gửi kèm) ra 1 chỗ, ví dụ `D:\ios_wrap`.
3. Mở **Command Prompt** tại thư mục đó (gõ `cmd` vào thanh địa chỉ File Explorer rồi Enter), chạy:
   ```
   npm install
   ```
   Lệnh này tải các gói Capacitor cần thiết (cần internet).

## GIAI ĐOẠN B — Build file .ipa bằng máy Mac ảo miễn phí (GitHub Actions)

4. Tạo tài khoản GitHub miễn phí tại https://github.com (nếu chưa có).
5. Tạo 1 **repository** mới (Public hoặc Private đều được), ví dụ đặt tên `so-thu-chi-ios`.
6. Đẩy toàn bộ thư mục `ios_wrap` lên repo đó. Cách dễ nhất nếu không quen dòng lệnh Git:
   - Cài **GitHub Desktop** (desktop.github.com) → File > Add local repository → chọn thư mục `ios_wrap` → Publish repository.
7. Vào repo trên GitHub (trên trình duyệt) → tab **Actions** → sẽ thấy workflow "Build unsigned iOS app (.ipa)" → bấm **Run workflow** để chạy thủ công (hoặc nó tự chạy khi bạn push code).
8. Đợi khoảng 5–10 phút cho máy Mac ảo build xong (theo dõi tiến trình ngay trong tab Actions).
9. Khi chạy xong (dấu tích xanh), bấm vào lần chạy đó → kéo xuống mục **Artifacts** → tải file **SoThuChi-ipa.zip** về → giải nén ra sẽ có file **SoThuChi.ipa**. Đây chính là file app cần cài.

## GIAI ĐOẠN C — Cài .ipa vào iPhone bằng AltStore (thay cho 3uTools)

> Vì sao dùng AltStore thay vì 3uTools: 3uTools cần file .ipa đã được **ký (sign)** bằng chứng chỉ Apple hợp lệ mới cài được, còn file build ở trên là bản **chưa ký**. AltStore tự động ký lại bằng Apple ID miễn phí của bạn ngay khi cài — đây là cách anh em cộng đồng vẫn dùng để cài app không qua App Store mà không cần tài khoản Developer trả phí.

10. Trên Windows, cài **iTunes** (bản từ trang chủ Apple, không phải bản Microsoft Store) — AltServer cần nó để nhận diện iPhone.
11. Tải **AltServer** cho Windows tại: https://altstore.io → cài đặt, chạy AltServer (icon sẽ nằm ở khay hệ thống, góc dưới phải màn hình).
12. Cắm iPhone vào máy tính bằng cáp, mở khoá máy, bấm "Tin cậy" (Trust) máy tính này nếu được hỏi.
13. Bấm chuột phải vào icon AltServer ở khay hệ thống → **Install AltStore** → chọn iPhone của bạn → nhập **Apple ID và mật khẩu** (dùng Apple ID thường, không cần mua Developer Program). AltStore sẽ được cài lên điện thoại.
14. Trên iPhone: vào **Cài đặt > Cài đặt chung > VPN & Quản lý thiết bị** → tin cậy chứng chỉ ứng với Apple ID của bạn.
15. Mở app **AltStore** trên iPhone → tab **My Apps** → bấm dấu **+** góc trên → chọn file **SoThuChi.ipa** (chuyển file này vào iPhone trước qua cáp/AirDrop/iCloud, hoặc dùng AltServer trên Windows: chuột phải icon khay hệ thống lúc iPhone đang cắm dây → có thể cài trực tiếp từ máy tính).
16. App sẽ xuất hiện trên màn hình chính iPhone như app thường.

### Lưu ý quan trọng
- Với Apple ID miễn phí, app sẽ **tự hết hạn sau 7 ngày** và cần ký lại. AltServer sẽ tự làm mới nếu điện thoại và máy tính (đang mở AltServer) **cùng kết nối 1 mạng WiFi** — không cần cắm dây lại, chỉ cần máy tính bật và mở AltServer.
- Nếu muốn không phụ thuộc máy tính (app tự làm mới qua mạng di động luôn), có thể tìm hiểu thêm **SideStore** — một biến thể của AltStore hoạt động độc lập hơn trên iPhone.
- Đây không phải kênh phân phối chính thức của Apple, chỉ nên dùng cho app cá nhân tự làm như thế này.

---

## Nếu gặp lỗi ở bước build (Giai đoạn B)

Lỗi thường gặp và cách xử lý:
- `pod install` báo lỗi thiếu Podfile → thử chạy lại `npx cap sync ios` trước khi push.
- Lỗi liên quan `CocoaPods` version → có thể cần thêm bước `sudo gem install cocoapods` vào workflow (báo mình dòng lỗi cụ thể, mình sẽ chỉnh lại file workflow).
- Build ra `.app` nhưng `.ipa` mở không được trong AltStore → báo lỗi cụ thể AltStore hiện lên, mình sẽ tra hướng khắc phục.

Bạn cứ làm theo từng bước, gặp lỗi ở đâu thì gửi mình ảnh chụp màn hình hoặc nội dung lỗi ở bước đó, mình sẽ hỗ trợ tiếp.
