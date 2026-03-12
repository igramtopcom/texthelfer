# TASK-014 — Content Depth & UX Improvements

**Ngày tạo:** 2026-03-12
**PM:** TextHelfer PM Agent
**Assignee:** Dev Agent (Claude CLI)
**Priority:** P0
**Status:** TODO
**Branch:** feature/TASK-014-content-depth-ux
**Estimate:** 3–4 ngày

---

## 1. CONTEXT

Khảo sát đối thủ cho thấy TextHelfer đang có lợi thế về thiết kế và tính năng, nhưng thua ở 2 điểm: (1) không có lý do để người dùng ở lại lâu hơn sau khi dùng xong tool, và (2) homepage chưa đủ tín hiệu để Google hiểu đây là resource site thực sự.

Task này bổ sung 4 nhóm cải tiến, tất cả tập trung vào việc làm cho người dùng thật sự hiểu và dùng được tốt hơn — không phải nhồi thêm từ khóa.

---

## 2. REQUIREMENTS — 4 NHÓM CẢI TIẾN

---

### 2.1 — "Verwandte Tools" cuối mỗi trang tool (tất cả 9 tool)

**Mục đích:** Người dùng xong Zeichenzähler thường cũng cần Wörter zählen hoặc Lesbarkeit. Hiện tại họ phải tự tìm trong nav bar — nhiều người thoát luôn.

**Yêu cầu:**
- Đặt ngay trước footer, sau FAQ accordion
- Tiêu đề section: `<h2>Das könnte Sie auch interessieren</h2>` — KHÔNG dùng "Verwandte Tools" (quá kỹ thuật)
- Hiển thị 3 card tool liên quan nhất với mỗi tool (xem mapping bên dưới)
- Mỗi card gồm: icon emoji + tên tool (link) + 1 câu mô tả ngắn (~10 từ)
- Layout: 3 cột desktop, 1 cột mobile
- Style: border nhẹ, border-radius 8px, hover effect nhẹ (background đổi màu)
- KHÔNG thêm ad placeholder mới trong section này

**Mapping — 3 tool gợi ý cho từng tool:**

| Tool | Gợi ý 1 | Gợi ý 2 | Gợi ý 3 |
|---|---|---|---|
| passwort-generator | zufallszahlen | umlaut-konverter | textvergleich |
| zeichenzaehler | woerter-zaehlen | lesbarkeit | text-formatierung |
| woerter-zaehlen | zeichenzaehler | lesbarkeit | lorem-ipsum-deutsch |
| lesbarkeit | woerter-zaehlen | zeichenzaehler | text-formatierung |
| lorem-ipsum-deutsch | textvergleich | text-formatierung | woerter-zaehlen |
| text-formatierung | zeichenzaehler | umlaut-konverter | woerter-zaehlen |
| umlaut-konverter | text-formatierung | zeichenzaehler | textvergleich |
| textvergleich | woerter-zaehlen | lesbarkeit | zeichenzaehler |
| zufallszahlen | passwort-generator | textvergleich | lorem-ipsum-deutsch |

**Mô tả ngắn cho từng card (tiếng Đức, dùng y nguyên):**

```
passwort-generator  → "Sichere Zufallspasswörter in Sekunden erstellen."
zeichenzaehler      → "Zeichen und Wörter in Echtzeit zählen."
woerter-zaehlen     → "Wörter, Sätze und Lesezeit auf einen Blick."
lesbarkeit          → "Flesch-Index und Wiener Sachtextformel berechnen."
lorem-ipsum-deutsch → "Deutschen Blindtext für Layouts generieren."
text-formatierung   → "Groß-, Kleinschreibung und mehr umwandeln."
umlaut-konverter    → "Umlaute in ae/oe/ue umschreiben und zurück."
textvergleich       → "Zwei Texte vergleichen und Unterschiede finden."
zufallszahlen       → "Zufallszahlen generieren und als CSV exportieren."
```

---

### 2.2 — Bảng "Aktuelle Zeichenlimits" trên trang Zeichenzähler

**Mục đích:** Người dùng dùng Zeichenzähler vì cần biết giới hạn ký tự của các platform. Hiện tại sidebar chỉ có progress bar — không có con số tham chiếu cụ thể để nhớ hay bookmark. Đối thủ QuillBot có liệt kê limits nhưng chỉ trong FAQ, không có bảng tra cứu riêng.

**Yêu cầu:**
- Đặt ngay sau section "Für wen ist der Zeichenzähler geeignet?", trước ad-bottom
- Tiêu đề: `<h2>Zeichenlimits 2026: Alle Plattformen im Überblick</h2>`
- HTML `<table>` đơn giản, không JavaScript
- Responsive: trên mobile dùng `overflow-x: auto` wrapper

**Nội dung bảng — dùng y nguyên dữ liệu này:**

| Plattform | Feld | Limit |
|---|---|---|
| X (Twitter) | Beitrag | 280 |
| X (Twitter) | Bio | 160 |
| Instagram | Caption | 2.200 |
| Instagram | Bio | 150 |
| Instagram | Stories-Sticker | 500 |
| LinkedIn | Beitrag | 3.000 |
| LinkedIn | Headline | 220 |
| Facebook | Beitrag | 63.206 |
| Facebook | Seitenname | 75 |
| TikTok | Caption | 2.200 |
| TikTok | Bio | 80 |
| YouTube | Videotitel | 100 |
| YouTube | Beschreibung | 5.000 |
| Google Ads | Überschrift | 30 |
| Google Ads | Beschreibung | 90 |
| Meta Ads | Primärtext | 125 |
| Meta Ads | Überschrift | 40 |
| WhatsApp | Status | 139 |
| E-Mail | Betreffzeile (Empfehlung) | 50–60 |
| SEO Title | Google-Suchergebnis | 60 |
| SEO Description | Google-Suchergebnis | 160 |

**Ghi chú nhỏ bên dưới bảng (thêm vào HTML):**
```
<p class="table-note">Stand: März 2026. Plattformen können ihre Limits jederzeit ändern.</p>
```

**Styling bảng:**
- Header row: background accent color (#2563EB light / #60A5FA dark), text trắng
- Alternating row background nhẹ cho dễ đọc
- Font-size: 14px — đủ compact để không chiếm quá nhiều chỗ
- Border: 1px solid border color theo theme

---

### 2.3 — "Wie es funktioniert" 3-bước trên Homepage

**Mục đích:** Người lần đầu vào texthelfer.net thấy tool cards nhưng không rõ cần làm gì. 3-bước giải thích nhanh — đặc biệt quan trọng với người dùng lớn tuổi hoặc người không quen dùng tool online.

**Vị trí:** Giữa tool card grid và section "Was ist TextHelfer?" — tức là ngay sau hết 9 card

**Tiêu đề:** `<h2>So funktioniert TextHelfer</h2>`

**3 bước — nội dung tiếng Đức (dùng y nguyên):**

```
Schritt 1 — Tool auswählen
Wählen Sie eines der neun Texttools oben aus.
Jedes Tool ist für eine bestimmte Aufgabe gemacht.

Schritt 2 — Text eingeben
Tippen oder fügen Sie Ihren Text direkt ins Tool ein.
Keine Anmeldung, keine Installation, kein Warten.

Schritt 3 — Ergebnis ablesen
Das Ergebnis erscheint sofort, während Sie schreiben.
Alles läuft in Ihrem Browser. Kein Zeichen verlässt Ihr Gerät.
```

**Layout:** 3 cột ngang desktop (flex), xếp dọc mobile. Mỗi bước có số lớn (1 / 2 / 3) màu accent, tiêu đề bold, mô tả 2 câu bình thường.

**Quan trọng về giọng văn:** Bước 3 phải kết thúc bằng câu về privacy — đây là USP số 1 của TextHelfer so với đối thủ. Dùng y nguyên câu đã viết ở trên, không sáng tác lại.

---

### 2.4 — FAQPage JSON-LD cho Homepage

**Mục đích:** Homepage hiện chưa có FAQ section và schema. Tất cả đối thủ tool thuần tiếng Đức cũng không có — TextHelfer có thể chiếm rich snippet FAQ trên kết quả Google cho các query tổng quan như "was ist texthelfer" hay "online texttools deutsch kostenlos".

**Yêu cầu:**
- Thêm FAQ accordion HTML (dạng `<details>`/`<summary>` như các tool pages)
- Thêm FAQPage JSON-LD vào `<head>`
- Đặt FAQ sau section "Für wen ist TextHelfer?", trước footer
- Tiêu đề: `<h2>Häufige Fragen zu TextHelfer</h2>`
- Tối thiểu 6 câu hỏi

**Nội dung FAQ — 8 câu hỏi (dùng y nguyên, tiếng Đức):**

**F1: Ist TextHelfer wirklich kostenlos?**
Ja, alle neun Tools sind vollständig kostenlos. Es gibt keine Premium-Version, kein Zeichenlimit und keine versteckten Kosten. Sie müssen sich weder registrieren noch eine E-Mail-Adresse angeben.

**F2: Werden meine Texte gespeichert oder weitergegeben?**
Nein. Alle Berechnungen laufen direkt in Ihrem Browser. Kein Text, den Sie eingeben, wird an einen Server gesendet oder gespeichert. Das gilt auch für generierte Passwörter. TextHelfer hat schlicht keinen Server, der Daten empfangen könnte.

**F3: Funktionieren die Tools auch auf dem Smartphone?**
Ja. Alle Tools sind für mobile Geräte optimiert und funktionieren auf iPhone, Android-Smartphones und Tablets genauso wie auf dem Desktop. Kein App-Download nötig.

**F4: Warum gibt es TextHelfer nur auf Deutsch?**
Weil viele Aufgaben sprachspezifisch sind. Der Flesch-Index für Deutsche Texte verwendet andere Formeln als die englische Version. Die Wiener Sachtextformel gibt es nur für Deutsch. Und Umlaute erfordern eine eigene Behandlung, die englische Tools ignorieren.

**F5: Kann ich TextHelfer offline nutzen?**
Die Tools benötigen eine Internetverbindung beim ersten Laden der Seite. Sobald die Seite geladen ist, laufen alle Berechnungen lokal in Ihrem Browser, auch ohne aktive Verbindung.

**F6: Wie viele Tools gibt es bei TextHelfer?**
Aktuell neun: Passwort Generator, Zeichenzähler, Wörter zählen, Lesbarkeit prüfen, Lorem Ipsum Deutsch, Text Formatierung, Umlaut Konverter, Textvergleich und Zufallszahlen Generator. Alle Tools decken verschiedene Aufgaben rund um deutschen Text ab.

**F7: Gibt es eine App für TextHelfer?**
Nein, und das ist Absicht. TextHelfer funktioniert direkt im Browser, ohne Installation. Das spart Speicherplatz auf Ihrem Gerät und bedeutet: Sie haben immer die aktuelle Version, ohne Updates zu installieren.

**F8: Wer steckt hinter TextHelfer?**
TextHelfer ist ein unabhängiges Projekt, das Textwerkzeuge speziell für die deutsche Sprache entwickelt. Bei Fragen oder Feedback erreichen Sie uns über die Kontaktseite.

---

## 3. TECH NOTES

> Ràng buộc kỹ thuật bất biến — không được bỏ qua:

- Toàn bộ CSS + JS inline trong từng file HTML — không tạo file CSS/JS riêng
- KHÔNG dùng framework hay external library
- Dark mode phải hoạt động cho tất cả element mới (bảng, card, section)
- `<html lang="de">` giữ nguyên trên tất cả file
- Dark mode script vẫn phải nằm trong `<head>` — không di chuyển

**Màu sắc cho element mới:**

```css
/* Table header */
Light: background #2563EB, color #FFFFFF
Dark:  background #1E40AF, color #F1F5F9

/* Table row alternating */
Light: odd #FFFFFF, even #F8FAFC
Dark:  odd #1E293B, even #0F172A

/* Verwandte Tools cards */
Light: background #F8FAFC, border #E2E8F0, hover #EFF6FF
Dark:  background #1E293B, border #334155, hover #1E3A5F

/* 3-step numbers */
Light + Dark: color accent (#2563EB / #60A5FA)
```

**FAQ JSON-LD homepage:** Thêm vào `<head>` của `index.html` cùng định dạng với các tool pages — `@type: FAQPage`, `mainEntity` array 8 Q&A.

---

## 4. ACCEPTANCE CRITERIA

```
NHÓM 1 — Verwandte Tools
[ ] Tất cả 9 tool pages có section "Das könnte Sie auch interessieren"
    Kiểm tra: Mở mỗi tool, scroll xuống cuối — section hiển thị đúng vị trí
    Kết quả: ___________

[ ] Mỗi tool hiển thị đúng 3 card theo mapping đã định
    Kiểm tra: Mở passwort-generator → thấy zufallszahlen, umlaut-konverter, textvergleich
    Kết quả: ___________

[ ] Tất cả link trong cards trỏ đúng URL tool tương ứng
    Kiểm tra: Click từng card trên 2 tool bất kỳ
    Kết quả: ___________

[ ] Dark mode: cards hiển thị đúng màu trong cả 2 theme
    Kết quả: ___________

[ ] Mobile 375px: 3 cards xếp thành 1 cột, không overflow
    Kết quả: ___________

NHÓM 2 — Bảng Zeichenlimits
[ ] Bảng hiển thị đủ 21 hàng dữ liệu, đúng số liệu
    Kiểm tra: Đếm hàng trong bảng, spot check 3 platform (X=280, Google Ads headline=30, SEO Description=160)
    Kết quả: ___________

[ ] Ghi chú "Stand: März 2026" hiển thị bên dưới bảng
    Kết quả: ___________

[ ] Mobile 375px: bảng scroll ngang được, không bị cắt
    Kết quả: ___________

[ ] Dark mode: header row và alternating rows đúng màu
    Kết quả: ___________

NHÓM 3 — 3-bước Homepage
[ ] Section "So funktioniert TextHelfer" hiển thị sau tool cards, trước "Was ist TextHelfer?"
    Kết quả: ___________

[ ] Đúng 3 bước với nội dung đúng — đặc biệt Schritt 3 có câu về privacy
    Kiểm tra: Đọc Schritt 3 — phải thấy "Kein Zeichen verlässt Ihr Gerät"
    Kết quả: ___________

[ ] Mobile: 3 bước xếp dọc, đọc được, không overflow
    Kết quả: ___________

NHÓM 4 — FAQ Homepage
[ ] FAQ accordion có đủ 8 câu hỏi, tất cả mở/đóng được
    Kiểm tra: Click lần lượt từng câu hỏi
    Kết quả: ___________

[ ] FAQPage JSON-LD có trong <head> của index.html
    Kiểm tra: View source → Ctrl+F → "FAQPage"
    Kết quả: ___________

[ ] JSON-LD hợp lệ tại schema.org/validator
    Kết quả: ___________

[ ] Nội dung FAQ đúng — F2 phải nêu rõ "kein Server, der Daten empfangen könnte"
    Kết quả: ___________

CHUNG
[ ] Lighthouse Performance ≥ 90 trên homepage và zeichenzaehler sau khi update
    Score: ___________

[ ] 0 console errors trên tất cả trang đã chỉnh
    Kết quả: ___________

[ ] Dark mode không flash khi reload — kiểm tra trên homepage và 1 tool
    Kết quả: ___________

[ ] Dev paste nội dung Schritt 1–3 thực tế từ homepage để PM review
    Nội dung: ___________

[ ] Dev paste 2 câu đầu của F2 (câu hỏi về lưu dữ liệu) từ homepage để PM review
    Nội dung: ___________
```

---

## 5. DELIVERABLES

- [ ] `zeichenzaehler/index.html` — thêm bảng Zeichenlimits
- [ ] `passwort-generator/index.html` — thêm Verwandte Tools
- [ ] `woerter-zaehlen/index.html` — thêm Verwandte Tools
- [ ] `lesbarkeit/index.html` — thêm Verwandte Tools
- [ ] `lorem-ipsum-deutsch/index.html` — thêm Verwandte Tools
- [ ] `text-formatierung/index.html` — thêm Verwandte Tools
- [ ] `umlaut-konverter/index.html` — thêm Verwandte Tools
- [ ] `textvergleich/index.html` — thêm Verwandte Tools
- [ ] `zufallszahlen/index.html` — thêm Verwandte Tools
- [ ] `index.html` — thêm 3-bước + FAQ accordion + FAQPage JSON-LD
- [ ] Commit file brief này với status: `DONE`

---

## 6. GHI CHÚ QUAN TRỌNG VỀ GIỌNG VĂN

> Dev đọc kỹ phần này trước khi viết bất kỳ chữ nào.

**KHÔNG dùng các mẫu câu sau — người đọc nhận ra AI ngay:**
- „In der heutigen Zeit..." / „Es ist wichtig zu wissen..."
- „Nicht nur... sondern auch..."
- „Zusammenfassend lässt sich sagen..."
- Dấu em dash (—) bất kỳ chỗ nào
- Câu bắt đầu bằng „Dies" hoặc „Das" lặp liên tiếp
- „erstens... zweitens... drittens..." trong văn xuôi

**LUÔN viết theo phong cách:**
- Câu ngắn đến trung bình (10–18 từ), xen kẽ nhịp nhanh-chậm
- Cụ thể, có ví dụ thực — không lý thuyết chung chung
- Giọng thân thiện, như đồng nghiệp giải thích cho nhau — không phải hướng dẫn sử dụng
- Từ khóa xuất hiện tự nhiên trong văn cảnh, không bao giờ đứng một mình hoặc lặp liên tiếp trong 1 đoạn

**Ví dụ tốt (từ TASK-012 đã được duyệt):**
> „Du sitzt an einem MacBook mit englischem Tastaturlayout und musst eine E-Mail an Herrn Müller schreiben. Wo ist das ü?"

**Ví dụ xấu (không được làm):**
> „In der heutigen digitalisierten Welt ist es wichtig, die richtigen Texttools zur Hand zu haben. Nicht nur für Profis, sondern auch für Einsteiger bietet TextHelfer erstklassige Lösungen."

---

> **Prompt giao Dev:**
> ```
> Bạn là Dev Agent của dự án TextHelfer.
> Đọc task brief tại: docs/tasks/TASK-014-content-depth-ux.md
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
