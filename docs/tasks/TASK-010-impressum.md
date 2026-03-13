# TASK-010 — Impressum & Datenschutz Pages

**Ngày tạo:** 2026-03-13
**PM:** TextHelfer PM Agent
**Assignee:** Dev Agent (Claude CLI)
**Priority:** P0 — PHẢI XONG TRƯỚC KHI APPLY ADSENSE
**Status:** TODO
**Branch:** feature/TASK-010-impressum
**Estimate:** 2–3 giờ

---

## 1. CONTEXT

TextHelfer hiện chưa có trang Impressum — đây là yêu cầu pháp lý bắt buộc theo § 5 TMG (Telemediengesetz) Đức cho mọi website thương mại có quảng cáo. Thiếu Impressum có thể bị phạt đến €50.000 và Google AdSense sẽ từ chối approve. Ngoài ra, footer hiện tại có link "Über uns" không cần thiết — cần thay bằng "Impressum". Trang Datenschutz (chính sách bảo mật) cũng cần có nội dung đúng chuẩn DSGVO (GDPR phiên bản Đức).

---

## 2. REQUIREMENTS

### Chức năng bắt buộc:

**Trang Impressum (`/impressum/index.html`):**
- [ ] Hiển thị đầy đủ thông tin pháp nhân theo § 5 TMG:
  - Tên công ty: **RIVERNET TECHNOLOGY PTE. LTD.**
  - Địa chỉ: **PENINSULA SHOPPING COMPLEX, 3 COLEMAN STREET #03-024, Singapore 179804**
  - Email: **support@texthelfer.net**
  - Hinweis: Verantwortlich für den Inhalt nach § 55 Abs. 2 RStV
- [ ] Có section "Haftungsausschluss" (disclaimer) chuẩn
- [ ] Có section "Urheberrecht" (copyright)
- [ ] Có section "Datenschutz" ngắn với link đến trang /datenschutz/
- [ ] Layout nhất quán với các trang tool (header, nav, footer giống hệt)

**Trang Datenschutz (`/datenschutz/index.html`):**
- [ ] Nội dung DSGVO-konform (phù hợp GDPR) bằng tiếng Đức
- [ ] Giải thích rõ: không thu thập dữ liệu người dùng, không có server-side processing
- [ ] Section về Google AdSense (dù chưa active, cần khai báo trước)
- [ ] Section về Google Analytics / Search Console (nếu có)
- [ ] Section về Cookies: chỉ có localStorage cho theme preference — không có tracking cookie
- [ ] Thông tin liên hệ: support@texthelfer.net
- [ ] Layout nhất quán với các trang tool

**Cập nhật footer TẤT CẢ các trang đã live:**
- [ ] Xóa link "Über uns" (nếu có)
- [ ] Thêm link "Impressum" → /impressum/
- [ ] Giữ "Datenschutz" → /datenschutz/
- [ ] Giữ "Kontakt" → /kontakt.html
- [ ] Footer mới: `Passwort Generator | Zeichenzähler | ... | Impressum | Datenschutz | Kontakt | © 2026 TextHelfer`

### Chức năng không được phá vỡ:
- [ ] Navigation header của tất cả trang giữ nguyên
- [ ] Dark mode tất cả trang vẫn hoạt động đúng
- [ ] SEO content của các trang tool không bị thay đổi

---

## 3. TECH NOTES

> Các ràng buộc kỹ thuật bất biến — Dev KHÔNG được bỏ qua:

- `<html lang="de">` trên thẻ html — bắt buộc
- Font: `system-ui` — KHÔNG dùng Google Fonts
- Toàn bộ CSS + JS inline trong 1 file HTML duy nhất
- KHÔNG dùng framework hoặc external JS library
- Dark mode: script đọc `localStorage.getItem("texthelfer-theme")` đặt trong `<head>`
- Navigation header: link đến tất cả 9 tool + dark mode toggle
- Màu sắc:
  - Light: bg `#FFFFFF`, text `#111827`, accent `#2563EB`, border `#E2E8F0`
  - Dark: bg `#0F172A`, text `#F1F5F9`, accent `#60A5FA`, border `#334155`
- localStorage key: `"texthelfer-theme"`

### SEO cho trang Impressum:
- Title: `Impressum | TextHelfer` (không cần keyword — trang legal)
- Meta robots: `<meta name="robots" content="noindex, follow">` — QUAN TRỌNG: trang legal không cần index
- Canonical: `<link rel="canonical" href="https://texthelfer.net/impressum/">`

### SEO cho trang Datenschutz:
- Title: `Datenschutzerklärung | TextHelfer`
- Meta robots: `<meta name="robots" content="noindex, follow">`
- Canonical: `<link rel="canonical" href="https://texthelfer.net/datenschutz/">`

### Nội dung Impressum — mẫu chuẩn (Dev dùng nguyên):

```
Impressum

Angaben gemäß § 5 TMG

RIVERNET TECHNOLOGY PTE. LTD.
Peninsula Shopping Complex
3 Coleman Street #03-024
Singapore 179804

Kontakt
E-Mail: support@texthelfer.net

Verantwortlich für den Inhalt nach § 55 Abs. 2 RStV
RIVERNET TECHNOLOGY PTE. LTD.
3 Coleman Street #03-024, Singapore 179804

Haftungsausschluss

Haftung für Inhalte
Die Inhalte unserer Seiten wurden mit größter Sorgfalt erstellt. 
Für die Richtigkeit, Vollständigkeit und Aktualität der Inhalte 
können wir jedoch keine Gewähr übernehmen. Als Diensteanbieter 
sind wir gemäß § 7 Abs.1 TMG für eigene Inhalte auf diesen Seiten 
nach den allgemeinen Gesetzen verantwortlich.

Haftung für Links
Unser Angebot enthält Links zu externen Webseiten Dritter, auf 
deren Inhalte wir keinen Einfluss haben. Für die Inhalte der 
verlinkten Seiten ist stets der jeweilige Anbieter oder Betreiber 
der Seiten verantwortlich.

Urheberrecht
Die durch die Seitenbetreiber erstellten Inhalte und Werke auf 
diesen Seiten unterliegen dem deutschen Urheberrecht. Die 
Vervielfältigung, Bearbeitung, Verbreitung und jede Art der 
Verwertung außerhalb der Grenzen des Urheberrechtes bedürfen 
der schriftlichen Zustimmung des jeweiligen Autors bzw. Erstellers.
```

### Nội dung Datenschutz — các điểm chính cần có:

```
1. Datenschutz auf einen Blick
   - Keine Datenerhebung, keine Registrierung erforderlich
   - Alle Berechnungen laufen lokal im Browser
   - Kein Text wird an einen Server übertragen

2. Verantwortliche Stelle
   - RIVERNET TECHNOLOGY PTE. LTD.
   - support@texthelfer.net

3. Datenerhebung auf dieser Website
   3.1 Cookies und lokale Speicherung
       - localStorage: nur "texthelfer-theme" (Hell/Dunkel-Modus)
       - Keine Tracking-Cookies
   3.2 Server-Log-Dateien
       - Hosting-Provider speichert Standard-Logs (IP, Zeitstempel)
       - Keine Weitergabe an Dritte

4. Google AdSense (Hinweis für künftige Nutzung)
   - Diese Website plant den Einsatz von Google AdSense
   - Google AdSense verwendet Cookies zur Anzeigenpersonalisierung
   - Opt-out möglich unter: https://www.google.com/settings/ads

5. Ihre Rechte
   - Auskunft, Berichtigung, Löschung nach DSGVO Art. 15–17
   - Kontakt: support@texthelfer.net
```

---

## 4. ACCEPTANCE CRITERIA

### Functional:
- [ ] `/impressum/` truy cập được, hiển thị đúng thông tin công ty
- [ ] `/datenschutz/` truy cập được, có đủ các section theo yêu cầu
- [ ] Footer tất cả trang đã live có link Impressum và Datenschutz
- [ ] Không còn link "Über uns" trong footer (nếu có)

### Technical:
- [ ] `<meta name="robots" content="noindex, follow">` có trong cả 2 trang
- [ ] `<html lang="de">` có trong source
- [ ] Dark mode hoạt động đúng trên cả 2 trang mới
- [ ] Không có broken link trong footer

### UX:
- [ ] Layout nhất quán với các trang tool (header, nav, footer giống hệt)
- [ ] Readable trên mobile 375px
- [ ] Font size body tối thiểu 16px, dễ đọc

### Deploy:
- [ ] PR đã merge vào `main`
- [ ] GitHub Actions deploy thành công
- [ ] `https://texthelfer.net/impressum/` truy cập được
- [ ] `https://texthelfer.net/datenschutz/` truy cập được
- [ ] HTTPS hoạt động, không mixed content warning

---

## 5. DELIVERABLES

Dev cần commit lên Git khi task hoàn thành:

- [ ] `impressum/index.html` — trang Impressum mới
- [ ] `datenschutz/index.html` — trang Datenschutz mới
- [ ] Cập nhật footer trong `index.html` (trang chủ)
- [ ] Cập nhật footer trong tất cả 9 file tool đã live
- [ ] Cập nhật `sitemap.xml` — thêm 2 URL mới (hoặc KHÔNG thêm vì noindex — Dev quyết định)
- [ ] Commit file brief này với status: `DONE`

---

## 6. GHI CHÚ THÊM

- Trang Impressum và Datenschutz dùng `noindex` vì không cần/không muốn Google index trang legal — đây là practice chuẩn
- Nội dung Impressum đã được PM soạn sẵn trong mục 3, Dev chỉ cần format thành HTML đẹp
- Datenschutz cần mention Google AdSense dù chưa active — để khi AdSense được approve thì không cần update lại
- Không cần thêm vào sitemap.xml vì 2 trang này noindex

---

**PM Sign-off:** [ ] Chờ ký duyệt
**Ngày ký duyệt:** ___________
**Ghi chú ký duyệt:** ___________
