# TASK-012 — Cập nhật header dropdown cho 9 trang tool

**Ngày tạo:** 2026-03-13
**PM:** TextHelfer PM Agent
**Assignee:** Dev Agent (Claude CLI)
**Priority:** P1
**Status:** TODO
**Branch:** feature/TASK-012-header-dropdown-tools
**Estimate:** 2 đến 3 giờ

---

## 1. CONTEXT

Homepage (index.html) đã được cập nhật header sang dropdown "Werkzeuge ▾" ở TASK-011. Tuy nhiên 9 trang tool hiện vẫn dùng nav cũ với 9 link thẳng hàng ngang. Người dùng khi đang dùng một tool và muốn chuyển sang tool khác sẽ thấy header khác biệt so với homepage, trông thiếu nhất quán. Task này đồng bộ header của tất cả 9 trang tool với homepage.

---

## 2. REQUIREMENTS

### Yêu cầu chính

Dev lấy đúng đoạn code header đã làm trong index.html (bao gồm dropdown Werkzeuge, dark mode toggle đã được fix ở TASK-011) và áp dụng vào tất cả 9 file sau:

- [ ] passwort-generator/index.html
- [ ] zeichenzaehler/index.html
- [ ] woerter-zaehlen/index.html
- [ ] lesbarkeit/index.html
- [ ] lorem-ipsum-deutsch/index.html
- [ ] text-formatierung/index.html
- [ ] umlaut-konverter/index.html
- [ ] textvergleich/index.html
- [ ] zufallszahlen/index.html

### Chi tiết header cần đồng bộ

Header sau khi sửa phải giống hệt homepage, gồm:

- Logo TextHelfer bên trái (Text màu #2563EB, Helfer màu #1E3A5F), link về /
- Mục "Werkzeuge ▾" với dropdown 9 tool có icon, hover trên desktop, click trên mobile
- Dark mode toggle đúng style: button border-radius 8px, background rgba mờ, icon ☀️/🌙

Dropdown 9 tool (giữ nguyên thứ tự và URL):
1. 🔐 Passwort Generator → /passwort-generator/
2. 🔤 Zeichenzähler → /zeichenzaehler/
3. 📝 Wörter zählen → /woerter-zaehlen/
4. 📊 Lesbarkeit prüfen → /lesbarkeit/
5. 📄 Lorem Ipsum Deutsch → /lorem-ipsum-deutsch/
6. 🔠 Text Formatierung → /text-formatierung/
7. 🔄 Umlaut Konverter → /umlaut-konverter/
8. 🔍 Textvergleich → /textvergleich/
9. 🎲 Zufallszahlen Generator → /zufallszahlen/

### Active state

Tool nào đang được mở thì link tương ứng trong dropdown hiển thị active state: màu accent (#2563EB light, #60A5FA dark), font hơi đậm hơn. Ví dụ khi đang ở trang /zeichenzaehler/ thì item "Zeichenzähler" trong dropdown có màu accent.

### Những phần KHÔNG được thay đổi trong mỗi file tool

- Toàn bộ phần `<main>`: tool UI, nội dung SEO, FAQ, ad placeholder
- Title tag, meta description, canonical URL, schema JSON-LD
- Footer của từng trang
- Dark mode script trong `<head>`

---

## 3. TECH NOTES

- Copy nguyên đoạn header HTML và CSS liên quan từ index.html, không viết lại từ đầu
- CSS của header nên được đặt chung với CSS hiện có trong `<style>` của từng file, không tạo file CSS riêng
- JS của dropdown (open/close, click outside) cũng copy từ index.html
- Dark mode script đọc localStorage vẫn phải nằm trong `<head>`, không dời xuống body
- localStorage key: "texthelfer-theme"
- Không dùng external library

---

## 4. ACCEPTANCE CRITERIA

```
TASK-012 — Test Report

[ ] Tất cả 9 trang tool có header dropdown "Werkzeuge ▾"
[ ] Dropdown hiện đúng 9 tool khi hover/click trên mỗi trang
[ ] Active state đúng: tool đang mở được highlight trong dropdown
[ ] Dark mode toggle đúng style trên tất cả 9 trang
[ ] Toggle hoạt động, preference giữ sau reload trên tất cả 9 trang
[ ] Trên mobile 375px: hamburger/accordion hoạt động đúng trên tất cả 9 trang
[ ] Dropdown tự đóng khi click ra ngoài trên tất cả 9 trang
[ ] Logo link về / hoạt động đúng trên tất cả 9 trang
[ ] Toàn bộ tool UI và nội dung SEO của từng trang không bị thay đổi
[ ] Không có console error trên bất kỳ trang nào
[ ] Lighthouse Performance vẫn 90 trở lên (test ít nhất 2 trang bất kỳ)
```

---

## 5. DELIVERABLES

- [ ] 9 file tool đã được cập nhật header
- [ ] Commit file brief này với status DONE

---

## 6. GHI CHÚ THÊM

Task này về bản chất là copy-paste có điều chỉnh, không phải viết logic mới. Phần tốn thời gian nhất là đảm bảo CSS của header không xung đột với CSS tool hiện có trong từng file. Nếu có xung đột Dev cần scope CSS header bằng class cụ thể thay vì dùng selector chung.

Active state là điểm duy nhất khác nhau giữa các trang. Dev có thể xử lý bằng cách thêm một dòng JS nhỏ ở cuối mỗi file: so sánh `window.location.pathname` với href của từng item trong dropdown rồi thêm class active.

---

**PM Sign-off:** [ ] Chờ ký duyệt
**Ngày ký duyệt:** ___________
**Ghi chú ký duyệt:** ___________
