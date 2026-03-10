# TASK-011 — Content Expansion: Top 3 Tools

**Ngày tạo:** 2026-03-09
**PM:** TextHelfer PM Agent
**Assignee:** Dev Agent (Claude CLI)
**Priority:** P0 — cần hoàn thành trước khi apply AdSense
**Status:** TODO
**Branch:** feature/TASK-011-content-expansion
**Estimate:** 2 ngày

---

## 1. CONTEXT

Ba tool có traffic tiềm năng cao nhất (Passwort Generator, Zeichenzähler, Wörter zählen) hiện chỉ có ~150–200 từ nội dung SEO mỗi trang. Google và AdSense reviewer đánh giá nội dung editorial để xác định đây có phải "genuine content site" không — mỏng quá sẽ khó được duyệt AdSense và khó rank bền vững. Nhiệm vụ task này là mở rộng nội dung 3 trang này lên 400–600 từ và FAQ từ 6 lên 8–10 câu, đồng thời đảm bảo nội dung nghe **tự nhiên như người thật viết**, không phải AI.

---

## 2. YÊU CẦU NỘI DUNG

### Độ dài mục tiêu mỗi trang:
- Tổng nội dung SEO (H2 sections + FAQ): **400–600 từ**
- FAQ: **8–10 câu hỏi** (tăng từ 6 hiện tại)
- Mỗi câu trả lời FAQ: **50–80 từ** — đủ thực chất, không dài dòng

### 3 tool cần cập nhật:
1. `passwort-generator/index.html`
2. `zeichenzaehler/index.html`
3. `woerter-zaehlen/index.html`

---

## 3. NGUYÊN TẮC VIẾT NỘI DUNG — BẮT BUỘC

> Đây là phần quan trọng nhất của task. Dev đọc kỹ trước khi viết bất kỳ dòng nào.

### ❌ TUYỆT ĐỐI KHÔNG dùng các pattern sau:

**Dấu câu và ký tự AI hay dùng:**
- Dấu gạch dài `—` (em dash) — thay bằng dấu phẩy hoặc viết lại câu
- Dấu hai chấm `::` kiểu liệt kê máy móc
- Dấu chấm lửng `...` ở cuối câu để tạo cảm giác sâu xa
- Dấu ngoặc kép kiểu nhấn mạnh không cần thiết: „dies ist *wirklich* wichtig"

**Cụm từ AI hay mở đầu:**
- „In der heutigen Zeit..." / „In der modernen Welt..."
- „Es ist wichtig zu beachten, dass..."
- „Zusammenfassend lässt sich sagen..."
- „Nicht nur... sondern auch..."
- „Selbstverständlich..." / „Natürlich..."
- „Im Zeitalter der Digitalisierung..."
- „Heutzutage ist es unerlässlich..."

**Cấu trúc câu AI hay dùng:**
- Câu bắt đầu bằng „Dies" hoặc „Das" liên tiếp nhiều lần
- Liệt kê 3 thứ theo kiểu „erstens... zweitens... drittens..."
- Câu quá dài, nhiều mệnh đề phụ lồng nhau
- Kết thúc đoạn bằng câu tổng kết kiểu „All in all..."

---

### ✅ VIẾT THEO PHONG CÁCH NÀY:

**Giọng văn:** Thân thiện, trực tiếp, như người có kinh nghiệm giải thích cho đồng nghiệp. Không quá formal, không quá casual.

**Cấu trúc câu:** Câu ngắn đến trung bình (10–18 từ). Xen kẽ câu ngắn sau câu dài để tạo nhịp đọc tự nhiên.

**Ví dụ cụ thể:** Thay vì giải thích lý thuyết, đưa ra tình huống thực tế người dùng gặp phải.

**Tránh lặp từ:** Không dùng cùng 1 từ quá 2 lần trong 1 đoạn.

---

### So sánh trực tiếp — đúng và sai:

**❌ Sai (nghe như AI):**
> „In der heutigen digitalen Welt ist die Sicherheit von Passwörtern von entscheidender Bedeutung. Es ist wichtig zu beachten, dass ein sicheres Passwort nicht nur Groß- und Kleinbuchstaben, sondern auch Zahlen und Sonderzeichen enthalten sollte."

**✅ Đúng (nghe như người):**
> „Ein Passwort wie 'Sommer2024' knackt ein Angreifer in Sekunden. Nicht weil er es errät, sondern weil solche Muster in Wörterbüchern stehen. Ein gutes Passwort sieht dagegen aus wie Zeichensalat: zufällig, lang, ohne erkennbares Muster."

---

**❌ Sai (nghe như AI):**
> „Der Zeichenzähler ist ein nützliches Tool, das Ihnen dabei hilft, die Anzahl der Zeichen in Ihrem Text zu zählen. Dies ist besonders wichtig für Social-Media-Plattformen, die Zeichenlimits haben."

**✅ Đúng (nghe như người):**
> „Wer schon mal einen LinkedIn-Post geschrieben hat, kennt das Problem: Der Text ist fertig, aber irgendwo steht 'Zeichen überschritten'. Mit welchem Zeichen fängt man an zu kürzen? Der Zeichenzähler zeigt sofort, wie weit man noch vom Limit entfernt ist."

---

## 4. NỘI DUNG CỤ THỂ CHO TỪNG TOOL

### T-01 — Passwort Generator

**H2: „Wie sicher ist Ihr Passwort wirklich?"** (~180 từ)

Viết về: tại sao mật khẩu yếu nguy hiểm hơn người nghĩ (không cần số liệu chính xác, dùng ví dụ cụ thể). Giải thích ngắn tại sao random thực sự khác với „random của con người". Đề cập tùy chọn Phonetisch cho người cần nhớ mật khẩu. Kết nối với privacy badge: tại sao „không gửi lên server" quan trọng.

**H2: „Für wen ist der Passwort Generator geeignet?"** (4–5 bullets, ~80 từ)
- Người dùng thường cần mật khẩu mới cho tài khoản mới
- Developer cần test data với mật khẩu giả
- IT-Abteilung cần tạo hàng loạt mật khẩu tạm cho nhân viên mới
- Người hay quên mật khẩu → dùng tùy chọn Phonetisch
- Ai cũng cần sau khi có thông báo data breach

**FAQ bổ sung (thêm vào 6 câu hiện có, tổng 9–10 câu):**
- Wie lang sollte ein sicheres Passwort sein?
- Was ist der Unterschied zwischen einem sicheren und einem starken Passwort?
- Warum sollte ich nicht dasselbe Passwort für mehrere Konten verwenden?
- Was bedeutet „phonetisches Passwort"?

---

### T-02 — Zeichenzähler

**H2: „Zeichenlimits: Wo es wirklich eng wird"** (~180 từ)

Viết về: tình huống thực tế người dùng hay gặp với từng nền tảng. Không liệt kê số liệu khô khan — kể như câu chuyện. Ví dụ: Google Ads headline 30 ký tự nghe có vẻ ít nhưng thực ra đủ nếu biết cách viết. Instagram bio 150 ký tự là khoảng không gian đủ để giới thiệu bản thân hoàn chỉnh. Meta Ads 90 ký tự cho description là thách thức thực sự với content writer.

**H2: „Für wen ist der Zeichenzähler geeignet?"** (4–5 bullets, ~80 từ)
- Content Writer viết quảng cáo Google và Meta
- Social Media Manager quản lý nhiều kênh cùng lúc
- Copywriter viết email subject line (thường bị cắt ở 50 ký tự)
- SEO-Texter kiểm tra độ dài meta description
- Alle, die täglich Texte für verschiedene Plattformen schreiben

**FAQ bổ sung (thêm vào 6 câu hiện có, tổng 9–10 câu):**
- Zählt ein Emoji als ein Zeichen?
- Warum unterscheiden sich Zeichen mit und ohne Leerzeichen?
- Wie viele Zeichen hat eine durchschnittliche E-Mail-Betreffzeile?
- Zählt der Zeichenzähler auch Sonderzeichen und Umlaute?

---

### T-03 — Wörter zählen

**H2: „Wörter zählen: Mehr als nur eine Zahl"** (~180 từ)

Viết về: tại sao đếm từ quan trọng trong các tình huống cụ thể (luận văn có giới hạn từ, bài báo có word count yêu cầu, blog post SEO cần đủ độ dài). Đề cập tính năng stopwords độc đáo: tại sao biết „top từ có nghĩa" hữu ích hơn top từ tổng (ví dụ: nếu „und" xuất hiện nhiều nhất thì chẳng nói lên điều gì). Kết nối với Lesbarkeit tool để phân tích sâu hơn.

**H2: „Für wen ist der Wörterzähler geeignet?"** (4–5 bullets, ~80 từ)
- Studenten die Wortanzahl für Seminararbeiten einhalten müssen
- Blogger die Artikel auf bestimmte Länge optimieren
- Übersetzer die Preise nach Wortanzahl berechnen
- Journalisten mit vorgegebenem Umfang
- Autoren die den Fortschritt ihres Manuskripts verfolgen

**FAQ bổ sung (thêm vào 6 câu hiện có, tổng 9–10 câu):**
- Was gilt als ein Wort beim Zählen?
- Zählt ein Bindestrich-Wort wie „Fußball-WM" als ein oder zwei Wörter?
- Wie viele Wörter hat eine DIN-A4-Seite?
- Was sind Stoppwörter und warum werden sie herausgefiltert?

---

## 5. TECH NOTES

- Chỉ thay đổi **nội dung text** trong các H2 section và FAQ — không được động vào tool UI, JavaScript, CSS
- FAQ mới phải được thêm vào cả **HTML accordion** lẫn **FAQPage JSON-LD** trong `<head>`
- Giữ nguyên tất cả id, class, ad placeholders, internal links hiện có
- Không thêm external request nào
- Kiểm tra lại keyword density sau khi viết — keyword chính xuất hiện 3–5 lần tự nhiên trong toàn trang, không nhồi nhét

---

## 6. ACCEPTANCE CRITERIA

```
[ ] passwort-generator: tổng nội dung SEO 400–600 từ
    Số từ thực tế: ___________

[ ] zeichenzaehler: tổng nội dung SEO 400–600 từ
    Số từ thực tế: ___________

[ ] woerter-zaehlen: tổng nội dung SEO 400–600 từ
    Số từ thực tế: ___________

[ ] Mỗi trang có 8–10 FAQ (accordion + JSON-LD đồng bộ)
    Số FAQ thực tế: ___________

[ ] Không có cụm từ bị cấm trong danh sách mục 3
    Kết quả kiểm tra: ___________

[ ] Nội dung đọc tự nhiên — không nghe như AI
    Dev tự đánh giá + paste 1 đoạn mẫu để PM review

[ ] Tool UI và JavaScript không bị ảnh hưởng — tất cả chức năng vẫn hoạt động
    Kết quả thực tế: ___________

[ ] FAQ JSON-LD trong <head> đồng bộ với FAQ accordion trong body
    Kết quả thực tế: ___________

[ ] Lighthouse Performance vẫn ≥ 90 sau khi thêm nội dung
    Score: ___________

[ ] 0 console errors
    Kết quả thực tế: ___________
```

---

## 7. DELIVERABLES

- [ ] `passwort-generator/index.html` — cập nhật nội dung
- [ ] `zeichenzaehler/index.html` — cập nhật nội dung
- [ ] `woerter-zaehlen/index.html` — cập nhật nội dung
- [ ] Commit file brief này với status: `DONE`

---

> **Bước tiếp theo:**
> Owner paste prompt giao việc vào Claude CLI (Dev Agent):
> ```
> Bạn là Dev Agent của dự án TextHelfer.
> Đọc task brief tại: docs/tasks/TASK-011-content-expansion.md
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
