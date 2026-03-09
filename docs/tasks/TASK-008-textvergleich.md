# TASK-008 — Textvergleich

**Ngày tạo:** 2026-03-09
**PM:** TextHelfer PM Agent
**Assignee:** Dev Agent (Claude CLI)
**Priority:** P2
**Status:** TODO
**Branch:** feature/TASK-008-textvergleich
**Estimate:** 2 ngày

---

## 1. CONTEXT

Sinh viên, editor, và developer tiếng Đức cần so sánh hai phiên bản văn bản để tìm điểm khác nhau — ví dụ kiểm tra bản nháp luận văn, so sánh phiên bản hợp đồng, hay diff nội dung website. Các tool tiếng Anh (như diffchecker.com) đang xếp hạng trên google.de nhưng không có giao diện tiếng Đức. Không có đối thủ tiếng Đức chuyên biệt nào chất lượng. Tool này có layout phức tạp hơn các tool trước (2 textarea cạnh nhau + highlight diff) nên cần phân bổ 2 ngày.

---

## 2. REQUIREMENTS

### Chức năng bắt buộc:
- [ ] Hai textarea đặt cạnh nhau (desktop ≥768px), xếp dọc trên mobile — textarea trái là „Text 1", textarea phải là „Text 2"
- [ ] Nút **„Vergleichen"** để trigger so sánh (không real-time — người dùng bấm chủ động vì diff phức tạp)
- [ ] Kết quả hiển thị ngay bên dưới 2 textarea: highlight trực tiếp trên text, không mở trang mới
- [ ] **Hai chế độ so sánh** (radio button hoặc toggle):
  - **Wort-für-Wort**: so sánh từng từ, highlight từng từ thêm/xóa
  - **Zeile-für-Zeile**: so sánh từng dòng, highlight cả dòng thêm/xóa
- [ ] Màu highlight: **xanh lá** (`#BBF7D0`) = nội dung có trong Text 2 nhưng không có Text 1 (thêm vào) — **đỏ** (`#FECACA`) = nội dung có trong Text 1 nhưng không có Text 2 (bị xóa)
- [ ] Thanh thống kê sau khi so sánh: „X Zeilen geändert · Y Wörter hinzugefügt · Z Wörter entfernt"
- [ ] Nút **„Tauschen"**: hoán đổi nội dung Text 1 ↔ Text 2, tự động re-run so sánh nếu đã có kết quả
- [ ] Nút **„Zurücksetzen"**: xóa cả 2 textarea và kết quả, có confirm khi tổng ký tự >100
- [ ] Nút **„Kopieren"** riêng cho từng textarea, toast „Kopiert!" 2 giây
- [ ] Khi chưa có text hoặc 2 textarea giống hệt nhau: hiển thị thông báo „Kein Unterschied gefunden" thay vì kết quả trống

### Chức năng không được phá vỡ:
- [ ] Highlight không làm vỡ layout khi text dài (overflow: auto trên kết quả)
- [ ] Chế độ mobile: 2 textarea xếp dọc, kết quả cũng xếp dọc, không overflow ngang
- [ ] Dark mode: màu highlight phải điều chỉnh cho dark background — dùng `#166534` (xanh đậm) và `#991B1B` (đỏ đậm) thay vì màu pastel
- [ ] SEO content và FAQ accordion không bị xóa

---

## 3. TECH NOTES

> Các ràng buộc kỹ thuật bất biến — Dev KHÔNG được bỏ qua:

- `<html lang="de">` trên thẻ html — bắt buộc
- Font: `system-ui` — KHÔNG dùng Google Fonts
- Toàn bộ CSS + JS inline trong 1 file HTML duy nhất
- KHÔNG dùng framework (React, Vue...) hoặc external JS library
- Thuật toán diff tự implement — không dùng thư viện diff.js hay tương tự
- Dark mode: script đọc `localStorage.getItem("texthelfer-theme")` đặt trong `<head>`, KHÔNG ở cuối body
- Navigation header: link đến tất cả 9 tool + footer link Kontakt
- 3 Ad placeholder: `id="ad-top"` (sau nút hành động), `id="ad-middle"` (sau H2 đầu), `id="ad-bottom"` (trước FAQ)
- Màu sắc:
  - Light: bg `#FFFFFF`, text `#111827`, accent `#2563EB`, border `#E2E8F0`
  - Dark: bg `#0F172A`, text `#F1F5F9`, accent `#60A5FA`, border `#334155`
- localStorage key cho theme: `"texthelfer-theme"`

### Thuật toán diff gợi ý (không cần thư viện):

**Zeile-für-Zeile:** Split cả 2 text bằng `\n`, so sánh từng phần tử mảng. Dòng chỉ có ở mảng 1 → đỏ, dòng chỉ có ở mảng 2 → xanh, dòng giống nhau → không màu.

**Wort-für-Wort:** Split cả 2 text bằng `/\s+/`, so sánh từng từ. Dùng thuật toán LCS (Longest Common Subsequence) đơn giản hoặc two-pointer để xác định từ thêm/xóa. Render kết quả bằng `<span>` với class màu, nối lại bằng khoảng trắng.

> ⚠️ Lưu ý hiệu năng: với text rất dài (>5.000 từ), LCS có thể chậm. Dev có thể giới hạn Wort-für-Wort ở 2.000 từ/textarea và hiển thị cảnh báo nếu vượt quá: *„Für sehr lange Texte empfehlen wir den Zeile-für-Zeile-Modus."*

### SEO bắt buộc:
- **Title:** `Text Vergleichen Online Kostenlos – Textvergleich | TextHelfer` (58 ký tự)
- **Meta description:** `Zwei Texte online vergleichen und Unterschiede finden. Kostenloser Textvergleich mit Wort- und Zeilenvergleich – direkt im Browser, keine Anmeldung nötig.` (155 ký tự)
- **Canonical:** `<link rel="canonical" href="https://texthelfer.net/textvergleich/">`
- **H1:** `Textvergleich – Zwei Texte online vergleichen`
- WebApplication JSON-LD schema (price: "0", priceCurrency: "EUR")
- FAQPage JSON-LD schema (tối thiểu 6 Q&A tiếng Đức — xem mục 6)
- Nội dung SEO: H2 „Wie funktioniert der Textvergleich?" (~150 từ) + H2 „Für wen ist der Textvergleich geeignet?" (4–5 bullets) + H2 „Häufige Fragen" (6–8 FAQ accordion)

---

## 4. ACCEPTANCE CRITERIA

> PM sẽ dùng danh sách này ở Bước 6 để ký duyệt.
> Dev dùng danh sách này ở Bước 5 để tự test và báo cáo.

### Functional:
- [ ] Nhập 2 đoạn text khác nhau → bấm „Vergleichen" → highlight đúng màu (xanh/đỏ)
- [ ] Chế độ Wort-für-Wort: highlight từng từ khác nhau
- [ ] Chế độ Zeile-für-Zeile: highlight cả dòng khác nhau
- [ ] Thanh thống kê hiển thị đúng số dòng/từ thay đổi
- [ ] Nút „Tauschen" hoán đổi nội dung 2 textarea, re-run so sánh nếu đã có kết quả
- [ ] Nút „Zurücksetzen" xóa tất cả, confirm khi >100 ký tự
- [ ] Nút „Kopieren" mỗi bên hoạt động, toast „Kopiert!" 2 giây
- [ ] 2 textarea giống hệt nhau → hiển thị „Kein Unterschied gefunden"
- [ ] 1 hoặc 2 textarea trống → không crash, hiển thị thông báo phù hợp

### SEO & Technical:
- [ ] `<html lang="de">` có trong source
- [ ] Title tag: `Text Vergleichen Online Kostenlos – Textvergleich | TextHelfer` — 50–60 ký tự
- [ ] 1 thẻ H1 duy nhất: `Textvergleich – Zwei Texte online vergleichen`
- [ ] Canonical URL: `https://texthelfer.net/textvergleich/`
- [ ] WebApplication JSON-LD hợp lệ
- [ ] FAQPage JSON-LD hợp lệ, tối thiểu 6 Q&A
- [ ] Lighthouse Performance ≥ 90 (target 95+)
- [ ] FCP dưới 1 giây

### UX:
- [ ] Desktop: 2 textarea cạnh nhau, kết quả bên dưới song song
- [ ] Mobile 375px: 2 textarea xếp dọc, không overflow ngang
- [ ] Highlight màu đúng ở cả Light mode và Dark mode
- [ ] Tất cả nút ≥52px chiều cao trên mobile
- [ ] Dark mode toggle hoạt động, không flash khi reload
- [ ] Preference dark mode giữ nguyên sau khi đóng/mở lại trình duyệt

### Integration:
- [ ] Navigation bar hiển thị link đến cả 9 tool
- [ ] Footer có link Kontakt
- [ ] Ít nhất 3 internal link ngữ cảnh trong nội dung SEO
- [ ] 3 div ad placeholder: `id="ad-top"`, `id="ad-middle"`, `id="ad-bottom"`

### Deploy:
- [ ] PR đã merge vào `main`
- [ ] GitHub Actions deploy thành công
- [ ] URL live: `https://texthelfer.net/textvergleich/`
- [ ] HTTPS hoạt động, không có mixed content warning

---

## 5. DELIVERABLES

Dev cần commit lên Git khi task hoàn thành:

- [ ] `textvergleich/index.html` — file tool hoàn chỉnh
- [ ] Cập nhật `index.html` (trang chủ) — thêm card Textvergleich vào grid
- [ ] Cập nhật navigation trong TẤT CẢ các tool đã live (thêm link `/textvergleich/`)
- [ ] Cập nhật `sitemap.xml` — thêm URL `https://texthelfer.net/textvergleich/`
- [ ] Commit file brief này với status: `DONE`

---

## 6. GHI CHÚ THÊM

### Nội dung FAQ tiếng Đức (6 câu, đưa vào cả accordion HTML lẫn JSON-LD):

1. **Wie vergleiche ich zwei Texte online?** → Mit dem TextHelfer Textvergleich fügen Sie einfach beide Texte in die jeweiligen Felder ein und klicken auf „Vergleichen". Unterschiede werden sofort farblich hervorgehoben: Grün zeigt hinzugefügte Inhalte, Rot zeigt entfernte Inhalte.

2. **Was ist der Unterschied zwischen Wort- und Zeilenvergleich?** → Der Wort-für-Wort-Vergleich hebt einzelne geänderte Wörter hervor und eignet sich für kurze Texte oder Satzänderungen. Der Zeile-für-Zeile-Vergleich markiert ganze Zeilen und ist besser geeignet für strukturierte Texte wie Listen, Code oder Verträge.

3. **Werden meine Texte gespeichert oder übertragen?** → Nein. Der gesamte Vergleich findet direkt in Ihrem Browser statt. Kein Text wird an einen Server gesendet oder gespeichert. Ihre Daten bleiben vollständig auf Ihrem Gerät.

4. **Kann ich den Textvergleich für Verträge oder vertrauliche Dokumente nutzen?** → Ja, da keine Daten übertragen werden, ist der Textvergleich auch für vertrauliche Inhalte geeignet. Es empfiehlt sich trotzdem, bei sehr sensiblen Dokumenten auf lokale Offline-Tools zurückzugreifen.

5. **Gibt es eine kostenlose Alternative zu Word-Vergleichsfunktion?** → Ja. Der TextHelfer Textvergleich bietet eine kostenlose, browserbasierte Alternative zur Vergleichsfunktion in Microsoft Word oder Google Docs – ohne Anmeldung und ohne Software-Installation.

6. **Wie viel Text kann ich gleichzeitig vergleichen?** → Für den Zeile-für-Zeile-Modus gibt es keine praktische Begrenzung. Beim Wort-für-Wort-Vergleich empfehlen wir bis zu 2.000 Wörter pro Textfeld für optimale Geschwindigkeit.

### Internal links gợi ý in nội dung SEO:
- Link đến **Wörter zählen** (`/woerter-zaehlen/`) khi đề cập đến phân tích văn bản
- Link đến **Zeichenzähler** (`/zeichenzaehler/`) khi đề cập đến độ dài văn bản
- Link đến **Text Formatierung** (`/text-formatierung/`) khi đề cập đến chỉnh sửa văn bản

### Layout wireframe:
```
┌──────────────────────────────────────────────────┐
│  Header + Nav                                    │
├──────────────────────────────────────────────────┤
│  H1 + mô tả 1 câu                                │
│  [○ Wort-für-Wort  ○ Zeile-für-Zeile]  (toggle)  │
├─────────────────────┬────────────────────────────┤
│  Text 1             │  Text 2                    │
│  [Textarea        ] │  [Textarea               ] │
│  [Kopieren]         │  [Kopieren]                │
├─────────────────────┴────────────────────────────┤
│  [← Tauschen]  [Vergleichen →]  [Zurücksetzen]   │
│  „3 Zeilen geändert · 5 Wörter hinzugefügt..."   │
├──────────────────────────────────────────────────┤
│  id="ad-top"                                     │
├──────────────────────────────────────────────────┤
│  ERGEBNIS:                                       │
│  [Text mit grün/rot Highlights]                  │
├──────────────────────────────────────────────────┤
│  H2: Wie funktioniert...                         │
│  id="ad-middle"                                  │
│  H2: Für wen...                                  │
│  id="ad-bottom"                                  │
│  H2: Häufige Fragen (FAQ accordion)              │
├──────────────────────────────────────────────────┤
│  Footer                                          │
└──────────────────────────────────────────────────┘
```

---

> **Bước tiếp theo:**
> Owner paste prompt giao việc vào Claude CLI (Dev Agent) khi TASK-007 hoàn thành:
> ```
> Bạn là Dev Agent của dự án TextHelfer.
> Đọc task brief tại: docs/tasks/TASK-008-textvergleich.md
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
