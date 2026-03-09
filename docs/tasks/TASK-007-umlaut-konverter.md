# TASK-007 — Umlaut Konverter

**Ngày tạo:** 2026-03-09
**PM:** TextHelfer PM Agent
**Assignee:** Dev Agent (Claude CLI)
**Priority:** P1
**Status:** TODO
**Branch:** feature/TASK-007-umlaut-konverter
**Estimate:** 1 ngày

---

## 1. CONTEXT

Người dùng tiếng Đức thường xuyên gặp tình huống phải gõ văn bản trên bàn phím không có ký tự Đức (ä, ö, ü, ß) — ví dụ bàn phím quốc tế, thiết bị cũ, hoặc hệ thống kỹ thuật không nhận umlaut. Họ cần chuyển đổi giữa hai dạng: dạng umlaut thực (ä/ö/ü/ß) và dạng thay thế ASCII (ae/oe/ue/ss), và ngược lại. Hiện tại gần như không có tool tiếng Đức chuyên biệt cho việc này — KD cực thấp (3–10), xác suất rank top rất cao. Tool build trong 1 ngày, tái sử dụng layout từ các tool trước.

---

## 2. REQUIREMENTS

### Chức năng bắt buộc:
- [ ] Hai textarea đặt cạnh nhau (desktop) hoặc xếp dọc (mobile ≤768px): bên trái là Input, bên phải là Output
- [ ] Chuyển đổi tự động real-time khi người dùng gõ vào textarea Input (không cần bấm nút riêng để convert)
- [ ] Chiều chuyển đổi mặc định: **Umlaut → ASCII** (ä→ae, ö→oe, ü→ue, ß→ss)
- [ ] Hỗ trợ chữ hoa: Ä→Ae, Ö→Oe, Ü→Ue (không phải AE — viết hoa chỉ chữ đầu)
- [ ] Nút **„Richtung tauschen"**: đổi chiều chuyển đổi sang ASCII → Umlaut (ae→ä, oe→ö, ue→ü, ss→ß), label trên nút cập nhật theo chiều hiện tại
- [ ] Hiển thị nhãn rõ ràng trên mỗi textarea: chiều đang hoạt động (ví dụ: „Mit Umlauten" ↔ „Ohne Umlauten")
- [ ] Nút **„Kopieren"** riêng biệt cho mỗi textarea (Input và Output), toast xác nhận „Kopiert!" 2 giây
- [ ] Nút **„Löschen"** cho textarea Input, có confirm khi text dài hơn 50 ký tự
- [ ] Hiển thị số ký tự đã chuyển đổi (ví dụ: „4 Umlaute umgewandelt") bên dưới Output

### Chức năng không được phá vỡ:
- [ ] Thứ tự xử lý replaceAll(): chữ hoa trước (Ä, Ö, Ü), sau đó chữ thường (ä, ö, ü, ß) — tránh double-convert
- [ ] Chiều Umlaut→ASCII: Ä→Ae, Ö→Oe, Ü→Ue, ä→ae, ö→oe, ü→ue, ß→ss
- [ ] Chiều ASCII→Umlaut: ae→ä, oe→ö, ue→ü, ss→ß (lưu ý: cần xử lý cẩn thận, xem Ghi chú kỹ thuật mục 6)
- [ ] Dark mode không làm vỡ layout 2 cột
- [ ] SEO content và FAQ accordion không bị xóa

---

## 3. TECH NOTES

> Các ràng buộc kỹ thuật bất biến — Dev KHÔNG được bỏ qua:

- `<html lang="de">` trên thẻ html — bắt buộc
- Font: `system-ui` — KHÔNG dùng Google Fonts
- Toàn bộ CSS + JS inline trong 1 file HTML duy nhất
- KHÔNG dùng framework (React, Vue...) hoặc external JS library
- Dark mode: script đọc `localStorage.getItem("texthelfer-theme")` đặt trong `<head>`, KHÔNG ở cuối body
- Navigation header: link đến tất cả 9 tool + footer link Kontakt
- 3 Ad placeholder: `id="ad-top"` (sau nút hành động), `id="ad-middle"` (sau H2 đầu), `id="ad-bottom"` (trước FAQ)
- Màu sắc:
  - Light: bg `#FFFFFF`, text `#111827`, accent `#2563EB`, border `#E2E8F0`
  - Dark: bg `#0F172A`, text `#F1F5F9`, accent `#60A5FA`, border `#334155`
- localStorage key cho theme: `"texthelfer-theme"`

### SEO bắt buộc:
- **Title:** `Umlaut Umschreiben Online Kostenlos – Umlaut Konverter | TextHelfer` (55 ký tự)
- **Meta description:** `Umlaute umschreiben leicht gemacht: ä zu ae, ö zu oe, ü zu ue und ß zu ss – und umgekehrt. Kostenloser Umlaut Konverter, keine Anmeldung erforderlich.` (151 ký tự)
- **Canonical:** `<link rel="canonical" href="https://texthelfer.net/umlaut-konverter/">`
- **H1:** `Umlaut Konverter – Umlaute umschreiben online`
- WebApplication JSON-LD schema (price: "0", priceCurrency: "EUR")
- FAQPage JSON-LD schema (tối thiểu 6 Q&A tiếng Đức — xem mục 6)
- Nội dung SEO: H2 „Wie funktioniert der Umlaut Konverter?" (~150 từ) + H2 „Für wen ist der Umlaut Konverter geeignet?" (4–5 bullets) + H2 „Häufige Fragen" (6–8 FAQ accordion)

---

## 4. ACCEPTANCE CRITERIA

> PM sẽ dùng danh sách này ở Bước 6 để ký duyệt.
> Dev dùng danh sách này ở Bước 5 để tự test và báo cáo.

### Functional:
- [ ] Gõ „Schöne Grüße aus München" vào Input → Output hiển thị „Schoene Gruesse aus Muenchen" (chiều Umlaut→ASCII)
- [ ] Gõ „Schoene Gruesse aus Muenchen" vào Input sau khi đổi chiều → Output hiển thị „Schöne Grüße aus München"
- [ ] Chữ hoa Ä→Ae (không phải AE): gõ „Ärger" → Output „Aerger"
- [ ] Nút „Richtung tauschen" đổi chiều, label textarea cập nhật, nội dung được re-convert ngay lập tức
- [ ] Nút „Kopieren" mỗi bên hoạt động, toast „Kopiert!" hiện 2 giây rồi tự mất
- [ ] Nút „Löschen" xóa Input, không cần confirm nếu ≤50 ký tự; có confirm nếu >50 ký tự
- [ ] Counter hiển thị đúng số lượng ký tự đã được chuyển đổi

### SEO & Technical:
- [ ] `<html lang="de">` có trong source
- [ ] Title tag: `Umlaut Umschreiben Online Kostenlos – Umlaut Konverter | TextHelfer`, 50–60 ký tự
- [ ] 1 thẻ H1 duy nhất: `Umlaut Konverter – Umlaute umschreiben online`
- [ ] Canonical URL: `https://texthelfer.net/umlaut-konverter/`
- [ ] WebApplication JSON-LD hợp lệ (validate tại schema.org/validator)
- [ ] FAQPage JSON-LD hợp lệ, tối thiểu 6 Q&A tiếng Đức
- [ ] Lighthouse Performance ≥ 90 (target 95+)
- [ ] FCP (First Contentful Paint) dưới 1 giây

### UX:
- [ ] Layout 2 cột trên desktop (≥768px), 1 cột trên mobile (<768px)
- [ ] Hiển thị đúng trên Chrome desktop (1920px) — không vỡ layout
- [ ] Hiển thị đúng trên mobile 375px — không overflow, không cắt chữ
- [ ] Tất cả nút bấm được trên mobile (≥52px chiều cao)
- [ ] Dark mode toggle hoạt động, KHÔNG flash trắng khi reload trang
- [ ] Preference dark mode giữ nguyên sau khi đóng/mở lại trình duyệt

### Integration:
- [ ] Navigation bar hiển thị link đến cả 9 tool
- [ ] Footer có link Kontakt
- [ ] Ít nhất 3 internal link ngữ cảnh trong nội dung SEO (ví dụ: link đến Zeichenzähler, Text Formatierung, Wörter zählen)
- [ ] 3 div ad placeholder: `id="ad-top"`, `id="ad-middle"`, `id="ad-bottom"`

### Deploy:
- [ ] PR đã merge vào `main`
- [ ] GitHub Actions deploy thành công (không có lỗi trong workflow log)
- [ ] URL live truy cập được: `https://texthelfer.net/umlaut-konverter/`
- [ ] HTTPS hoạt động, không có mixed content warning

---

## 5. DELIVERABLES

Dev cần commit lên Git khi task hoàn thành:

- [ ] `umlaut-konverter/index.html` — file tool hoàn chỉnh
- [ ] Cập nhật `index.html` (trang chủ) — thêm card Umlaut Konverter vào grid
- [ ] Cập nhật navigation trong TẤT CẢ các tool đã live (thêm link `/umlaut-konverter/`)
- [ ] Cập nhật `sitemap.xml` — thêm URL `https://texthelfer.net/umlaut-konverter/`
- [ ] Commit file brief này với status: `DONE`

---

## 6. GHI CHÚ THÊM

### Thuật toán chuyển đổi — thứ tự QUAN TRỌNG:

**Chiều Umlaut → ASCII** (thứ tự bắt buộc, chữ hoa trước):
```
Ä → Ae
Ö → Oe
Ü → Ue
ä → ae
ö → oe
ü → ue
ß → ss
```

**Chiều ASCII → Umlaut** (cần cẩn thận với từ không phải umlaut):
```
ae → ä  (ví dụ: "Strasse" → "Straße" nhưng "Israel" → giữ nguyên)
oe → ö
ue → ü
ss → ß
```
> ⚠️ Lưu ý: Chiều ASCII→Umlaut có thể tạo ra kết quả sai cho một số từ tiếng Đức gốc không chứa umlaut (ví dụ: "Israel", "Mosel", "Koeln" nếu xử lý sai). Không cần xử lý edge case này trong v1 — thêm ghi chú nhỏ trong UI: *„Bitte Ergebnis prüfen – nicht alle ae/oe/ue sind Umlaute."*

### Nội dung FAQ tiếng Đức (6 câu, đưa vào cả accordion HTML lẫn JSON-LD):

1. **Wie schreibt man ä ohne Umlaut?** → Das ä wird als „ae" geschrieben, das ö als „oe" und das ü als „ue". Das ß wird durch „ss" ersetzt. Diese Schreibweise wird Umlautumschreibung genannt und ist besonders beim Tippen auf internationalen Tastaturen nützlich.

2. **Wann brauche ich die Umlautumschreibung?** → Die Umlautumschreibung wird benötigt, wenn Systeme oder Tastaturen keine deutschen Sonderzeichen unterstützen – zum Beispiel bei E-Mail-Adressen, Benutzernamen, alten Computersystemen oder beim Schreiben auf einer englischen Tastatur.

3. **Wie buchstabiert man ü auf Englisch?** → Das ü wird im Deutschen als „ue" umgeschrieben. Das Ü (Großbuchstabe) wird zu „Ue". Im internationalen Kontext ist diese Schreibweise allgemein anerkannt.

4. **Ist die Umschreibung von ß zu ss korrekt?** → Ja, ss ist die offizielle Umschreibung von ß, wenn das Zeichen nicht verfügbar ist. In der Schweiz wird ß grundsätzlich als ss geschrieben – das ist seit Jahrzehnten gängige Praxis.

5. **Kann ich den Umlaut Konverter für E-Mail-Adressen verwenden?** → Ja. Viele E-Mail-Server akzeptieren zwar heute Umlaute, aber für maximale Kompatibilität empfiehlt sich die ASCII-Umschreibung. Der Konverter hilft, Text schnell in die kompatible Schreibweise umzuwandeln.

6. **Funktioniert der Konverter auch in umgekehrter Richtung?** → Ja. Mit dem Knopf „Richtung tauschen" können Sie ASCII-Text (ae, oe, ue, ss) zurück in die Umlaut-Schreibweise (ä, ö, ü, ß) umwandeln. Bitte prüfen Sie das Ergebnis, da nicht jedes „ae" im Deutschen ein Umlaut ist.

### Internal links gợi ý trong nội dung SEO:
- Link đến **Zeichenzähler** (`/zeichenzaehler/`) khi đề cập đến độ dài văn bản
- Link đến **Text Formatierung** (`/text-formatierung/`) khi đề cập đến xử lý văn bản tiếng Đức
- Link đến **Wörter zählen** (`/woerter-zaehlen/`) khi đề cập đến phân tích văn bản

### Layout 2 cột:
```
┌────────────────────────────────────────┐
│  Header + Nav                          │
├───────────────────┬────────────────────┤
│  H1 + mô tả       │                    │
├───────────────────┴────────────────────┤
│  [Label: Mit Umlauten ↓]  [Label: Ohne Umlauten ↓]  │
│  [Textarea Input      ]  [Textarea Output (readonly)]│
│  [Kopieren] [Löschen ]  [Kopieren]                  │
│  „4 Umlaute umgewandelt"                             │
│  [← Richtung tauschen →]                             │
├────────────────────────────────────────┤
│  id="ad-top"                           │
├────────────────────────────────────────┤
│  H2: Wie funktioniert...               │
│  id="ad-middle"                        │
│  H2: Für wen...                        │
│  id="ad-bottom"                        │
│  H2: Häufige Fragen (FAQ accordion)    │
├────────────────────────────────────────┤
│  Footer                                │
└────────────────────────────────────────┘
```

---

**PM Sign-off:** [ ] Chờ ký duyệt
**Ngày ký duyệt:** ___________
**Ghi chú ký duyệt:** ___________
