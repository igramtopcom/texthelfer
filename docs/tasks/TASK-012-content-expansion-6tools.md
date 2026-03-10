# TASK-012 — Content Expansion: 6 Tools còn lại

**Ngày tạo:** 2026-03-09
**PM:** TextHelfer PM Agent
**Assignee:** Dev Agent (Claude CLI)
**Priority:** P1
**Status:** DONE ✅
**Branch:** feature/TASK-012-content-expansion-6tools
**Estimate:** 3 ngày

---

## 1. CONTEXT

TASK-011 đã mở rộng nội dung thành công cho 3 tool P0 (Passwort Generator, Zeichenzähler, Wörter zählen). Task này áp dụng cùng phương pháp cho 6 tool còn lại. Mục tiêu: toàn bộ 9 tool có độ sâu nội dung đồng đều, tăng authority tổng thể của site và cải thiện xếp hạng các keyword dài hạn.

Dev đã quen với phong cách viết từ TASK-011 — áp dụng y chang, không thay đổi.

---

## 2. 6 TOOLS CẦN CẬP NHẬT

1. `lesbarkeit/index.html`
2. `lorem-ipsum-deutsch/index.html`
3. `text-formatierung/index.html`
4. `umlaut-konverter/index.html`
5. `textvergleich/index.html`
6. `zufallszahlen/index.html`

---

## 3. NGUYÊN TẮC VIẾT — GIỐNG HỆT TASK-011, BẮT BUỘC

> Dev đã thực hiện tốt ở TASK-011. Áp dụng y chang cho task này.

### ❌ TUYỆT ĐỐI KHÔNG:
- Dấu gạch dài `—` (em dash)
- „In der heutigen Zeit...", „Es ist wichtig zu beachten...", „Zusammenfassend..."
- „Nicht nur... sondern auch...", „Im Zeitalter der Digitalisierung..."
- Câu tổng kết cuối đoạn kiểu AI
- Liệt kê 3 thứ theo kiểu „erstens... zweitens... drittens..."

### ✅ VIẾT THEO PHONG CÁCH:
- Câu ngắn đến trung bình (10–18 từ), xen kẽ nhịp ngắn-dài
- Mở đầu bằng tình huống thực tế người dùng hay gặp
- Ví dụ cụ thể thay vì giải thích lý thuyết
- Giọng thân thiện, trực tiếp — như người có kinh nghiệm giải thích cho đồng nghiệp

### Ví dụ chuẩn từ TASK-011 (Dev dùng làm tham chiếu):
> „Wer schon mal einen LinkedIn-Post geschrieben hat, kennt das Problem: Der Text steht, aber die Plattform meldet 'Zeichen überschritten'. Wo fängt man an zu kürzen?"

---

## 4. MỤC TIÊU ĐỘ DÀI

- Nội dung **mới thêm** mỗi trang: **400–500 từ**
- FAQ: tăng từ 6 lên **8–10 câu**
- Mỗi câu trả lời FAQ: **50–80 từ**

---

## 5. NỘI DUNG CỤ THỂ TỪNG TOOL

---

### T-04 — Lesbarkeit prüfen

**H2 mới: „Wann ist ein Text schwer zu lesen?"** (~180 từ)

Viết về: tình huống thực tế người viết hay mắc lỗi độ khó (văn bản hành chính Đức nổi tiếng phức tạp, luận văn sinh viên hay quá hàn lâm, email marketing dài dòng). Giải thích ngắn sự khác biệt giữa Flesch và Wiener Sachtextformel — tại sao cần cả hai thay vì chỉ một. Đề cập gợi ý cụ thể tool đưa ra (câu quá dài, nên rút ngắn).

**H2: „Für wen ist die Lesbarkeitsanalyse geeignet?"** — viết lại tự nhiên hơn (4–5 bullets, ~80 từ):
- Texter và content writer muốn kiểm tra bài viết trước khi publish
- Sinh viên viết luận văn cần đảm bảo giám khảo đọc được dễ dàng
- Unternehmen viết tài liệu hướng dẫn cho khách hàng
- Journalisten muốn bài báo tiếp cận được nhiều đối tượng
- Alle, die schreiben und verstanden werden wollen

**FAQ bổ sung (4 câu mới, tổng 10):**
- Was ist ein guter Flesch-Wert für deutschen Text?
- Was ist der Unterschied zwischen Flesch und Wiener Sachtextformel?
- Wie messe ich die Lesbarkeit eines langen Dokuments?
- Welche Satzlänge gilt als optimal für gute Lesbarkeit?

---

### T-05 — Lorem Ipsum Deutsch

**H2 mới: „Warum deutsches Lorem Ipsum besser ist"** (~180 từ)

Viết về: tình huống designer/developer dùng Latin Lorem Ipsum cho mockup rồi khách hàng Đức không hiểu bố cục sẽ trông như thế nào với text thật. Ví dụ cụ thể: chữ Đức dài hơn chữ Anh trung bình 30% (compound words) nên mockup bằng tiếng Anh hay Latin không phản ánh đúng layout thực tế. Khi dùng placeholder tiếng Đức thật, buổi review với stakeholder hiệu quả hơn.

**H2: „Für wen ist der Deutsche Lorem Ipsum Generator geeignet?"** (4–5 bullets, ~80 từ):
- UI/UX Designer tạo wireframe và prototype
- Webentwickler cần test layout trước khi có content thật
- Grafikdesigner layout sách, brochure, tạp chí tiếng Đức
- Agenturen presentieren mockup cho khách hàng Đức
- Alle, die Platzhaltertext für deutschsprachige Projekte brauchen

**FAQ bổ sung (4 câu mới, tổng 10):**
- Warum heißt es Lorem Ipsum?
- Was ist der Unterschied zwischen Lorem Ipsum und echtem Blindtext?
- Kann ich den deutschen Blindtext für kommerzielle Projekte verwenden?
- Warum ist deutscher Platzhaltertext besser als englischer für DACH-Projekte?

---

### T-06 — Text Formatierung

**H2 mới: „Wann braucht man eine Textformatierung?"** (~180 từ)

Viết về: tình huống copy-paste từ Word/PDF vào CMS hay email thường kéo theo định dạng lộn xộn (ALL CAPS từ tiêu đề, khoảng trắng thừa, xuống dòng không đúng chỗ). Đề cập tính năng độc đáo Substantive großschreiben — công cụ duy nhất làm được việc này tự động với 3.794 danh từ tiếng Đức. Ví dụ: khi nhận bản thảo từ tác giả gõ trên điện thoại không có auto-capitalize.

**H2: „Für wen ist die Text Formatierung geeignet?"** (4–5 bullets, ~80 từ):
- Content Manager copy-paste nội dung vào CMS
- Lektor và editor cần chuẩn hóa bản thảo nhanh
- Sekretärin xử lý văn bản từ nhiều nguồn khác nhau
- Developer cần normalize text data trước khi xử lý
- Alle, die mit unformatiertem Text aus verschiedenen Quellen arbeiten

**FAQ bổ sung (4 câu mới, tổng 10):**
- Was ist Title Case auf Deutsch?
- Wie funktioniert das automatische Großschreiben von Substantiven?
- Was macht die Funktion „Text bereinigen" genau?
- Kann ich den formatierten Text direkt in Word oder Google Docs einfügen?

---

### T-07 — Umlaut Konverter

**H2 mới: „Wann braucht man die Umlautumschreibung?"** (~180 từ)

Viết về: tình huống cụ thể người dùng hay gặp — gõ tên tiếng Đức trên bàn phím quốc tế (ví dụ: đang dùng MacBook với keyboard layout Anh, cần viết email với tên khách hàng „Müller"), điền form online không nhận ký tự đặc biệt, địa chỉ email không thể có umlaut, URL slug cần dạng ASCII. Đề cập hướng ngược: khi nhận văn bản dạng ae/oe/ue cần convert lại thành umlaut thật để đăng lên web.

**H2: „Für wen ist der Umlaut Konverter geeignet?"** (4–5 bullets, ~80 từ):
- Alle mit internationaler Tastatur die auf Deutsch schreiben müssen
- Webentwickler xử lý URL slug và permalink tiếng Đức
- Người điền form online hay database không nhận ký tự đặc biệt
- E-Mail-Nutzer tạo địa chỉ email từ tên tiếng Đức
- Alle die deutschen Text für internationale Systeme aufbereiten

**FAQ bổ sung (4 câu mới, tổng 10):**
- Wie schreibt man einen deutschen Namen ohne Umlaut?
- Warum akzeptieren manche Systeme keine Umlaute?
- Ist „Mueller" oder „Müller" die offizielle Umschreibung?
- Funktioniert die Umschreibung auch für Österreich und die Schweiz?

---

### T-08 — Textvergleich

**H2 mới: „Wann ist ein Textvergleich sinnvoll?"** (~180 từ)

Viết về: tình huống thực tế — nhận 2 phiên bản hợp đồng từ luật sư và không biết điểm nào đã thay đổi, bản thảo luận văn sau khi giáo viên hướng dẫn sửa và muốn xem thay đổi ở đâu, bài blog đã publish và bản update mới. Đề cập sự khác nhau giữa 2 chế độ: Wort-für-Wort tốt cho văn xuôi, Zeile-für-Zeile tốt cho code, danh sách, hợp đồng có cấu trúc dòng rõ ràng.

**H2: „Für wen ist der Textvergleich geeignet?"** (4–5 bullets, ~80 từ):
- Studenten kiểm tra bản thảo sau khi chỉnh sửa theo góp ý
- Juristen và Sachbearbeiter so sánh phiên bản hợp đồng
- Redakteure so sánh bản gốc và bản đã biên tập
- Entwickler diff nội dung text file không có Git
- Alle, die zwei Versionen desselben Textes vergleichen wollen

**FAQ bổ sung (4 câu mới, tổng 10):**
- Was ist der Unterschied zwischen Wort- und Zeilenvergleich?
- Kann ich den Textvergleich für Verträge nutzen?
- Wie werden die Unterschiede farblich dargestellt?
- Gibt es eine Begrenzung für die Textlänge beim Vergleich?

---

### T-09 — Zufallszahlen Generator

**H2 mới: „Wofür braucht man Zufallszahlen?"** (~180 từ)

Viết về: các tình huống thực tế đa dạng — giáo viên quay số học sinh trả bài (công bằng hơn là chọn tay), tổ chức bốc thăm trúng thưởng minh bạch, developer cần seed data cho database test, người làm thống kê cần random sample. Đề cập tính năng xuất CSV độc đáo — các tool khác không có, hữu ích cho giáo viên và người làm data. Giải thích ngắn tại sao `crypto.getRandomValues()` đáng tin hơn `Math.random()` — không cần đi sâu kỹ thuật, chỉ cần người dùng hiểu là "thực sự ngẫu nhiên".

**H2: „Für wen ist der Zufallszahlen Generator geeignet?"** (4–5 bullets, ~80 từ):
- Lehrer quay số học sinh hoặc phân nhóm ngẫu nhiên
- Veranstalter tổ chức bốc thăm minh bạch
- Entwickler và Tester cần random test data
- Statistiker cần random sample từ tập dữ liệu lớn
- Alle, die eine faire und nachvollziehbare Zufallsentscheidung brauchen

**FAQ bổ sung (4 câu mới, tổng 10):**
- Wie exportiere ich Zufallszahlen als CSV?
- Was ist der Unterschied zwischen Math.random() und crypto.getRandomValues()?
- Kann ich Zufallszahlen mit Dezimalstellen generieren?
- Wie funktioniert die „Keine Duplikate"-Option?

---

## 6. TECH NOTES

- Chỉ thay đổi **nội dung text** trong H2 sections và FAQ — không được động vào tool UI, JavaScript, CSS
- FAQ mới phải thêm vào cả **HTML accordion** lẫn **FAQPage JSON-LD** trong `<head>` — phải đồng bộ
- Giữ nguyên tất cả id, class, ad placeholders, internal links hiện có
- Keyword chính xuất hiện 3–5 lần tự nhiên trong toàn trang, không nhồi nhét
- Không thêm external request nào

---

## 7. ACCEPTANCE CRITERIA

```
[ ] lesbarkeit: ~400–500 từ mới thêm, tổng 8–10 FAQ
    Từ mới / FAQ thực tế: ___________

[ ] lorem-ipsum-deutsch: ~400–500 từ mới thêm, tổng 8–10 FAQ
    Từ mới / FAQ thực tế: ___________

[ ] text-formatierung: ~400–500 từ mới thêm, tổng 8–10 FAQ
    Từ mới / FAQ thực tế: ___________

[ ] umlaut-konverter: ~400–500 từ mới thêm, tổng 8–10 FAQ
    Từ mới / FAQ thực tế: ___________

[ ] textvergleich: ~400–500 từ mới thêm, tổng 8–10 FAQ
    Từ mới / FAQ thực tế: ___________

[ ] zufallszahlen: ~400–500 từ mới thêm, tổng 8–10 FAQ
    Từ mới / FAQ thực tế: ___________

[ ] Không có em dash, không có cụm mở đầu AI bị cấm trong cả 6 tool
    Kết quả: ___________

[ ] Dev paste 1 đoạn mẫu từ ít nhất 2 tool để PM review giọng văn
    Đoạn mẫu: ___________

[ ] FAQ JSON-LD trong <head> đồng bộ với FAQ accordion trong body — cả 6 tool
    Kết quả: ___________

[ ] Tool UI và JavaScript không bị ảnh hưởng — tất cả chức năng vẫn hoạt động
    Kết quả: ___________

[ ] Lighthouse Performance ≥ 90 trên ít nhất 2 tool sau khi update
    Score: ___________

[ ] 0 console errors trên cả 6 tool
    Kết quả: ___________
```

---

## 8. DELIVERABLES

- [ ] `lesbarkeit/index.html` — cập nhật nội dung
- [ ] `lorem-ipsum-deutsch/index.html` — cập nhật nội dung
- [ ] `text-formatierung/index.html` — cập nhật nội dung
- [ ] `umlaut-konverter/index.html` — cập nhật nội dung
- [ ] `textvergleich/index.html` — cập nhật nội dung
- [ ] `zufallszahlen/index.html` — cập nhật nội dung
- [ ] Commit file brief này với status: `DONE`

---

> **Bước tiếp theo:**
> Owner paste prompt giao việc vào Claude CLI (Dev Agent):
> ```
> Bạn là Dev Agent của dự án TextHelfer.
> Đọc task brief tại: docs/tasks/TASK-012-content-expansion-6tools.md
> Sau khi đọc xong:
> 1. Tóm tắt lại những gì bạn hiểu về task này trong 3–5 câu
> 2. Liệt kê những gì bạn sẽ làm (không code ngay)
> 3. Hỏi nếu có gì chưa rõ
> Chờ xác nhận từ Owner trước khi bắt đầu code.
> ```

---

**PM Sign-off:** [x] ĐÃ KÝ DUYỆT ✅
**Ngày ký duyệt:** 2026-03-09
**Ghi chú ký duyệt:** Chất lượng nội dung đồng đều trên cả 7 file — không có tool nào bị padding hay giọng văn khác biệt. Homepage viết lại hoàn toàn tốt hơn bản cũ, xứng đáng là entry point của site.
