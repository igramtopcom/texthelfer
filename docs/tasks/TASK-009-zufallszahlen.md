# TASK-009 — Zufallszahlen Generator

**Ngày tạo:** 2026-03-09
**PM:** TextHelfer PM Agent
**Assignee:** Dev Agent (Claude CLI)
**Priority:** P2
**Status:** TODO
**Branch:** feature/TASK-009-zufallszahlen
**Estimate:** 1 ngày

---

## 1. CONTEXT

Giáo viên quay số học sinh, người tổ chức bốc thăm, developer cần số ngẫu nhiên cho demo, và người làm thống kê là các nhóm người dùng chính của tool này. Volume tìm kiếm 7K–14K/tháng tại DACH, không có đối thủ tiếng Đức mạnh. Tool tương đối đơn giản về mặt UI nhưng có 2 tính năng độc đáo so với đối thủ: **xuất CSV** và **lịch sử 5 lần gần nhất trong session**. Đây là task cuối cùng trong backlog 9 tool — sau khi hoàn thành, TextHelfer có đủ bộ sản phẩm để apply AdSense.

---

## 2. REQUIREMENTS

### Chức năng bắt buộc:
- [ ] Input **Min** và **Max**: số nguyên, Min mặc định = 1, Max mặc định = 100
- [ ] Input **Anzahl** (số lượng cần tạo): 1–1.000, mặc định = 1
- [ ] Toggle **„Keine Duplikate"**: khi bật, các số trong kết quả không trùng nhau. Khi Anzahl > (Max - Min + 1) và Keine Duplikate bật → hiển thị lỗi „Nicht genug einzigartige Zahlen im Bereich [Min]–[Max]"
- [ ] Nút **„Generieren"** (màu `#2563EB`, cao ≥52px): tạo số ngẫu nhiên
- [ ] Kết quả hiển thị to, rõ ràng (font 48px+ khi chỉ 1 số, dạng list/grid khi nhiều số)
- [ ] Nút **„Kopieren"**: copy toàn bộ kết quả vào clipboard (các số cách nhau bằng dấu phẩy), toast „Kopiert!" 2 giây
- [ ] Nút **„Als CSV exportieren"**: tạo file CSV tải về, tên file `zufallszahlen.csv`, mỗi số trên 1 dòng
- [ ] **Lịch sử 5 lần gần nhất** trong session: hiển thị dưới kết quả, mỗi entry ghi „[timestamp] Min–Max, X Zahlen: [kết quả rút gọn...]". Click vào entry để load lại kết quả đó
- [ ] Validation input: Min phải < Max, Anzahl phải ≥ 1 và ≤ 1.000, chỉ chấp nhận số nguyên — hiển thị lỗi tiếng Đức rõ ràng nếu sai

### Chức năng không được phá vỡ:
- [ ] Dùng `crypto.getRandomValues()` — tuyệt đối không dùng `Math.random()`
- [ ] Xuất CSV dùng Blob + trigger download, không gửi dữ liệu lên server
- [ ] Dark mode không làm vỡ layout kết quả và lịch sử
- [ ] SEO content và FAQ accordion không bị xóa

---

## 3. TECH NOTES

> Các ràng buộc kỹ thuật bất biến — Dev KHÔNG được bỏ qua:

- `<html lang="de">` trên thẻ html — bắt buộc
- Font: `system-ui` — KHÔNG dùng Google Fonts
- Toàn bộ CSS + JS inline trong 1 file HTML duy nhất
- KHÔNG dùng framework (React, Vue...) hoặc external JS library
- Dùng `crypto.getRandomValues()` — không dùng `Math.random()`
- Dark mode: script đọc `localStorage.getItem("texthelfer-theme")` đặt trong `<head>`, KHÔNG ở cuối body
- Navigation header: link đến tất cả 9 tool + footer link Kontakt
- 3 Ad placeholder: `id="ad-top"` (sau nút hành động), `id="ad-middle"` (sau H2 đầu), `id="ad-bottom"` (trước FAQ)
- Màu sắc:
  - Light: bg `#FFFFFF`, text `#111827`, accent `#2563EB`, border `#E2E8F0`
  - Dark: bg `#0F172A`, text `#F1F5F9`, accent `#60A5FA`, border `#334155`
- localStorage key cho theme: `"texthelfer-theme"`

### Thuật toán random không trùng (Keine Duplikate):
Dùng Fisher-Yates shuffle trên mảng [Min..Max], lấy Anzahl phần tử đầu:
```javascript
// Tạo mảng [min..max]
const pool = Array.from({length: max - min + 1}, (_, i) => min + i);
// Fisher-Yates shuffle dùng crypto.getRandomValues()
for (let i = pool.length - 1; i > 0; i--) {
    const arr = new Uint32Array(1);
    crypto.getRandomValues(arr);
    const j = arr[0] % (i + 1);
    [pool[i], pool[j]] = [pool[j], pool[i]];
}
return pool.slice(0, anzahl);
```
> ⚠️ Lưu ý: khi Max - Min + 1 rất lớn (ví dụ 1–1.000.000) và Anzahl nhỏ, shuffle toàn bộ mảng sẽ chậm. Dev có thể dùng Set-based approach thay thế khi pool > 10.000 phần tử.

### Xuất CSV:
```javascript
const csv = numbers.join('\n');
const blob = new Blob([csv], {type: 'text/csv'});
const url = URL.createObjectURL(blob);
const a = document.createElement('a');
a.href = url;
a.download = 'zufallszahlen.csv';
a.click();
URL.revokeObjectURL(url);
```

### SEO bắt buộc:
- **Title:** `Zufallszahlen Generator Online Kostenlos | TextHelfer` (51 ký tự)
- **Meta description:** `Zufallszahlen online generieren – mit oder ohne Duplikate, als CSV exportieren. Kostenloser Zufallszahlen Generator, keine Anmeldung erforderlich.` (148 ký tự)
- **Canonical:** `<link rel="canonical" href="https://texthelfer.net/zufallszahlen/">`
- **H1:** `Zufallszahlen Generator – Zufallszahlen online erstellen`
- WebApplication JSON-LD schema (price: "0", priceCurrency: "EUR")
- FAQPage JSON-LD schema (tối thiểu 6 Q&A tiếng Đức — xem mục 6)
- Nội dung SEO: H2 „Wie funktioniert der Zufallszahlen Generator?" (~150 từ) + H2 „Für wen ist der Zufallszahlen Generator geeignet?" (4–5 bullets) + H2 „Häufige Fragen" (6–8 FAQ accordion)

---

## 4. ACCEPTANCE CRITERIA

> PM sẽ dùng danh sách này ở Bước 6 để ký duyệt.
> Dev dùng danh sách này ở Bước 5 để tự test và báo cáo theo format test case.

### Functional:
```
[ ] Min=1, Max=10, Anzahl=1 → 1 số nguyên trong khoảng 1–10 hiển thị to
    Kết quả thực tế: ___________

[ ] Min=1, Max=100, Anzahl=10, Keine Duplikate=OFF → 10 số, có thể trùng
    Kết quả thực tế: ___________

[ ] Min=1, Max=10, Anzahl=10, Keine Duplikate=ON → 10 số, không trùng (chính xác 1–10)
    Kết quả thực tế: ___________

[ ] Min=1, Max=5, Anzahl=10, Keine Duplikate=ON → lỗi "Nicht genug einzigartige Zahlen im Bereich 1–5"
    Kết quả thực tế: ___________

[ ] Min=50, Max=10 → lỗi validation "Min muss kleiner als Max sein"
    Kết quả thực tế: ___________

[ ] Anzahl=1.001 → lỗi validation "Maximal 1.000 Zahlen"
    Kết quả thực tế: ___________

[ ] Nút "Kopieren": Min=1, Max=10, Anzahl=3 → clipboard chứa "X,Y,Z", toast "Kopiert!" 2 giây
    Kết quả thực tế: ___________

[ ] Nút "Als CSV exportieren" → file zufallszahlen.csv tải về, mỗi số trên 1 dòng
    Kết quả thực tế: ___________

[ ] Tạo 3 lần liên tiếp → lịch sử hiển thị 3 entry theo thứ tự mới nhất trên cùng
    Kết quả thực tế: ___________

[ ] Click vào entry lịch sử → kết quả được load lại đúng
    Kết quả thực tế: ___________

[ ] Tạo 6 lần → lịch sử chỉ giữ 5 entry gần nhất (entry cũ nhất bị xóa)
    Kết quả thực tế: ___________
```

### SEO & Technical:
- [ ] `<html lang="de">` có trong source
- [ ] Title: `Zufallszahlen Generator Online Kostenlos | TextHelfer` — 50–60 ký tự
- [ ] 1 thẻ H1 duy nhất: `Zufallszahlen Generator – Zufallszahlen online erstellen`
- [ ] Canonical URL: `https://texthelfer.net/zufallszahlen/`
- [ ] WebApplication JSON-LD hợp lệ
- [ ] FAQPage JSON-LD hợp lệ, tối thiểu 6 Q&A
- [ ] Lighthouse Performance ≥ 90 (target 95+)
- [ ] FCP dưới 1 giây

### UX:
- [ ] 1 số: hiển thị font 48px+, căn giữa, nổi bật
- [ ] Nhiều số: hiển thị dạng grid hoặc list dễ đọc, không overflow
- [ ] Mobile 375px: input fields không bị cắt, nút ≥52px, không overflow ngang
- [ ] Dark mode: toggle, persistence, không flash

### Integration:
- [ ] Navigation bar 9 links, Footer Kontakt
- [ ] Ít nhất 3 internal links trong SEO content
- [ ] 3 ad placeholders: `ad-top`, `ad-middle`, `ad-bottom`

### Deploy:
- [ ] PR merged vào `main`
- [ ] GitHub Actions deploy thành công
- [ ] URL live: `https://texthelfer.net/zufallszahlen/`
- [ ] HTTPS, 0 console errors

---

## 5. DELIVERABLES

Dev cần commit lên Git khi task hoàn thành:

- [ ] `zufallszahlen/index.html` — file tool hoàn chỉnh
- [ ] Cập nhật `index.html` (trang chủ) — thêm card Zufallszahlen Generator vào grid
- [ ] Cập nhật navigation trong TẤT CẢ các tool đã live (thêm link `/zufallszahlen/`)
- [ ] Cập nhật `sitemap.xml` — thêm URL `https://texthelfer.net/zufallszahlen/`
- [ ] Commit file brief này với status: `DONE`

> ⚠️ Đây là tool thứ 9 và cuối cùng. Sau khi deploy, Dev kiểm tra lại navigation trên TẤT CẢ 9 tool + homepage để đảm bảo link `/zufallszahlen/` đã có đủ ở mọi trang.

---

## 6. GHI CHÚ THÊM

### Nội dung FAQ tiếng Đức (6 câu, đưa vào cả accordion HTML lẫn JSON-LD):

1. **Wie funktioniert ein Zufallszahlen Generator?** → Ein Zufallszahlen Generator erstellt Zahlen, die keine erkennbare Muster oder Vorhersagbarkeit haben. Unser Generator nutzt die `crypto.getRandomValues()`-API des Browsers – einen kryptografisch sicheren Zufallsgenerator, der deutlich zuverlässiger ist als einfache mathematische Verfahren.

2. **Sind Online-Zufallszahlen wirklich zufällig?** → Unser Generator verwendet die `crypto.getRandomValues()`-API, die von modernen Browsern als kryptografisch sicher eingestuft wird. Für die meisten Anwendungen – Verlosungen, Spiele, Statistik – ist diese Qualität mehr als ausreichend.

3. **Kann ich Zufallszahlen ohne Duplikate generieren?** → Ja. Mit der Option „Keine Duplikate" stellt der Generator sicher, dass jede Zahl im Ergebnis nur einmal vorkommt. Wichtig: Die Anzahl der gewünschten Zahlen darf dabei nicht größer sein als der Bereich zwischen Minimum und Maximum.

4. **Wie kann ich Zufallszahlen als CSV exportieren?** → Klicken Sie nach der Generierung auf „Als CSV exportieren". Die Datei `zufallszahlen.csv` wird automatisch heruntergeladen. Jede Zahl steht in einer eigenen Zeile – kompatibel mit Excel, Google Sheets und allen gängigen Tabellenkalkulationen.

5. **Wie viele Zufallszahlen kann ich gleichzeitig generieren?** → Sie können bis zu 1.000 Zufallszahlen in einem Durchgang generieren. Für größere Mengen empfehlen wir, mehrere Durchgänge durchzuführen und die CSV-Exporte zusammenzuführen.

6. **Wofür kann ich den Zufallszahlen Generator nutzen?** → Der Generator eignet sich für viele Zwecke: Verlosungen und Gewinnspiele, Zufallsstichproben in der Statistik, Testdaten für Entwickler, Würfelersatz für Brettspiele, zufällige Gruppenaufteilung im Unterricht oder einfach als Entscheidungshilfe.

### Internal links gợi ý trong nội dung SEO:
- Link đến **Passwort Generator** (`/passwort-generator/`) khi đề cập đến `crypto.getRandomValues()` và bảo mật
- Link đến **Lorem Ipsum Deutsch** (`/lorem-ipsum-deutsch/`) khi đề cập đến dữ liệu ngẫu nhiên cho developer
- Link đến **Textvergleich** (`/textvergleich/`) khi đề cập đến công cụ hữu ích cho developer

### Layout wireframe:
```
┌──────────────────────────────────────────┐
│  Header + Nav                            │
├──────────────────────────────────────────┤
│  H1 + mô tả 1 câu                        │
├────────────┬────────────┬────────────────┤
│  Min       │  Max       │  Anzahl        │
│  [  1    ] │  [ 100   ] │  [    1      ] │
├────────────┴────────────┴────────────────┤
│  [ ] Keine Duplikate                     │
│  [ GENERIEREN (blau, 52px)             ] │
├──────────────────────────────────────────┤
│                                          │
│         42          ← 1 số: 48px+        │
│  (hoặc grid khi nhiều số)                │
│                                          │
│  [Kopieren]  [Als CSV exportieren]       │
├──────────────────────────────────────────┤
│  id="ad-top"                             │
├──────────────────────────────────────────┤
│  Verlauf (lịch sử 5 lần):                │
│  14:32 · 1–100 · 5 Zahlen: 42, 7, 91... │
│  14:28 · 1–10  · 1 Zahl:  6             │
│  ...                                     │
├──────────────────────────────────────────┤
│  H2: Wie funktioniert...                 │
│  id="ad-middle"                          │
│  H2: Für wen...                          │
│  id="ad-bottom"                          │
│  H2: Häufige Fragen (FAQ accordion)      │
├──────────────────────────────────────────┤
│  Footer                                  │
└──────────────────────────────────────────┘
```

---

> **Bước tiếp theo:**
> Owner paste prompt giao việc vào Claude CLI (Dev Agent):
> ```
> Bạn là Dev Agent của dự án TextHelfer.
> Đọc task brief tại: docs/tasks/TASK-009-zufallszahlen.md
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
