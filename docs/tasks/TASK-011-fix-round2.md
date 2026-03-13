# TASK-011 — Homepage Redesign (Fix Round 2)

**Ngày cập nhật:** 2026-03-13
**PM:** TextHelfer PM Agent
**Assignee:** Dev Agent (Claude CLI)
**Priority:** P1
**Status:** IN_REVIEW — cần sửa trước khi ký duyệt
**Branch:** feature/TASK-011-homepage-redesign (sửa trên branch cũ, không tạo branch mới)

---

## Bối cảnh

Round 1 của TASK-011 đã hoàn thành phần lớn yêu cầu. PM review giao diện mới và xác định 3 lỗi cần sửa, đồng thời Owner quyết định thay đổi thêm cấu trúc header và footer. Tất cả được gộp vào round fix này để Dev làm một lần, không cần tạo task mới.

---

## Danh sách việc cần làm

### FIX-1 — Dark mode toggle chưa được cập nhật

Góc phải header vẫn là hình tròn vàng cam như bản cũ. Cần đổi thành:

- Button có border-radius 8px
- Background: rgba(255,255,255,0.08) ở dark mode, rgba(0,0,0,0.06) ở light mode
- Border: 1px solid màu border của theme (light: #E2E8F0, dark: #334155)
- Bên trong: icon ☀️ khi đang dark mode, 🌙 khi đang light mode, font-size 18px
- Kích thước button tối thiểu 44x44px, có padding đủ để icon không bị sát cạnh
- Transition: opacity 0.2s khi hover

### FIX-2 — Card grid 8 tool: hàng cuối bị lệch trái

Hàng cuối chỉ có 2 card (Textvergleich và Zufallszahlen), ô thứ 3 bỏ trống trông mất cân bằng. Cách xử lý: dùng CSS để 2 card hàng cuối tự center trong hàng thay vì bám trái. Gợi ý dùng `justify-content: center` trên grid container hoặc đặt 2 card đó vào wrapper riêng với `margin: 0 auto`.

### FIX-3 — "Für wen" grid: hàng dưới lệch trái

Tương tự FIX-2. Hàng dưới của "Für wen" có 2 card (Webentwickler và Übersetzer) nằm trái, cần center lại. Xử lý giống FIX-2.

### FIX-4 — Header: thu 9 tool link vào dropdown "Werkzeuge"

Hiện tại header liệt kê 9 link thẳng hàng ngang. Cần đổi thành:

- Một mục duy nhất tên "Werkzeuge" có mũi tên nhỏ ▾ bên cạnh
- Khi hover vào "Werkzeuge" (desktop) hoặc click (mobile) thì hiện dropdown
- Dropdown gồm 9 tool, mỗi item có icon emoji nhỏ bên trái và tên tool đầy đủ bên phải
- Dropdown style: background màu card (#1E293B ở dark, #F8FAFC ở light), border-radius 8px, box-shadow nhẹ, padding 8px
- Mỗi item trong dropdown cao tối thiểu 44px để bấm được trên mobile
- Khi click vào item thì navigate đến đúng tool URL
- Dropdown tự đóng khi click ra ngoài hoặc khi đã navigate
- Trên mobile: "Werkzeuge" expand/collapse dạng accordion thay vì dropdown ngang

Thứ tự 9 tool trong dropdown (giữ nguyên như hiện tại):
1. 🔐 Passwort Generator → /passwort-generator/
2. 🔤 Zeichenzähler → /zeichenzaehler/
3. 📝 Wörter zählen → /woerter-zaehlen/
4. 📊 Lesbarkeit prüfen → /lesbarkeit/
5. 📄 Lorem Ipsum Deutsch → /lorem-ipsum-deutsch/
6. 🔠 Text Formatierung → /text-formatierung/
7. 🔄 Umlaut Konverter → /umlaut-konverter/
8. 🔍 Textvergleich → /textvergleich/
9. 🎲 Zufallszahlen Generator → /zufallszahlen/

Sau khi thu vào dropdown, header chỉ còn: Logo bên trái | "Werkzeuge" dropdown ở giữa/phải | Dark mode toggle góc phải cùng.

Quan trọng: tất cả 9 link vẫn phải có trong HTML để Googlebot crawl được, chỉ ẩn về mặt visual khi dropdown chưa mở.

### FIX-5 — Footer: đổi danh sách tool sang grid 3 cột

Hiện tại footer liệt kê 9 tool trên 1 hàng ngang phân cách bằng dấu |, trông dài và khó đọc. Cần đổi thành:

- Grid 3 cột, mỗi cột 3 tool, tổng 9 tool
- Font-size nhỏ hơn body text một chút, khoảng 14px
- Mỗi link có padding đủ để bấm được trên mobile
- Màu link: màu mờ hơn text chính (ví dụ #94A3B8 ở dark mode), có underline khi hover
- Trên mobile: đổi thành 2 cột hoặc 1 cột tùy độ rộng

Bố cục footer sau khi sửa:

```
[ Passwort Generator ]  [ Zeichenzähler    ]  [ Wörter zählen   ]
[ Lesbarkeit prüfen  ]  [ Lorem Ipsum      ]  [ Text Formatierung]
[ Umlaut Konverter   ]  [ Textvergleich    ]  [ Zufallszahlen    ]

────────────────────────────────────────────────────────────────
Impressum | Datenschutz | Kontakt

© 2026 TextHelfer
```

---

## Những phần KHÔNG được thay đổi

- Toàn bộ nội dung SEO: "Was ist TextHelfer?", "Warum TextHelfer?", tất cả internal links trong phần text
- FAQ accordion và nội dung tất cả câu hỏi
- Hero section: H1, subtitle, trust badge 4 pills
- Card Passwort Generator hero (full width, nút CTA, badge BELIEBT)
- 8 card tool còn lại (chỉ fix alignment ở FIX-2)
- Section "So funktioniert TextHelfer"
- Section "Für wen" (chỉ fix alignment ở FIX-3)
- Schema JSON-LD trong head
- Title tag, meta description, canonical URL

---

## Acceptance Criteria cho round fix này

Dev tự test và báo cáo theo format sau trước khi tạo PR:

```
TASK-011 Fix Round 2 — Test Report

[ ] FIX-1: Dark mode toggle đúng style (border-radius 8px, background mờ, icon đổi đúng)
[ ] FIX-1: Toggle hoạt động, không flash khi reload, preference giữ sau khi đóng browser
[ ] FIX-2: 2 card hàng cuối của grid 8 tool được center, không lệch trái
[ ] FIX-3: 2 card hàng cuối của "Für wen" được center, không lệch trái
[ ] FIX-4: Header chỉ còn 1 mục "Werkzeuge" thay vì 9 link
[ ] FIX-4: Dropdown hiện đúng 9 tool khi hover/click
[ ] FIX-4: Tất cả 9 link vẫn có trong HTML source (kiểm tra Ctrl+U)
[ ] FIX-4: Dropdown tự đóng khi click ra ngoài
[ ] FIX-4: Trên mobile 375px dropdown hoạt động đúng dạng accordion
[ ] FIX-5: Footer tool list hiển thị dạng grid 3 cột
[ ] FIX-5: Trên mobile footer xuống 2 cột hoặc 1 cột, không overflow
[ ] FIX-5: Impressum và Datenschutz vẫn có trong footer hàng phía dưới
[ ] Toàn bộ nội dung SEO và FAQ còn nguyên (kiểm tra page source)
[ ] Lighthouse Performance vẫn 90 trở lên
[ ] Không có broken link trong header dropdown và footer
```

---

## Ghi chú thêm

FIX-4 là thay đổi lớn nhất và ảnh hưởng đến tất cả các trang tool (vì navigation header dùng chung). Tuy nhiên vì task này chỉ sửa index.html, Dev chỉ cần cập nhật header trong index.html. Việc cập nhật header dropdown cho 9 trang tool còn lại sẽ được xử lý trong một task riêng sau khi round fix này được ký duyệt, để không block tiến độ.

---

**PM Sign-off:** [ ] Chờ ký duyệt
**Ngày ký duyệt:** ___________
**Ghi chú ký duyệt:** ___________
