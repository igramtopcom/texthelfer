# TASK-006 — Text Formatierung

**Ngày tạo:** 2026-03-09
**PM:** TextHelfer PM Agent
**Assignee:** Dev Agent (Claude CLI)
**Priority:** P1
**Status:** TODO
**Branch:** feature/TASK-006-text-formatierung
**Estimate:** 1–2 ngày

---

## 1. CONTEXT

Text Formatierung là tool chuyển đổi định dạng chữ và dọn dẹp văn bản tiếng Đức. Volume 5.000–9.000/tháng, KD 10–18. Điểm độc quyền: tính năng **"Substantive großschreiben"** — tự động viết hoa danh từ tiếng Đức theo quy tắc ngữ pháp, dựa trên noun list ~5.000 từ phổ biến nhất. Không có tool tiếng Anh hay tiếng Đức nào làm được điều này. Đây là lý do chính để người dùng Đức chọn TextHelfer thay vì công cụ nước ngoài.

---

## 2. REQUIREMENTS

### Chức năng bắt buộc:
- [ ] Textarea INPUT lớn, placeholder: *"Deinen Text hier eingeben..."*
- [ ] Textarea OUTPUT (readonly) — hiển thị kết quả đã format
- [ ] **7 nút chuyển đổi** (bấm 1 nút → áp dụng ngay vào output):

| Nút | Chức năng | Ví dụ |
|---|---|---|
| GROSSBUCHSTABEN | Tất cả in hoa | "hallo welt" → "HALLO WELT" |
| kleinbuchstaben | Tất cả chữ thường | "HALLO WELT" → "hallo welt" |
| Ersten Buchstaben | Hoa chữ đầu tiên của toàn văn bản | "hallo welt" → "Hallo welt" |
| Jeden Satz | Hoa đầu mỗi câu | "hallo. wie geht es?" → "Hallo. Wie geht es?" |
| Substantive | Viết hoa danh từ tiếng Đức (noun list) | "der hund läuft" → "der Hund läuft" |
| Spiegel-Text | Đảo ngược toàn bộ văn bản | "hallo" → "ollah" |
| Text bereinigen | Xóa khoảng trắng thừa + ngắt dòng thừa | "hallo   welt\n\n\ntest" → "hallo welt\n\ntest" |

- [ ] Nút **Kopieren** (output) — copy kết quả, toast "Kopiert!" 2 giây
- [ ] Nút **Tauschen** — hoán đổi nội dung: output → input (để tiếp tục format)
- [ ] Nút **Zurücksetzen** — xóa cả input lẫn output
- [ ] **Bộ đếm ký tự** nhỏ dưới mỗi textarea: "X Zeichen"
- [ ] Dark mode toggle

### Chức năng không được phá vỡ:
- [ ] Khi input trống và bấm nút format → output hiển thị chuỗi rỗng, không crash
- [ ] Khi đổi theme → text trong cả 2 textarea không bị mất

---

## 3A. TECH NOTES

### Logic từng chức năng:

```javascript
// 1. GROSSBUCHSTABEN
text.toUpperCase()

// 2. kleinbuchstaben
text.toLowerCase()

// 3. Ersten Buchstaben großschreiben
text.charAt(0).toUpperCase() + text.slice(1).toLowerCase()

// 4. Jeden Satz großschreiben
text.toLowerCase().replace(/(^\s*|\.\s+|!\s+|\?\s+)([a-züäöß])/g,
  (match, p1, p2) => p1 + p2.toUpperCase())

// 5. Substantive großschreiben — dùng NOUN_SET (xem bên dưới)
text.split(/\b/).map(word => {
  const clean = word.toLowerCase().replace(/[^a-züäöß]/g, '');
  return NOUN_SET.has(clean) ? word.charAt(0).toUpperCase() + word.slice(1) : word;
}).join('')

// 6. Spiegel-Text
text.split('').reverse().join('')

// 7. Text bereinigen
text
  .replace(/[ \t]+/g, ' ')           // khoảng trắng thừa trong dòng
  .replace(/\n{3,}/g, '\n\n')        // > 2 dòng trống → 2 dòng trống
  .trim()
```

### NOUN_SET — 120 danh từ tiếng Đức phổ biến nhất (dùng JavaScript Set):

> Đây là bộ danh từ rút gọn cho tool. Dev hardcode danh sách này dưới dạng `const NOUN_SET = new Set([...])` — lookup O(1).

```javascript
const NOUN_SET = new Set([
  // Người và gia đình
  'mann', 'frau', 'kind', 'mutter', 'vater', 'bruder', 'schwester', 'familie',
  'mensch', 'person', 'freund', 'freundin', 'kollege', 'chef', 'kunde',
  // Nơi chốn
  'haus', 'wohnung', 'zimmer', 'küche', 'garten', 'stadt', 'dorf', 'land',
  'straße', 'weg', 'platz', 'schule', 'universität', 'büro', 'laden', 'markt',
  'bahnhof', 'flughafen', 'krankenhaus', 'kirche', 'park', 'wald', 'berg', 'see',
  // Thời gian
  'tag', 'nacht', 'morgen', 'abend', 'stunde', 'minute', 'woche', 'monat', 'jahr',
  'montag', 'dienstag', 'mittwoch', 'donnerstag', 'freitag', 'samstag', 'sonntag',
  'januar', 'februar', 'märz', 'april', 'mai', 'juni', 'juli', 'august',
  'september', 'oktober', 'november', 'dezember',
  // Vật thể thường gặp
  'tisch', 'stuhl', 'bett', 'fenster', 'tür', 'buch', 'heft', 'stift', 'papier',
  'computer', 'handy', 'telefon', 'auto', 'bus', 'zug', 'fahrrad', 'tasche',
  'flasche', 'glas', 'tasse', 'teller', 'messer', 'löffel', 'gabel',
  // Thiên nhiên / động vật
  'hund', 'katze', 'vogel', 'pferd', 'fisch', 'baum', 'blume', 'gras',
  'sonne', 'mond', 'stern', 'wolke', 'regen', 'schnee', 'wind', 'wasser', 'feuer',
  // Công việc / xã hội
  'arbeit', 'beruf', 'firma', 'geld', 'preis', 'produkt', 'service', 'projekt',
  'aufgabe', 'problem', 'lösung', 'ergebnis', 'bericht', 'vertrag', 'rechnung',
  // Trừu tượng phổ biến
  'idee', 'plan', 'ziel', 'weg', 'möglichkeit', 'frage', 'antwort', 'thema',
  'beispiel', 'grund', 'unterschied', 'vorteil', 'nachteil', 'bedeutung'
]);
```

> **Lưu ý:** Danh sách 120 từ này là bộ rút gọn đủ để demo tính năng và test. Trong thực tế production, Dev có thể mở rộng lên 500–1.000 từ nếu muốn.

### SEO bắt buộc:
- **Title:** `Text Formatierung Online Kostenlos | TextHelfer` (48 ký tự)
- **Meta description:** `Text formatieren kostenlos: Groß- und Kleinbuchstaben umwandeln, Substantive automatisch großschreiben, Text bereinigen. Speziell für deutsche Texte. Keine Anmeldung.` (162 ký tự → Dev điều chỉnh về ≤160)
- **Canonical:** `<link rel="canonical" href="https://texthelfer.net/text-formatierung/">`
- **H1:** `Text Formatierung – Deutschen Text online formatieren`
- **WebApplication JSON-LD** + **FAQPage JSON-LD** (≥ 6 Q&A)

---

## 3B. UI / DESIGN SPEC

> Kế thừa CSS variables, header, footer từ TASK-001.

---

### LAYOUT TỔNG THỂ

```
┌─────────────────────────────────────────────────────────────┐
│  HEADER                                                     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  MAIN (max-width: 900px, margin: 0 auto, padding: 24px)     │
│                                                             │
│  H1: Text Formatierung – Deutschen Text online formatieren  │
│  Subtitle: Groß-/Kleinschreibung und mehr...                │
│                                                             │
│  ┌──────────────── FORMAT BUTTONS ───────────────────────┐  │
│  │  [GROSS]  [klein]  [Ersten]  [Satz]                   │  │
│  │  [Substantive ⭐]  [Spiegel]  [Bereinigen]            │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                             │
│  ┌── INPUT ────────────┐  ┌── OUTPUT ───────────────────┐   │
│  │                     │  │                             │   │
│  │  [Textarea input]   │  │  [Textarea output]          │   │
│  │  min-height: 240px  │  │  readonly                   │   │
│  │                     │  │  min-height: 240px          │   │
│  │  123 Zeichen        │  │  123 Zeichen                │   │
│  └─────────────────────┘  └─────────────────────────────┘   │
│                                                             │
│  [← Tauschen]                          [Kopieren] [Reset]   │
│                                                             │
│  ┌── AD: id="ad-top" ─────────────────────────────────────┐ │
│  └────────────────────────────────────────────────────────┘ │
│                                                             │
│  H2: Wie funktioniert die Text Formatierung?                │
│                                                             │
│  ┌── AD: id="ad-middle" ──────────────────────────────────┐ │
│  └────────────────────────────────────────────────────────┘ │
│                                                             │
│  H2: Für wen ist das geeignet?                              │
│                                                             │
│  ┌── AD: id="ad-bottom" ──────────────────────────────────┐ │
│  └────────────────────────────────────────────────────────┘ │
│                                                             │
│  H2: Häufige Fragen                                         │
├─────────────────────────────────────────────────────────────┤
│  FOOTER                                                     │
└─────────────────────────────────────────────────────────────┘
```

**Mobile (< 768px):** 2 textarea xếp dọc (input trên, output dưới). Button grid xuống 2 dòng.

---

### FORMAT BUTTONS

```
display: flex; flex-wrap: wrap; gap: 10px
margin-bottom: 20px

Mỗi nút format:
  height: 44px; padding: 0 18px
  background: var(--bg-card)
  border: 1px solid var(--border)
  border-radius: 8px
  font-size: 14px; font-weight: 500
  color: var(--text); cursor: pointer
  transition: all 0.15s
  white-space: nowrap

  :hover → background var(--accent); color #fff; border-color var(--accent)
  :active → transform scale(0.96)

Nút "Substantive großschreiben" — nổi bật hơn các nút khác:
  background: var(--accent); color: #ffffff; border-color: var(--accent)
  font-weight: 600
  :hover → background var(--accent-hover)
  Dark mode: background #60A5FA; color #0F172A
```

---

### TEXTAREA AREA

```
display: flex; gap: 16px; align-items: flex-start
(mobile: flex-direction column)

Mỗi textarea wrapper:
  flex: 1; display flex; flex-direction column; gap 6px

Label:
  font-size: 13px; font-weight: 600; text-transform: uppercase
  letter-spacing: 0.5px; color: var(--text-muted)
  "EINGABE" (trái) / "ERGEBNIS" (phải)

Textarea:
  width: 100%; min-height: 240px; resize: vertical
  padding: 14px 16px
  font-size: 17px; line-height: 1.7
  color: var(--text); background: var(--bg)
  border: 2px solid var(--border); border-radius: 10px
  font-family: system-ui; outline: none
  transition: border-color 0.2s

  Input textarea: :focus → border-color var(--accent)
  Output textarea (readonly):
    background: var(--bg-card)
    cursor: default
    :focus → border-color var(--border) (không đổi)

Bộ đếm ký tự:
  font-size: 12px; color: var(--text-muted); text-align: right
  "X Zeichen"
```

---

### ACTION BUTTONS (dưới 2 textarea)

```
display: flex; justify-content: space-between; align-items: center
margin-top: 12px

Bên trái — nút Tauschen:
  height: 44px; padding: 0 16px
  background: transparent
  border: 1px solid var(--border); border-radius: 8px
  font-size: 14px; font-weight: 500; color var(--text-muted)
  cursor: pointer
  Icon: "⇄" trước label "Tauschen"
  :hover → border-color var(--accent); color var(--accent)

Bên phải — 2 nút nhóm lại:
  display flex; gap 10px

  Nút Kopieren:
    height: 44px; padding: 0 18px
    background: var(--accent); color: #fff
    border: none; border-radius: 8px
    font-size: 14px; font-weight: 600; cursor pointer
    :hover → var(--accent-hover)

  Nút Zurücksetzen:
    height: 44px; padding: 0 16px
    background: transparent
    border: 1px solid var(--border); border-radius: 8px
    font-size: 14px; color var(--text-muted); cursor pointer
    :hover → border-color var(--danger); color var(--danger)
```

---

### RESPONSIVE — MOBILE (< 768px)

| Thay đổi | Desktop | Mobile |
|---|---|---|
| Textarea layout | flex row (2 cột) | flex column (xếp dọc) |
| Format buttons | 1 hàng flex-wrap | 2 hàng, mỗi nút flex: 1 |
| Textarea min-height | 240px | 160px |
| Action buttons | space-between | stack dọc, gap 8px |
| H1 font-size | 28px | 22px |

---

## 4. ACCEPTANCE CRITERIA

### Functional:
- [ ] GROSSBUCHSTABEN → output tất cả in hoa, kể cả ä→Ä, ö→Ö, ü→Ü
- [ ] kleinbuchstaben → output tất cả chữ thường
- [ ] Ersten Buchstaben → chỉ chữ đầu tiên toàn văn bản viết hoa
- [ ] Jeden Satz → hoa đầu mỗi câu sau dấu `.` `!` `?`
- [ ] Substantive → các từ có trong NOUN_SET được viết hoa, từ không trong list giữ nguyên
- [ ] Spiegel-Text → toàn bộ text đảo ngược ký tự
- [ ] Text bereinigen → xóa khoảng trắng thừa, giảm > 2 dòng trống về 2
- [ ] Tauschen → nội dung output chuyển sang input
- [ ] Zurücksetzen → cả 2 textarea về rỗng
- [ ] Kopieren → copy nội dung output → toast "Kopiert!" 2 giây
- [ ] Bộ đếm ký tự cập nhật real-time cho cả 2 textarea
- [ ] Input trống + bấm format → output rỗng, không crash

### Design:
- [ ] Nút "Substantive" nổi bật: background accent, màu trắng
- [ ] 6 nút còn lại: background card, border, hover → đổi màu accent
- [ ] 2 textarea cạnh nhau desktop, xếp dọc mobile
- [ ] Output textarea: background bg-card, không editable
- [ ] Label "EINGABE" / "ERGEBNIS": uppercase, text-muted

### SEO & Technical:
- [ ] `<html lang="de">` có trong source
- [ ] Title ≤ 60 ký tự, keyword "text formatierung" đứng đầu
- [ ] 1 thẻ H1 duy nhất
- [ ] Canonical: `https://texthelfer.net/text-formatierung/`
- [ ] WebApplication + FAQPage JSON-LD hợp lệ, ≥ 6 Q&A
- [ ] Lighthouse Performance ≥ 90, FCP < 1 giây

### UX:
- [ ] Desktop 1920px: layout đúng
- [ ] Mobile 375px: 2 textarea xếp dọc, không overflow
- [ ] Tất cả nút height ≥ 44px
- [ ] Dark mode: không flash khi reload, giữ preference
- [ ] Đổi theme → text trong cả 2 textarea không mất

### Integration:
- [ ] Navigation bar: 9 tool links
- [ ] Footer: link Kontakt
- [ ] ≥ 3 internal links ngữ cảnh (gợi ý: Zeichenzähler, Umlaut Konverter, Wörter zählen)
- [ ] 3 ad placeholders: `id="ad-top"`, `id="ad-middle"`, `id="ad-bottom"`

### Deploy:
- [ ] PR merged vào `main`
- [ ] GitHub Actions deploy thành công
- [ ] URL live: `https://texthelfer.net/text-formatierung/`
- [ ] HTTPS, không mixed content warning

---

## 5. DELIVERABLES

Dev commit lên Git khi hoàn thành:

- [ ] `text-formatierung/index.html` — file tool hoàn chỉnh
- [ ] Cập nhật `index.html` — mark card Text Formatierung là live
- [ ] Cập nhật `sitemap.xml` — thêm `/text-formatierung/`
- [ ] Cập nhật status brief này: `DONE`

---

## 6. NỘI DUNG SEO TIẾNG ĐỨC

### H2: Wie funktioniert die Text Formatierung?
*(Dev viết ~150 từ, keyword xuất hiện 2–3 lần tự nhiên)*

Gợi ý: giải thích 7 tính năng, nhấn mạnh tính năng Substantive là độc quyền cho tiếng Đức, ứng dụng cho content writer copy-paste từ nguồn khác nhau, tính năng bereinigen để dọn dẹp text từ PDF hay Word.

### H2: Für wen ist die Text Formatierung geeignet?
- Content Writer, die Texte aus verschiedenen Quellen einheitlich formatieren
- Redakteure, die Rohtexte schnell in publikationsreife Form bringen
- Studierende, die kopierte Texte für Präsentationen aufbereiten
- Social-Media-Manager, die Texte für verschiedene Plattformen anpassen
- Alle, die schnell zwischen Groß- und Kleinschreibung wechseln müssen

### FAQ — 6 Q&A:

**F1:** Wie schreibt man Substantive groß im Deutschen?
**A1:** Im Deutschen werden alle Substantive (Nomen) großgeschrieben — das ist eine der bekanntesten Besonderheiten der deutschen Sprache. Dazu gehören nicht nur konkrete Dinge wie "Haus" und "Auto", sondern auch abstrakte Begriffe wie "Liebe" und "Freiheit" sowie nominalisierte Verben und Adjektive wie "das Laufen" oder "das Schöne". Unser Tool erkennt automatisch die häufigsten deutschen Substantive.

**F2:** Was macht die Funktion "Text bereinigen"?
**A2:** Die Funktion "Text bereinigen" entfernt mehrfache Leerzeichen innerhalb von Zeilen und reduziert mehr als zwei aufeinanderfolgende Leerzeilen auf zwei. Das ist besonders nützlich, wenn du Text aus PDFs, Word-Dokumenten oder Webseiten kopierst, der oft unerwünschte Formatierungsartefakte enthält. Das Ergebnis ist ein sauber strukturierter Text ohne störende Lücken.

**F3:** Was ist ein Spiegel-Text?
**A3:** Ein Spiegel-Text entsteht durch das Umkehren der Reihenfolge aller Zeichen im Text. Aus "Hallo Welt" wird zum Beispiel "tleW ollaH". Spiegel-Texte werden für Rätsel, kreative Projekte, Wasserzeichen-Effekte oder einfach zum Spaß verwendet. Unsere Funktion kehrt den gesamten Text zeichenweise um, inklusive Leerzeichen und Sonderzeichen.

**F4:** Was ist der Unterschied zwischen "Ersten Buchstaben" und "Jeden Satz"?
**A4:** "Ersten Buchstaben großschreiben" macht nur den allerersten Buchstaben des gesamten Textes groß und setzt den Rest auf Kleinschreibung. "Jeden Satz großschreiben" hingegen erkennt Satzgrenzen (nach Punkt, Ausrufezeichen und Fragezeichen) und schreibt den ersten Buchstaben jedes Satzes groß. Die zweite Option ist für normale Texte deutlich sinnvoller.

**F5:** Werden meine Texte gespeichert?
**A5:** Nein. Alle Formatierungen werden ausschließlich in deinem Browser durchgeführt. Kein Text wird an unsere Server übertragen oder gespeichert. Du kannst das Tool bedenkenlos für vertrauliche Dokumente, Geschäftstexte oder persönliche Inhalte verwenden.

**F6:** Kann ich mehrere Formatierungen nacheinander anwenden?
**A6:** Ja. Nach jeder Formatierung kannst du den "Tauschen"-Button verwenden, um das Ergebnis in das Eingabefeld zu übertragen und anschließend eine weitere Formatierung anzuwenden. So kannst du zum Beispiel erst alle Buchstaben kleinschreiben und dann jeden Satz großschreiben — in zwei einfachen Schritten.

---

## 7. PROMPT GIAO CHO DEV (Claude CLI)

Owner paste đoạn sau vào Claude CLI:

```
Bạn là Dev Agent của dự án TextHelfer.

Đọc task brief tại: docs/tasks/TASK-006-text-formatierung.md

Lưu ý:
- Tool này có layout đặc biệt: 2 textarea song song (input trái, output phải)
- Nút "Substantive großschreiben" dùng NOUN_SET đã viết sẵn trong Tech Notes — copy nguyên vào code
- Nút Tauschen chuyển output → input để tiếp tục format tiếp

Sau khi đọc xong:
1. Tóm tắt lại 7 chức năng format trong 3–5 câu
2. Liệt kê những gì bạn sẽ làm theo thứ tự
3. Hỏi nếu có gì chưa rõ

Chờ xác nhận từ Owner trước khi bắt đầu code.
```

---

**PM Sign-off:** [ ] Chờ ký duyệt
**Ngày ký duyệt:** ___________
**Ghi chú ký duyệt:** ___________
