# TASK-005 — Lorem Ipsum Deutsch

**Ngày tạo:** 2026-03-09
**PM:** TextHelfer PM Agent
**Assignee:** Dev Agent (Claude CLI)
**Priority:** P1
**Status:** TODO
**Branch:** feature/TASK-005-lorem-ipsum-deutsch
**Estimate:** 1 ngày

---

## 1. CONTEXT

Lorem Ipsum Deutsch tạo placeholder text tiếng Đức thực sự thay vì Latin Lorem Ipsum. Dành cho designer và developer Đức muốn stakeholder đọc được mockup. Volume 4.000–8.000/tháng, KD chỉ 5–12 — gần như không có đối thủ chuyên biệt chất lượng. Điểm khác biệt: corpus ~500 câu tiếng Đức tự nhiên, viết tay, không phải machine-translated. Đây là tool đơn giản nhất trong 9 tool — logic chỉ là random + ghép câu. Dev nên hoàn thành trong 1 ngày.

---

## 2. REQUIREMENTS

### Chức năng bắt buộc:
- [ ] Chọn **số đoạn văn**: slider hoặc input số, range 1–10, mặc định 3
- [ ] Chọn **độ dài mỗi đoạn** (radio button 3 mức):
  - Kurz (~3–4 câu, ~50 từ)
  - Mittel (~6–7 câu, ~100 từ)
  - Lang (~10–12 câu, ~200 từ)
- [ ] Toggle **HTML-Tags**: bật → bọc mỗi đoạn bằng `<p>`, thêm `<h2>` trước đoạn đầu tiên
- [ ] **Preview mode**: tab "Vorschau" xem text được render như HTML thật (không hiện tags)
- [ ] **Raw mode**: tab "Quelltext" xem text thuần hoặc với HTML tags
- [ ] Nút **Neu generieren** — tạo lại bộ câu ngẫu nhiên mới
- [ ] Nút **Alles kopieren** — copy toàn bộ nội dung (raw text hoặc HTML tùy tab đang chọn), toast "Kopiert!" 2 giây
- [ ] Tự động generate khi load trang (không để trống)
- [ ] Dark mode toggle

### Chức năng không được phá vỡ:
- [ ] Thay đổi số đoạn hoặc độ dài → **không tự động re-generate** (giữ nguyên text hiện tại, chỉ generate lại khi bấm nút "Neu generieren")
- [ ] Bật/tắt HTML-Tags → cập nhật ngay nội dung hiển thị mà không re-generate câu

---

## 3A. TECH NOTES

### Corpus tiếng Đức — 60 câu mẫu (hardcoded trong JS):

Dev dùng đúng danh sách câu dưới đây. Đây là câu tiếng Đức tự nhiên, trung tính về chủ đề, phù hợp làm placeholder:

```javascript
const CORPUS = [
  "Die moderne Gestaltung verbindet Funktionalität mit ästhetischer Schönheit.",
  "Jedes Detail wurde sorgfältig ausgewählt, um ein harmonisches Gesamtbild zu schaffen.",
  "Die Benutzeroberfläche wurde intuitiv gestaltet, damit alle Nutzer sie sofort verstehen.",
  "Durch klare Strukturen wird die Navigation erheblich vereinfacht.",
  "Das Design spricht sowohl junge als auch erfahrene Nutzer gleichermaßen an.",
  "Farben und Formen wurden aufeinander abgestimmt, um eine angenehme Atmosphäre zu erzeugen.",
  "Die Typografie spielt eine entscheidende Rolle für die Lesbarkeit des Inhalts.",
  "Jede Seite erzählt ihre eigene Geschichte durch sorgfältig platzierte Elemente.",
  "Das Layout passt sich flexibel an verschiedene Bildschirmgrößen an.",
  "Benutzerfreundlichkeit steht im Mittelpunkt aller Designentscheidungen.",
  "Die Farbpalette wurde bewusst gewählt, um Vertrauen und Seriosität auszustrahlen.",
  "Moderne Webtechnologien ermöglichen ein reibungsloses Nutzererlebnis auf allen Geräten.",
  "Der erste Eindruck entscheidet darüber, ob ein Besucher bleibt oder die Seite verlässt.",
  "Konsistenz in der Gestaltung schafft Vertrauen und Wiedererkennungswert.",
  "Gutes Design löst Probleme, ohne dass der Nutzer es bewusst wahrnimmt.",
  "Die Balance zwischen Text und Bildern sorgt für ein ausgewogenes Erscheinungsbild.",
  "Weißraum ist kein leerer Raum, sondern ein aktives Gestaltungsmittel.",
  "Jedes Element hat seinen Platz und seinen Zweck im Gesamtkonzept.",
  "Die Navigation führt den Nutzer intuitiv durch alle Bereiche der Anwendung.",
  "Schnelle Ladezeiten sind ein wesentlicher Bestandteil einer guten Nutzererfahrung.",
  "Das Produkt überzeugt durch seine klare und durchdachte Struktur.",
  "Innovation und Tradition verbinden sich zu einem einzigartigen Ergebnis.",
  "Die Zusammenarbeit verschiedener Disziplinen führt zu besseren Lösungen.",
  "Nachhaltigkeit und Effizienz sind die Grundpfeiler des modernen Designs.",
  "Kreativität entfaltet sich am besten innerhalb klar definierter Grenzen.",
  "Das Projekt wurde von Anfang an mit dem Nutzer im Mittelpunkt entwickelt.",
  "Technologie sollte dem Menschen dienen, nicht umgekehrt.",
  "Die einfachsten Lösungen sind oft die elegantesten und wirkungsvollsten.",
  "Qualität zeigt sich in den Details, die auf den ersten Blick unsichtbar sind.",
  "Ein durchdachtes Konzept bildet die Grundlage für jedes erfolgreiche Projekt.",
  "Die Kombination aus Form und Funktion schafft bleibende Eindrücke.",
  "Nutzerforschung liefert wertvolle Einblicke für eine zielgerichtete Gestaltung.",
  "Prototypen helfen dabei, Ideen schnell zu testen und zu verfeinern.",
  "Feedback von echten Nutzern ist unbezahlbar für die Produktentwicklung.",
  "Iterative Prozesse führen zu stetig verbesserten Ergebnissen.",
  "Die visuelle Hierarchie lenkt den Blick des Betrachters gezielt durch den Inhalt.",
  "Barrierefreiheit ist kein Kompromiss, sondern eine Bereicherung für alle.",
  "Responsive Design stellt sicher, dass Inhalte auf jedem Gerät optimal dargestellt werden.",
  "Die Markenidentität spiegelt sich in jedem visuellen Element wider.",
  "Storytelling durch Design schafft emotionale Verbindungen mit dem Publikum.",
  "Das Zusammenspiel von Inhalt und Form entscheidet über den Erfolg einer Seite.",
  "Durchdachte Mikrointeraktionen verbessern das Nutzererlebnis spürbar.",
  "Eine starke visuelle Sprache kommuniziert Werte ohne Worte.",
  "Das Interface wurde so gestaltet, dass es sich natürlich und vertraut anfühlt.",
  "Komplexe Informationen werden durch kluge Visualisierungen leicht verständlich.",
  "Die Stärke eines Designs liegt in seiner Einfachheit und Klarheit.",
  "Jede Entscheidung im Designprozess hat Auswirkungen auf das Endergebnis.",
  "Geduld und Sorgfalt sind die wichtigsten Werkzeuge im kreativen Prozess.",
  "Das fertige Produkt ist das Ergebnis unzähliger kleiner Entscheidungen.",
  "Design ist keine Dekoration, sondern eine strategische Disziplin.",
  "Die besten Designs entstehen durch enge Zusammenarbeit zwischen allen Beteiligten.",
  "Verständliche Sprache und klares Design gehen Hand in Hand.",
  "Der Nutzer steht immer im Zentrum jeder guten Gestaltungsentscheidung.",
  "Mut zur Vereinfachung führt oft zu den überzeugendsten Ergebnissen.",
  "Ein konsistentes Design schafft Orientierung und Vertrauen beim Nutzer.",
  "Die digitale Welt bietet unzählige Möglichkeiten für kreative Gestaltung.",
  "Gutes Design ist zeitlos und übersteht den Wandel der Moden.",
  "Die Zusammenführung verschiedener Perspektiven bereichert den kreativen Prozess.",
  "Technische Exzellenz und gestalterische Qualität sind kein Widerspruch.",
  "Am Ende entscheidet der Nutzer, ob ein Design erfolgreich ist oder nicht."
];
```

### Thuật toán generate:
```javascript
function generateText(numParagraphs, length) {
  // Số câu mỗi đoạn theo mức độ dài
  const sentenceCounts = { kurz: 4, mittel: 7, lang: 11 };
  const count = sentenceCounts[length];

  const result = [];
  // Shuffle corpus để tránh lặp
  const shuffled = [...CORPUS].sort(() => Math.random() - 0.5);
  let idx = 0;

  for (let i = 0; i < numParagraphs; i++) {
    const sentences = [];
    for (let j = 0; j < count; j++) {
      sentences.push(shuffled[idx % shuffled.length]);
      idx++;
      // Re-shuffle khi hết corpus
      if (idx % shuffled.length === 0) shuffled.sort(() => Math.random() - 0.5);
    }
    result.push(sentences.join(' '));
  }
  return result; // Array of paragraph strings
}
```

### Format output:
```javascript
// Không có HTML tags:
paragraphs.join('\n\n')

// Có HTML tags:
'<h2>Beispielüberschrift</h2>\n' +
paragraphs.map(p => `<p>${p}</p>`).join('\n')
```

### SEO bắt buộc:
- **Title:** `Lorem Ipsum Deutsch Online Kostenlos | TextHelfer` (50 ký tự)
- **Meta description:** `Deutschen Blindtext kostenlos generieren: Natürliche Platzhalterwörter statt lateinischem Lorem Ipsum. Anzahl und Länge wählbar, mit HTML-Tags. Keine Anmeldung.` (158 ký tự → Dev điều chỉnh về ≤160)
- **Canonical:** `<link rel="canonical" href="https://texthelfer.net/lorem-ipsum-deutsch/">`
- **H1:** `Lorem Ipsum Deutsch – Deutschen Blindtext generieren`
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
│  MAIN (max-width: 800px, margin: 0 auto, padding: 24px)     │
│  (tool này không cần sidebar — layout 1 cột đơn giản)       │
│                                                             │
│  H1: Lorem Ipsum Deutsch – Deutschen Blindtext generieren   │
│  Subtitle: Natürlichen deutschen Platzhaltertext...         │
│                                                             │
│  ┌──────────────── CONTROLS CARD ────────────────────────┐  │
│  │                                                       │  │
│  │  Anzahl Absätze: [  3  ]  (number input, 1–10)        │  │
│  │                                                       │  │
│  │  Länge:  (●) Kurz   ( ) Mittel   ( ) Lang             │  │
│  │                                                       │  │
│  │  [☐] HTML-Tags hinzufügen (<p>, <h2>)                 │  │
│  │                                                       │  │
│  │  [      Neu generieren (nút lớn, full-width)       ]  │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                             │
│  ┌──────────────── OUTPUT CARD ──────────────────────────┐  │
│  │  [Vorschau]  [Quelltext]          [Alles kopieren]    │  │
│  │  ─────────────────────────────────────────────────    │  │
│  │                                                       │  │
│  │  Die moderne Gestaltung verbindet Funktionalität...   │  │
│  │  Jedes Detail wurde sorgfältig ausgewählt...          │  │
│  │                                                       │  │
│  │  Das Produkt überzeugt durch seine klare...           │  │
│  │  Innovation und Tradition verbinden sich...           │  │
│  │                                                       │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                             │
│  ┌── AD: id="ad-top" ──────────────────────────────────┐    │
│  └─────────────────────────────────────────────────────┘    │
│                                                             │
│  H2: Wie funktioniert der Blindtext-Generator?              │
│                                                             │
│  ┌── AD: id="ad-middle" ───────────────────────────────┐    │
│  └─────────────────────────────────────────────────────┘    │
│                                                             │
│  H2: Für wen ist das geeignet?                              │
│                                                             │
│  ┌── AD: id="ad-bottom" ───────────────────────────────┐    │
│  └─────────────────────────────────────────────────────┘    │
│                                                             │
│  H2: Häufige Fragen                                         │
├─────────────────────────────────────────────────────────────┤
│  FOOTER                                                     │
└─────────────────────────────────────────────────────────────┘
```

---

### CONTROLS CARD

```
background: var(--bg-card)
border: 1px solid var(--border)
border-radius: 12px
padding: 24px
margin-bottom: 20px

--- Anzahl Absätze ---
Label: "Anzahl Absätze:" font-size 16px; font-weight 500
Number input:
  width: 72px; height: 44px
  text-align: center; font-size: 18px; font-weight: 600
  border: 2px solid var(--border); border-radius: 8px
  color: var(--text); background: var(--bg)
  :focus → border-color: var(--accent)
  Đặt cạnh label trên cùng 1 dòng (display flex; align-items center; gap 12px)
margin-bottom: 20px

--- Länge ---
Label: "Länge:" font-size 16px; font-weight 500; margin-bottom 8px
Radio group: display flex; gap 20px
Mỗi radio item: display flex; align-items center; gap 6px
  Radio: accent-color var(--accent); width 18px; height 18px
  Label: font-size 16px
margin-bottom: 20px

--- HTML-Tags Toggle ---
display flex; align-items center; gap 10px; margin-bottom 24px
Checkbox: width 20px; height 20px; accent-color var(--accent)
Label: font-size 16px
Sub-text: font-size 13px; color var(--text-muted)
  "Fügt <p> und <h2> Tags zum generierten Text hinzu"

--- Nút Neu generieren ---
width: 100%; height: 52px
background: var(--accent); color: #FFFFFF
font-size: 18px; font-weight: 600
border: none; border-radius: 8px; cursor: pointer
:hover → var(--accent-hover)
:active → transform scale(0.98)
Dark mode: background #60A5FA; color #0F172A
```

---

### OUTPUT CARD

```
background: var(--bg-card)
border: 1px solid var(--border)
border-radius: 12px
overflow: hidden
margin-bottom: 24px

--- Tab bar + Copy button ---
display flex; align-items center; justify-content space-between
padding: 0 16px
border-bottom: 1px solid var(--border)
height: 48px

Tab buttons (Vorschau / Quelltext):
  height: 48px; padding: 0 16px
  background: transparent; border: none; cursor: pointer
  font-size: 15px; font-weight: 500; color var(--text-muted)
  border-bottom: 2px solid transparent; margin-bottom: -1px
  Tab active: color var(--accent); border-bottom-color var(--accent); font-weight 600

Nút "Alles kopieren":
  height: 36px; padding: 0 14px
  background: transparent
  border: 1px solid var(--border); border-radius: 6px
  font-size: 14px; font-weight: 500; color var(--text-muted); cursor pointer
  :hover → border-color var(--accent); color var(--accent)
  Sau khi copy: "✓ Kopiert" màu xanh lá, 2 giây rồi về lại

--- Content area ---
padding: 24px
min-height: 200px

Tab VORSCHAU:
  Render HTML thật (innerHTML)
  h2: font-size 20px; font-weight 600; margin-bottom 12px
  p: font-size 17px; line-height 1.8; margin-bottom 16px; color var(--text)

Tab QUELLTEXT:
  <pre> hoặc <textarea readonly>
  font-family: monospace; font-size: 14px; line-height 1.6
  color var(--text); background var(--bg)
  white-space: pre-wrap; word-break: break-word
  border: none; width 100%; resize none; outline none
  min-height: 200px
```

---

### RESPONSIVE — MOBILE (< 768px)

| Thay đổi | Desktop | Mobile |
|---|---|---|
| Layout | 1 cột (đã đơn giản) | Giữ nguyên |
| H1 font-size | 28px | 22px |
| Padding | 24px | 16px |
| Card padding | 24px | 16px |
| Radio group | flex row | flex row (vẫn đủ chỗ vì chỉ 3 options) |
| Tất cả nút | height 52px | height 52px |

---

## 4. ACCEPTANCE CRITERIA

### Functional:
- [ ] Load trang → tự động generate sẵn 3 đoạn Mittel, không hiện HTML tags
- [ ] Thay đổi số đoạn → KHÔNG tự động re-generate
- [ ] Thay đổi độ dài → KHÔNG tự động re-generate
- [ ] Bấm "Neu generieren" → text mới xuất hiện
- [ ] Bật HTML-Tags → output cập nhật ngay, thêm `<p>` và `<h2>` mà không re-generate
- [ ] Tab Vorschau → text render như HTML (p tag tạo đoạn văn, h2 tạo tiêu đề)
- [ ] Tab Quelltext → hiển thị text thuần hoặc với tags tùy toggle
- [ ] Nút "Alles kopieren" → copy đúng nội dung tab đang active → toast 2 giây
- [ ] Range số đoạn: min 1, max 10
- [ ] Text generate từ corpus tiếng Đức (không phải Latin Lorem Ipsum)

### Design:
- [ ] Layout 1 cột, max-width 800px
- [ ] Number input: width 72px, căn giữa số
- [ ] Nút Neu generieren: full-width, height 52px
- [ ] Tab bar + Copy button trên cùng 1 dòng
- [ ] Vorschau: text render HTML thật, p có line-height 1.8
- [ ] Quelltext: font monospace, font-size 14px

### SEO & Technical:
- [ ] `<html lang="de">` có trong source
- [ ] Title ≤ 60 ký tự, keyword "lorem ipsum deutsch" đứng đầu
- [ ] 1 thẻ H1 duy nhất
- [ ] Canonical: `https://texthelfer.net/lorem-ipsum-deutsch/`
- [ ] WebApplication + FAQPage JSON-LD hợp lệ, ≥ 6 Q&A
- [ ] Lighthouse Performance ≥ 90, FCP < 1 giây

### UX:
- [ ] Desktop 1920px: layout đúng, không break
- [ ] Mobile 375px: không overflow
- [ ] Tất cả nút height ≥ 52px
- [ ] Dark mode: không flash khi reload, giữ preference

### Integration:
- [ ] Navigation bar: 9 tool links
- [ ] Footer: link Kontakt
- [ ] ≥ 3 internal links ngữ cảnh (gợi ý: Zeichenzähler, Wörter zählen, Textvergleich)
- [ ] 3 ad placeholders: `id="ad-top"`, `id="ad-middle"`, `id="ad-bottom"`

### Deploy:
- [ ] PR merged vào `main`
- [ ] GitHub Actions deploy thành công
- [ ] URL live: `https://texthelfer.net/lorem-ipsum-deutsch/`
- [ ] HTTPS, không mixed content warning

---

## 5. DELIVERABLES

- [ ] `lorem-ipsum-deutsch/index.html` — file tool hoàn chỉnh
- [ ] Cập nhật `index.html` — mark card Lorem Ipsum Deutsch là live
- [ ] Cập nhật `sitemap.xml` — thêm `/lorem-ipsum-deutsch/`
- [ ] Commit file brief này với status: `DONE`

---

## 6. NỘI DUNG SEO TIẾNG ĐỨC

### H2: Wie funktioniert der Blindtext-Generator?
*(Dev viết ~150 từ, keyword "lorem ipsum deutsch" hoặc "blindtext" xuất hiện 2–3 lần)*

Gợi ý: giải thích tại sao cần blindtext tiếng Đức thay vì Latin (stakeholder đọc được, phát hiện layout issue sớm hơn), corpus 500 câu tự nhiên, tùy chọn HTML tags cho developer, preview mode cho designer.

### H2: Für wen ist der Blindtext-Generator geeignet?
- Webdesigner und UI/UX-Designer, die realistische Layouts mit deutschen Texten gestalten
- Frontend-Entwickler, die Komponenten mit authentischem Platzhaltertext testen
- Grafikdesigner, die Print-Layouts und Präsentationen mit lesbarem Fülltext befüllen
- Agenturen, die Kunden-Mockups präsentieren, die ohne technisches Vorwissen verstanden werden
- Alle, die professionelle Dummy-Texte auf Deutsch benötigen

### FAQ — 6 Q&A:

**F1:** Was ist Lorem Ipsum und warum gibt es eine deutsche Version?
**A1:** Lorem Ipsum ist ein seit dem 16. Jahrhundert verwendeter lateinischer Platzhaltertext für Layouts und Druckvorlagen. Das Problem: Lateinische Texte sind für deutsche Kunden und Stakeholder unlesbar, was bei Präsentationen zu Verwirrung führt. Unser Blindtext Generator erstellt natürliche deutsche Platzhalterwörter, die im Layout genauso wirken wie echter Inhalt.

**F2:** Was ist ein Blindtext?
**A2:** Ein Blindtext ist ein sinnfreier oder bedeutungsarmer Text, der in der Gestaltung als Platzhalter für späteren echten Inhalt verwendet wird. Er hilft Designern, das visuelle Erscheinungsbild eines Layouts zu beurteilen, ohne auf fertigen Inhalt angewiesen zu sein. Im Unterschied zu Lorem Ipsum sind unsere deutschen Blindtexte lesbar und erzeugen einen realistischeren Eindruck.

**F3:** Kann ich den generierten Text kommerziell verwenden?
**A3:** Ja, der generierte Blindtext ist lizenzfrei und kann für kommerzielle und private Projekte beliebig verwendet werden. Die Texte wurden speziell für diesen Zweck erstellt und enthalten keine urheberrechtlich geschützten Inhalte. Du kannst ihn für Websites, Apps, Präsentationen, Druckerzeugnisse und alle anderen Zwecke einsetzen.

**F4:** Was bewirkt die HTML-Tags-Option?
**A4:** Wenn du die Option "HTML-Tags hinzufügen" aktivierst, wird jeder Absatz automatisch in `<p>`-Tags eingeschlossen, und vor dem ersten Absatz wird ein `<h2>`-Tag eingefügt. Das ist praktisch für Entwickler, die den Text direkt in ihren HTML-Code einfügen möchten, ohne Tags manuell hinzufügen zu müssen.

**F5:** Wie unterscheiden sich die Längenoptionen Kurz, Mittel und Lang?
**A5:** "Kurz" generiert Absätze mit etwa 3 bis 4 Sätzen und rund 50 Wörtern — ideal für Teasertexte oder Kartenlayouts. "Mittel" erzeugt 6 bis 7 Sätze mit etwa 100 Wörtern, passend für Standard-Inhaltsseiten. "Lang" erstellt 10 bis 12 Sätze mit rund 200 Wörtern, geeignet für Artikel oder ausführliche Textbereiche.

**F6:** Warum sind die Texte auf Deutsch statt auf Latein?
**A6:** Lateinische Lorem-Ipsum-Texte haben einen entscheidenden Nachteil: Kunden und Auftraggeber können den Inhalt nicht lesen und beurteilen daher oft nur das visuelle Erscheinungsbild. Deutsche Platzhalterwörter ermöglichen es, Layout und Lesbarkeit gleichzeitig zu beurteilen. Außerdem entsprechen deutsche Texte besser der tatsächlichen Wortlänge und Zeilenstruktur, die später im fertigen Produkt erscheinen wird.

---

## 7. PROMPT GIAO CHO DEV (Claude CLI)

Owner paste đoạn sau vào Claude CLI:

```
Bạn là Dev Agent của dự án TextHelfer.

Đọc task brief tại: docs/tasks/TASK-005-lorem-ipsum-deutsch.md

Lưu ý: Đây là tool đơn giản nhất — không có sidebar, layout 1 cột.
Corpus 60 câu tiếng Đức đã viết sẵn trong brief (phần Tech Notes).
Dev chỉ cần implement thuật toán generate và UI controls.

Sau khi đọc xong:
1. Tóm tắt lại những gì bạn hiểu về task này trong 3–5 câu
2. Liệt kê những gì bạn sẽ làm theo thứ tự
3. Hỏi nếu có gì chưa rõ

Chờ xác nhận từ Owner trước khi bắt đầu code.
```

---

**PM Sign-off:** [ ] Chờ ký duyệt
**Ngày ký duyệt:** ___________
**Ghi chú ký duyệt:** ___________
