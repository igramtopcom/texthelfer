# TASK-004 — Lesbarkeit prüfen

**Status:** DONE ✅
**PM Sign-off:** APPROVED
**Ngày ký duyệt:** 2026-03-09
**Kết quả test:** 32/33 — 1 KHÔNG ĐẠT do lỗi brief, không phải lỗi code

## Deliverables
- [x] `lesbarkeit/index.html` — full tool
- [x] `sitemap.xml` — updated with /lesbarkeit/
- [x] Live: https://texthelfer.net/lesbarkeit/

## Features
- Flesch Reading Ease (German/Amstad): 180 − ASL − 58.5 × ASW, clamped [0,100]
- Wiener Sachtextformel (simplified): 0.2656 × MS + 0.2744 × SL − 1.693
- Tab switching between Flesch and Wiener views
- 5 classification levels with color-coded badge (20% opacity bg)
- Score display: 64px, 800 weight, centered
- 4 stat rows: Ø Wörter/Satz, Ø Silben/Wort, Sätze, Wörter
- Sentence highlighting preview (yellow >20, red >30 words)
- Long sentence warning (>20 words)
- Empty state for <3 words with 📝 icon
- Dark mode with localStorage persistence
- Responsive: 2-column desktop (sidebar 300px), stacked mobile (score 48px)
- SEO: WebApplication + FAQPage JSON-LD, 6 FAQ, 5 internal links

## Ghi chú PM
Item #4 KHÔNG ĐẠT do PM ghi sai khoảng kỳ vọng trong brief. Công thức Flesch (DE) và Wiener Sachtextformel implement đúng theo spec. Kết quả định tính "Sehr leicht" cho đoạn văn mẫu là chính xác.
