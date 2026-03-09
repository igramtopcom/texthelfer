# TASK-010 — Global UX & Legal Improvements

**Ngày tạo:** 2026-03-09
**PM:** TextHelfer PM Agent
**Assignee:** Dev Agent (Claude CLI)
**Priority:** P0 — chặn AdSense approval nếu thiếu Legal pages
**Status:** TODO
**Branch:** feature/TASK-010-global-ux-legal
**Estimate:** 2–3 ngày

---

## 1. CONTEXT

Sau khi 9 tool đã live, có 4 vấn đề cần xử lý toàn cục trước khi apply Google AdSense:

1. **Dark mode mặc định** — thị trường DACH có tỷ lệ dùng dark mode cao, đặc biệt trên desktop. Hiện tại mặc định là light, cần đổi thành dark.
2. **Mobile navigation** — người dùng mobile muốn chuyển giữa các tool phải vào trang chủ rồi mới chọn, gây friction không cần thiết. Cần hamburger menu.
3. **Logo và favicon** — website hiện không có icon nhận diện thương hiệu, trông thiếu chuyên nghiệp và khó nhận ra trên browser tab.
4. **Legal pages** — Google AdSense **bắt buộc** phải có Privacy Policy và Terms of Service trước khi duyệt tài khoản. Thiếu các trang này = bị reject AdSense.

Task này ảnh hưởng đến **toàn bộ 10 file** (9 tool + homepage) nên cần thực hiện cẩn thận, tránh regression.

---

## 2. REQUIREMENTS

### 2.1 Dark Mode mặc định

- [ ] Thay đổi logic dark mode: nếu người dùng **chưa từng** set preference (`localStorage.getItem("texthelfer-theme")` = null) → áp dụng **dark mode mặc định**
- [ ] Nếu người dùng đã set preference trước đó → tôn trọng preference đó (không override)
- [ ] Logic mới trong `<head>`:
```javascript
const theme = localStorage.getItem("texthelfer-theme");
if (theme === "dark" || theme === null) {
    document.documentElement.setAttribute("data-theme", "dark");
}
```
- [ ] Áp dụng thay đổi này vào **tất cả 10 file**: 9 tool + `index.html`
- [ ] Toggle button vẫn hoạt động bình thường — bật/tắt và lưu preference

### 2.2 Hamburger Menu cho Mobile

- [ ] Trên màn hình **< 768px**: ẩn navigation links ngang, hiển thị nút hamburger ☰ góc phải header (44×44px, cạnh dark mode toggle)
- [ ] Bấm hamburger → mở **dropdown menu dọc** hiển thị đủ 9 tool links + link Kontakt
- [ ] Bấm lại hamburger hoặc bấm ra ngoài menu → đóng menu
- [ ] Menu dropdown: nền `#1E293B` (dark) / `#F8FAFC` (light), có border, border-radius 8px, z-index cao để không bị che bởi nội dung
- [ ] Link đang active (trang hiện tại) được highlight màu accent trong dropdown
- [ ] Áp dụng vào **tất cả 10 file**: 9 tool + `index.html`
- [ ] Trên desktop (≥768px): navigation ngang như cũ, không hiển thị hamburger

### 2.3 Logo và Favicon

**Thiết kế logo đề xuất:**
- Hình dạng: chữ **„T"** cách điệu trong hình vuông bo góc (border-radius 8px)
- Màu nền: `#2563EB` (accent xanh của TextHelfer)
- Chữ T: màu trắng `#FFFFFF`, font bold, serif hoặc slab-serif
- Kích thước render: 32×32px (favicon), 40×40px (header logo)
- Implement bằng **SVG inline** — không dùng file ảnh ngoài, không external request

**Favicon:**
- [ ] Tạo `favicon.svg` tại root — SVG square với logo T như trên
- [ ] Thêm vào `<head>` tất cả file: `<link rel="icon" type="image/svg+xml" href="/favicon.svg">`
- [ ] Thêm fallback PNG: `<link rel="icon" type="image/png" href="/favicon.png">` (Dev tạo favicon.png 32×32 từ SVG)

**Header logo:**
- [ ] Thay text „TextHelfer" thuần túy hiện tại bằng: **[SVG icon T] + text „TextHelfer"** đặt cạnh nhau
- [ ] SVG icon 32×32px, vertical-align middle, margin-right 8px
- [ ] Text „Text" màu `#2563EB`, „Helfer" màu `#1E3A5F` (light) / „Helfer" màu `#93C5FD` (dark)
- [ ] Áp dụng vào **tất cả 10 file**

### 2.4 Legal Pages

Tạo 3 trang mới. Nội dung **bằng tiếng Đức**, phong cách trang trọng nhưng dễ hiểu. Email liên hệ: `texthelfer.net@gmail.com`

#### 2.4.1 `about.html` — Über uns
- [ ] H1: „Über TextHelfer"
- [ ] Nội dung (~300 từ tiếng Đức): giới thiệu TextHelfer là bộ công cụ văn bản tiếng Đức miễn phí, chạy 100% trong trình duyệt, không lưu dữ liệu người dùng, dành cho thị trường DACH. Nhấn mạnh giá trị: kostenlos, datenschutzfreundlich, keine Anmeldung.
- [ ] Liệt kê 9 tool với mô tả ngắn 1 câu mỗi tool
- [ ] Contact: „Bei Fragen oder Feedback: texthelfer.net@gmail.com"

#### 2.4.2 `datenschutz.html` — Datenschutzerklärung
- [ ] H1: „Datenschutzerklärung"
- [ ] **Bắt buộc có các mục sau** (yêu cầu DSGVO / GDPR tại Đức):
  - Verantwortlicher (tên tổ chức: TextHelfer, email: texthelfer.net@gmail.com)
  - Welche Daten wir erheben (câu trả lời: keine personenbezogenen Daten — toàn bộ xử lý trong trình duyệt)
  - Cookies (chỉ dùng localStorage cho theme preference — không phải tracking cookie)
  - Google AdSense (thông báo sẽ hiển thị quảng cáo từ Google, Google có thể dùng cookie)
  - Google Analytics / Search Console (nếu có — chỉ dùng Search Console, không dùng GA)
  - Ihre Rechte (quyền của người dùng theo DSGVO: Auskunft, Löschung, Widerspruch)
  - Kontakt: texthelfer.net@gmail.com
- [ ] Ngày cập nhật: März 2026

#### 2.4.3 `nutzungsbedingungen.html` — Nutzungsbedingungen
- [ ] H1: „Nutzungsbedingungen"
- [ ] Các mục cần có:
  - Nutzung der Tools (miễn phí, phi thương mại và thương mại đều được phép)
  - Keine Garantie (tool cung cấp „as is", không đảm bảo kết quả 100% chính xác)
  - Haftungsausschluss (TextHelfer không chịu trách nhiệm cho thiệt hại phát sinh từ việc dùng tool)
  - Urheberrecht (nội dung website thuộc TextHelfer, tool code không được copy để tạo sản phẩm cạnh tranh)
  - Änderungen (TextHelfer có quyền thay đổi điều khoản bất cứ lúc nào)
  - Kontakt: texthelfer.net@gmail.com
- [ ] Ngày cập nhật: März 2026

**Yêu cầu chung cho 3 trang legal:**
- [ ] Dùng cùng header/navigation/footer như 9 tool (hamburger menu, dark mode, logo)
- [ ] Layout đơn giản: max-width 760px, padding đủ rộng, font 18px, line-height 1.7
- [ ] Không cần ad placeholders trên legal pages
- [ ] Thêm link vào footer của **tất cả 10 file** hiện có: „Über uns | Datenschutz | Nutzungsbedingungen | Kontakt | © 2026 TextHelfer"

---

## 3. TECH NOTES

> Các ràng buộc kỹ thuật bất biến:

- `<html lang="de">` — bắt buộc trên tất cả file kể cả legal pages
- Font: `system-ui` — KHÔNG dùng Google Fonts
- KHÔNG dùng framework hoặc external JS library
- SVG logo phải inline hoặc là file SVG — không dùng PNG/JPG cho logo header
- localStorage key cho theme: `"texthelfer-theme"`
- Dark mode script đặt trong `<head>`, KHÔNG cuối body
- Hamburger menu phải đóng khi bấm ra ngoài (event listener `document.addEventListener('click', ...)`)

### Thứ tự thực hiện được đề xuất:
1. Tạo SVG logo/favicon trước → test render
2. Cập nhật 1 file tool làm mẫu (zeichenzaehler) với đủ 4 thay đổi → Owner review
3. Sau khi Owner approve mẫu → áp dụng cho 9 file còn lại
4. Tạo 3 legal pages
5. Cập nhật footer tất cả file

---

## 4. ACCEPTANCE CRITERIA

### 4.1 Dark Mode mặc định:
```
[ ] Mở tool lần đầu (xóa localStorage) → giao diện hiển thị dark mode ngay
    Kết quả thực tế: ___________

[ ] Bật light mode → reload → vẫn giữ light mode (preference được tôn trọng)
    Kết quả thực tế: ___________

[ ] Bật dark mode → reload → vẫn giữ dark mode
    Kết quả thực tế: ___________

[ ] Áp dụng đúng trên tất cả 10 file (9 tool + homepage)
    Kết quả thực tế: ___________
```

### 4.2 Hamburger Menu:
```
[ ] Mobile 375px: hamburger ☰ hiển thị góc phải header, nav links ẩn
    Kết quả thực tế: ___________

[ ] Bấm hamburger → dropdown 9 tool + Kontakt mở ra
    Kết quả thực tế: ___________

[ ] Bấm link trong dropdown → chuyển trang đúng, menu đóng lại
    Kết quả thực tế: ___________

[ ] Bấm ra ngoài dropdown → menu đóng
    Kết quả thực tế: ___________

[ ] Desktop ≥768px: nav ngang bình thường, không có hamburger
    Kết quả thực tế: ___________

[ ] Áp dụng đúng trên tất cả 10 file
    Kết quả thực tế: ___________
```

### 4.3 Logo và Favicon:
```
[ ] Favicon hiển thị trên browser tab (không phải icon trắng mặc định)
    Kết quả thực tế: ___________

[ ] Logo header: SVG icon T + text "TextHelfer" hiển thị đúng
    Kết quả thực tế: ___________

[ ] Logo hiển thị đúng ở cả light mode và dark mode
    Kết quả thực tế: ___________

[ ] Logo là link về trang chủ /
    Kết quả thực tế: ___________

[ ] Không có external request nào cho logo/favicon (kiểm tra Network tab)
    Kết quả thực tế: ___________
```

### 4.4 Legal Pages:
```
[ ] about.html live tại https://texthelfer.net/about.html
    Kết quả thực tế: ___________

[ ] datenschutz.html live tại https://texthelfer.net/datenschutz.html
    Kết quả thực tế: ___________

[ ] nutzungsbedingungen.html live tại https://texthelfer.net/nutzungsbedingungen.html
    Kết quả thực tế: ___________

[ ] Datenschutz có đề cập đến Google AdSense cookies
    Kết quả thực tế: ___________

[ ] Footer tất cả 10 file có đủ links: Über uns | Datenschutz | Nutzungsbedingungen | Kontakt
    Kết quả thực tế: ___________

[ ] 3 trang legal dùng cùng header/nav/footer như tool pages
    Kết quả thực tế: ___________
```

### Regression — không được phá vỡ:
```
[ ] Tất cả 9 tool vẫn hoạt động đúng chức năng sau khi cập nhật global
[ ] Dark mode toggle vẫn hoạt động và lưu preference
[ ] Nav links trên desktop vẫn đúng
[ ] Lighthouse Performance vẫn ≥ 90 trên ít nhất 2 tool (Dev chọn tool để test)
```

### Deploy:
- [ ] Tất cả file đã push lên `main`
- [ ] GitHub Actions deploy thành công
- [ ] `sitemap.xml` cập nhật thêm 3 URL legal pages
- [ ] 0 console errors trên tất cả trang

---

## 5. DELIVERABLES

Dev cần commit lên Git khi task hoàn thành:

- [ ] `favicon.svg` — tại root
- [ ] `favicon.png` — tại root (32×32px fallback)
- [ ] `about.html`
- [ ] `datenschutz.html`
- [ ] `nutzungsbedingungen.html`
- [ ] Cập nhật `index.html` + 9 tool files (dark mode default + hamburger + logo + footer)
- [ ] Cập nhật `sitemap.xml`
- [ ] Commit file brief này với status: `DONE`

---

## 6. GHI CHÚ THÊM

**Tại sao dark mode mặc định?**
Nghiên cứu thị trường DACH cho thấy >60% người dùng desktop ở Đức đã bật dark mode ở cấp độ OS. Việc flash sang light mode khi load trang lần đầu tạo trải nghiệm tiêu cực. Đặt dark làm mặc định giúp đa số người dùng có trải nghiệm tốt ngay từ đầu.

**Tại sao Legal pages quan trọng với AdSense?**
Google AdSense review team kiểm tra 3 điều trước khi approve: (1) đủ nội dung gốc, (2) có Privacy Policy rõ ràng đề cập đến AdSense cookies, (3) website không vi phạm content policy. Thiếu `datenschutz.html` là lý do reject phổ biến nhất với site mới tại thị trường EU/DACH.

**Về SVG logo:**
Dev có thể implement SVG inline trực tiếp trong HTML thay vì file riêng để giảm HTTP request. Ví dụ tham khảo:
```html
<a href="/" class="logo">
  <svg width="32" height="32" viewBox="0 0 32 32" fill="none">
    <rect width="32" height="32" rx="8" fill="#2563EB"/>
    <text x="16" y="23" text-anchor="middle" fill="white"
          font-size="20" font-weight="700" font-family="system-ui">T</text>
  </svg>
  <span><span class="logo-text">Text</span><span class="logo-helfer">Helfer</span></span>
</a>
```

---

> **Bước tiếp theo:**
> Owner paste prompt giao việc vào Claude CLI (Dev Agent):
> ```
> Bạn là Dev Agent của dự án TextHelfer.
> Đọc task brief tại: docs/tasks/TASK-010-global-ux-legal.md
> Sau khi đọc xong:
> 1. Tóm tắt lại những gì bạn hiểu về task này trong 3–5 câu
> 2. Liệt kê những gì bạn sẽ làm (không code ngay)
> 3. Hỏi nếu có gì chưa rõ
> Chờ xác nhận từ Owner trước khi bắt đầu code.
> ```

---

**PM Sign-off:** [ ] Chờ ký duyệt
**Ngày ký duyệt:** ___________
**Ghi chú ký duyệt:** ___________
