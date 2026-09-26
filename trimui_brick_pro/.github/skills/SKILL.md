# 🛠️ Tệp Kỹ Năng AI (AI Skill Context): TrimUI Brick Pro - ROM Directory

## Ngữ cảnh dự án (Project Overview)
Mục tiêu: Xây dựng một trang web tĩnh (Single Page Application) nhúng trên Google Sites, dùng để tra cứu cấu trúc thư mục ROM, đuôi file, và thông tin chi tiết các hệ máy/giả lập cho máy chơi game TrimUI Brick Pro[cite: 5].

Nguồn dữ liệu: Đọc dữ liệu trực tiếp từ file Google Sheet thông qua API xuất file CSV công khai (`/gviz/tq?tqx=out:csv`)[cite: 5].

Môi trường: HTML, CSS, Vanilla JavaScript[cite: 5].
Cấu trúc dự án bao gồm: File `index.html` và file `style.css`[cite: 5]. Code JS được nhúng trực tiếp trong file HTML[cite: 5]. Riêng code CSS được serve ở file `style.css`[cite: 5]. Thư mục fonts chứa resource font[cite: 5].

## Cấu trúc dữ liệu Google Sheet (Data Schema)
Dữ liệu đầu vào là một mảng 2 chiều đọc từ CSV[cite: 5]. AI cần ghi nhớ chính xác thứ tự các cột sau (index bắt đầu từ 0)[cite: 5]:
- `cols[0]` - Tên hệ máy (Console Name): Tên đầy đủ của máy (VD: Gameboy Advance, FBNM, MAME)[cite: 5].
- `cols[1]` - Tên thư mục (Folder Name): Tên thư mục ROM chuẩn trên thẻ nhớ (VD: GBA, FC, PS)[cite: 5].
- `cols[2]` - Đuôi file (Extensions): Các định dạng ROM hỗ trợ, phân cách nhau bởi dấu `|` hoặc `,` (VD: .gba|.zip)[cite: 5].
- `cols[3]` - Hình ảnh (Picture): ID hoặc Link chia sẻ Google Drive của ảnh minh họa[cite: 5].
- `cols[4]` - Link web (WebLink): URL tham khảo tài liệu hoặc tải ROM[cite: 5].
- `cols[5]` - Mô tả (Description): Thông tin giải thích chi tiết[cite: 5].
- `cols[6]` - Giả Lập (Is Emulator): Nhận giá trị TRUE/FALSE hoặc YES/NO[cite: 5].
- `cols[7]` - Năm hình thành (Year): Dữ liệu số học (VD: 1989, 2004)[cite: 5].

## Lịch sử thay đổi & Nâng cấp (Changelog / History)
AI cần hiểu các thay đổi trong quá khứ để không làm mất tính năng khi viết code mới[cite: 5]:
- **Phase 1:** Code sơ khai chỉ fetch dữ liệu và in ra danh sách tĩnh[cite: 5]. Được thiết kế để chạy qua Live Server (VS Code) nhằm tránh lỗi CORS[cite: 5].
- **Phase 2 (Expander UI):** Đổi giao diện thành cấu trúc Accordion sử dụng thẻ HTML5 `<details>` và `<summary>`, mũi tên xoay ▶ thay vì danh sách mở tĩnh[cite: 5].
- **Phase 3 (Bổ sung Data & Layout):** Thêm 3 cột: Mô tả, Hình ảnh, WebLink[cite: 5]. Bố cục UI bên trong chi tiết được cấu trúc lại: Ảnh minh họa (cố định bên trái) - Mô tả & WebLink (bên phải)[cite: 5]. Tích hợp hàm `formatDriveImageUrl()` tự động chuyển đổi URL Google Drive thành link ảnh trực tiếp (`lh3.googleusercontent.com`)[cite: 5].
- **Phase 4 (Phân loại & Thuật toán sắp xếp):** Thêm cột Giả Lập và Năm[cite: 5]. JS phân tích toàn bộ CSV thành Mảng các Object (Array of Objects), sắp xếp mảng theo năm tăng dần (Sort by Year ASC), rồi mới render ra HTML[cite: 5]. Thêm Badge hiển thị Năm và Loại nền tảng[cite: 5].
- **Phase 5 (Tabs & Cards View - Mới nhất):** 
  - Tái cấu trúc layout chính thành 2 Tabs riêng biệt: `Tab 1` (Tree View nguyên bản) và `Tab 2` (Cards View dạng Grid).
  - Tích hợp logic liên kết Vanilla JS: Cho phép click vào một đối tượng ở dạng Card (Tab 2) để tự động nhảy sang Tab 1, tự động gán thuộc tính `open = true` cho thẻ `<details>` mục tiêu, và dùng `scrollIntoView({ behavior: 'smooth' })` để cuộn trơn tru tới phần tử đó.

## Các quy chuẩn lập trình bắt buộc (Strict Coding Guidelines)
Khi yêu cầu AI chỉnh sửa hoặc thêm tính năng mới, AI phải tuân thủ các quy định sau[cite: 5]:
- **Zero Dependencies:** Luôn sử dụng CSS thuần và Vanilla JS[cite: 5]. Tuyệt đối không nhúng thư viện UI bên thứ 3.
- **Xử lý ảnh an toàn (Image Fallbacks):** Phải giữ lại hàm `formatDriveImageUrl`[cite: 5]. Đối với ảnh ở Tree View, bắt buộc dùng `onerror="this.parentElement.style.display='none'"`[cite: 5]. Đối với ảnh ở Cards View, phải sử dụng logic ghi đè HTML (`innerHTML`) tạo placeholder icon khi ảnh lỗi nhằm đảm bảo lưới Grid không bị sụp.
- **Responsive Design & Layout:** Sử dụng Flexbox cho giao diện bên trong `details-body`[cite: 5]. Đối với Cards View, sử dụng CSS Grid và thiết lập các `@media` queries thu nhỏ linh hoạt (ví dụ: từ 5 cột giảm dần xuống 1 cột) để hiển thị tốt trên Google Sites mobile view[cite: 5].
- **Data Normalization:** Phải có fallback/check null cho mọi dữ liệu từ `cols[]` (VD: `cols[0] || ''`)[cite: 5].
- **Sort Logic:** Dữ liệu trống ở cột "Năm" (không có số hợp lệ) phải được gán giá trị max (VD: 9999) để đẩy xuống cuối danh sách thay vì báo lỗi[cite: 5].

## Mục tiêu của AI khi nhận lệnh (AI Task Protocol)
- **Hiểu yêu cầu:** Phân tích xem yêu cầu mới thuộc về thao tác UI (CSS), cấu trúc dữ liệu (Google Sheet) hay logic xử lý (JS Fetch/Sort)[cite: 5].
- **Không phá vỡ nền tảng:** Bất kỳ sửa đổi nào cũng phải kế thừa phiên bản mới nhất (Phase 5) thay vì Phase 4[cite: 5]. Lưu ý vòng lặp render duy nhất của Mảng dữ liệu (Array of Objects) phải xây dựng chuỗi HTML cho cả tab Tree và tab Cards đồng thời.