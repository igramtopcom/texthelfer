# TASK-011 — Homepage Redesign

**Ngày tạo:** 2026-03-13
**PM:** TextHelfer PM Agent
**Assignee:** Dev Agent (Claude CLI)
**Priority:** P1
**Status:** DONE ✅
**Branch:** feature/TASK-011-homepage-redesign
**Estimate:** 1 ngày

---

## 1. CONTEXT

Trang chủ hiện tại (index.html) đã có đủ nội dung SEO và cấu trúc cơ bản, nhưng trải nghiệm người dùng chưa đủ tốt để giữ chân khách ghé lần đầu. Cụ thể: hero section không tạo được ấn tượng ban đầu, card grid chưa phân cấp rõ ràng giữa hero tool và 8 tool còn lại, section "Für wen" trình bày dạng đoạn văn khiến người dùng phải đọc nhiều thay vì scan nhanh, và footer thiếu link Impressum (đã được tạo ở TASK-010).

Task này redesign lại toàn bộ trang chủ trong cùng một file HTML, giữ nguyên toàn bộ nội dung SEO hiện có, chỉ thay đổi cách trình bày và cấu trúc visual.

---

## 2. REQUIREMENTS

### Hero Section

Hiện tại hero section chỉ có logo TextHelfer to ở giữa và một câu subtitle. Cần thay đổi như sau:

- [ ] Bỏ logo TextHelfer lớn ở giữa trang (logo đã có trong header, không cần lặp lại)
- [ ] H1 gọn hơn, khoảng 6 đến 8 từ, ví dụ: "Textwerkzeuge für die deutsche Sprache"
- [ ] Subtitle ngắn 1 câu, tối đa 15 từ, tập trung vào lợi ích người dùng thay vì liệt kê spec
- [ ] Thêm 4 trust badge nằm ngang ngay dưới subtitle, mỗi badge gồm icon nhỏ và text ngắn:
  - "Kostenlos" (icon: ✓)
  - "Keine Anmeldung" (icon: ✓)
  - "Kein Server" (icon: ✓)
  - "Speziell für Deutsch" (icon: ✓)
- [ ] Badge style: pill shape, background mờ, border nhẹ, font nhỏ khoảng 13px
- [ ] Khoảng cách từ header đến H1 thoải mái, không bị chật

### Card Grid — Passwort Generator nổi bật hơn

- [ ] Card Passwort Generator chiếm toàn bộ hàng đầu tiên (width 100%), cao hơn các card khác
- [ ] Bên trong card hero: icon to hơn bên trái, tên tool và mô tả bên phải, kèm nút "Jetzt ausprobieren" màu accent
- [ ] Badge "BELIEBT" giữ nguyên, đặt góc trên phải card
- [ ] 8 card còn lại vẫn dạng grid 3 cột như hiện tại (hoặc 4 cột nếu trông cân hơn)
- [ ] Tất cả card có hover state nhẹ: nổi lên một chút (translateY -2px), shadow tăng nhẹ
- [ ] Icon trong tất cả card phải nhất quán về style: dùng emoji hoặc dùng SVG icon, không trộn lẫn
- [ ] Mô tả trong card đủ 2 dòng, không bị cụt hoặc overflow

### Section "So funktioniert TextHelfer"

- [ ] Thu nhỏ lại: số 1, 2, 3 giảm từ 48px xuống còn khoảng 32px
- [ ] Chuyển xuống dưới phần card grid, trước section "Was ist TextHelfer?"
- [ ] Thêm background nhẹ phân tách với phần trên (ví dụ: background hơi khác màu hoặc border-top)

### Section "Für wen ist TextHelfer?"

Hiện đang là đoạn văn với các từ bold inline, khó scan. Cần đổi thành:

- [ ] 5 card nhỏ dạng grid 2 cột hoặc 3 cột, mỗi card gồm:
  - Icon emoji hoặc SVG đại diện cho nhóm người dùng
  - Tên nhóm (bold, ví dụ: "Texter & Content Creator")
  - 1 câu mô tả ngắn dưới 15 từ
- [ ] 5 nhóm người dùng giữ nguyên như nội dung hiện có: Texter, Studierende, Marketing-Manager, Webentwickler, Übersetzer

### Footer

- [ ] Thêm link "Impressum" trỏ đến /impressum/
- [ ] Thứ tự footer links: tất cả 9 tool, rồi Impressum, Datenschutz, Kontakt
- [ ] Bỏ "Über uns" và "Nutzungsbedingungen" nếu đang có
- [ ] Giữ "© 2026 TextHelfer"

### Dark Mode Toggle

- [ ] Đổi từ hình tròn vàng/cam hiện tại sang button có border-radius 8px
- [ ] Background button: rgba mờ, border 1px solid màu border của theme
- [ ] Bên trong: icon ☀️ hoặc 🌙 kích thước 18px, padding đủ để button tối thiểu 44x44px
- [ ] Transition nhẹ khi hover

### Những phần KHÔNG được thay đổi

- [ ] Toàn bộ nội dung text SEO: "Was ist TextHelfer?", "Warum TextHelfer?", tất cả internal links
- [ ] FAQ accordion và nội dung FAQ
- [ ] Navigation header (chỉ sửa dark mode toggle)
- [ ] Schema JSON-LD trong head
- [ ] Tất cả anchor text và href của internal links

---

## 3. TECH NOTES

- `<html lang="de">` bắt buộc
- Font: `system-ui`, không dùng Google Fonts
- Tất cả CSS và JS inline trong 1 file HTML duy nhất
- Không dùng framework hoặc external JS library
- Dark mode script đọc localStorage đặt trong `<head>`, không ở cuối body
- localStorage key: "texthelfer-theme"
- Màu sắc Light: bg #FFFFFF, text #111827, accent #2563EB, border #E2E8F0
- Màu sắc Dark: bg #0F172A, text #F1F5F9, accent #60A5FA, border #334155, card bg #1E293B
- Navigation: giữ nguyên link 9 tool, không thêm không bớt
- 3 div ad placeholder giữ nguyên vị trí: id="ad-top", id="ad-middle", id="ad-bottom"

### SEO bắt buộc giữ nguyên:
- Title tag hiện tại không được thay đổi
- Meta description hiện tại không được thay đổi
- H1 duy nhất, chứa keyword chính
- Canonical URL không thay đổi
- Tất cả Schema JSON-LD không thay đổi

---

## 4. ACCEPTANCE CRITERIA

### Visual và UX:
- [ ] Hero section không còn logo TextHelfer lớn ở giữa trang
- [ ] 4 trust badge hiển thị đúng ngay dưới subtitle trên cả desktop và mobile
- [ ] Card Passwort Generator chiếm full width hàng đầu, có nút CTA
- [ ] 8 card còn lại hiển thị đúng grid, không bị vỡ layout
- [ ] Hover state hoạt động trên tất cả card
- [ ] Section "Für wen" hiển thị dạng card grid thay vì đoạn văn
- [ ] Dark mode toggle trông nhất quán với design tổng thể
- [ ] Footer có link Impressum, không còn "Über uns"

### Nội dung SEO không bị mất:
- [ ] Toàn bộ text trong "Was ist TextHelfer?" còn nguyên
- [ ] Toàn bộ text trong "Warum TextHelfer?" còn nguyên
- [ ] Tất cả internal links (href và anchor text) còn nguyên
- [ ] FAQ accordion còn đủ tất cả câu hỏi và câu trả lời
- [ ] Schema JSON-LD còn nguyên trong source

### Technical:
- [ ] `<html lang="de">` có trong source
- [ ] Lighthouse Performance 90 trở lên (target 95)
- [ ] FCP dưới 1 giây
- [ ] Không có external request không cần thiết

### Responsive:
- [ ] Hiển thị đúng trên Chrome desktop 1920px
- [ ] Hiển thị đúng trên mobile 375px, không overflow, không cắt chữ
- [ ] Trust badge trên mobile: xuống 2 hàng (2 badge mỗi hàng) thay vì 1 hàng ngang
- [ ] Card Passwort Generator trên mobile: layout dọc, vẫn full width
- [ ] "Für wen" cards trên mobile: 1 cột

### Dark mode:
- [ ] Toggle hoạt động, không flash trắng khi reload
- [ ] Preference giữ nguyên sau khi đóng mở lại trình duyệt
- [ ] Trust badge, card hero, "Für wen" cards đều có màu đúng ở cả 2 mode

### Deploy:
- [ ] PR đã merge vào main
- [ ] GitHub Actions deploy thành công
- [ ] https://texthelfer.net/ truy cập được và hiển thị đúng
- [ ] HTTPS hoạt động

---

## 5. DELIVERABLES

- [ ] index.html đã được cập nhật đầy đủ
- [ ] Commit file brief này với status DONE

---

## 6. GHI CHÚ THÊM

Toàn bộ thay đổi nằm trong index.html, không ảnh hưởng đến bất kỳ file tool nào. Dev không cần đụng vào các subfolder tool.

Khi làm card hero Passwort Generator full width, cần đặc biệt chú ý: trên mobile thì card này vẫn cần trông tốt và nút CTA vẫn bấm được (tối thiểu 52px cao).

Section "So funktioniert TextHelfer" giữ nguyên nội dung 3 bước, chỉ thu nhỏ font số và dời vị trí xuống dưới card grid.

Nếu Dev thấy có phần nào trong layout mới xung đột với nội dung SEO hiện có thì ưu tiên giữ nội dung SEO, báo lại PM trước khi xử lý.

---

**PM Sign-off:** [ ] Chờ ký duyệt
**Ngày ký duyệt:** ___________
**Ghi chú ký duyệt:** ___________
