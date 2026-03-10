# TASK-013 — Google Analytics 4 & Search Console Integration

**Ngày tạo:** 2026-03-09
**PM:** TextHelfer PM Agent
**Assignee:** Dev Agent (Claude CLI)
**Priority:** P0 — cần thiết để theo dõi traffic và SEO performance
**Status:** DONE ✅
**Branch:** feature/TASK-013-analytics
**Estimate:** 2–3 giờ

---

## 1. CONTEXT

TextHelfer cần tích hợp Google Search Console và Google Analytics 4 để theo dõi traffic organic, keyword impressions, và hành vi người dùng. Đây là bước bắt buộc trước khi apply AdSense. Task này chỉ thêm code vào `<head>` — không thay đổi UI hay chức năng bất kỳ tool nào.

---

## 2. REQUIREMENTS

### 2.1 Google Search Console — Verification Tag

Thêm meta tag sau vào `<head>` của **tất cả 14 file HTML**:

```html
<meta name="google-site-verification" content="1i35qIHvSiRVE8XEYBeMSJUxj7xg1AoSEymf7Fw1wkI" />
```

**Vị trí:** Ngay sau `<meta charset="UTF-8">` — dòng đầu tiên trong `<head>`

---

### 2.2 Google Analytics 4 — Tracking Code

Thêm GA4 snippet sau vào `<head>` của **tất cả 14 file HTML**:

```html
<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-EVY152B7GE"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-EVY152B7GE');
</script>
```

**Vị trí:** Ngay sau Search Console meta tag, trước Dark Mode script

---

### 2.3 Danh sách 14 file cần cập nhật

**9 tool pages:**
- `passwort-generator/index.html`
- `zeichenzaehler/index.html`
- `woerter-zaehlen/index.html`
- `lesbarkeit/index.html`
- `lorem-ipsum-deutsch/index.html`
- `text-formatierung/index.html`
- `umlaut-konverter/index.html`
- `textvergleich/index.html`
- `zufallszahlen/index.html`

**Homepage + legal pages:**
- `index.html`
- `kontakt.html`
- `about.html`
- `datenschutz.html`
- `nutzungsbedingungen.html`

---

### 2.4 Thứ tự đúng trong `<head>` sau khi thêm

```html
<head>
  <meta charset="UTF-8">
  <meta name="google-site-verification" content="1i35qIHvSiRVE8XEYBeMSJUxj7xg1AoSEymf7Fw1wkI" />

  <!-- Google tag (gtag.js) -->
  <script async src="https://www.googletagmanager.com/gtag/js?id=G-EVY152B7GE"></script>
  <script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());
    gtag('config', 'G-EVY152B7GE');
  </script>

  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>...</title>
  <!-- ... các meta tag SEO khác ... -->

  <!-- Dark Mode script — phải giữ nguyên vị trí trong <head> -->
  <script>
    const theme = localStorage.getItem("texthelfer-theme");
    if (theme === "dark" || theme === null) {
      document.documentElement.setAttribute("data-theme", "dark");
    }
  </script>
</head>
```

---

## 3. TECH NOTES

- **Không** thay đổi bất kỳ nội dung, CSS, hay JavaScript nào ngoài việc thêm 2 đoạn code trên
- GA4 script có `async` — không ảnh hưởng đến Lighthouse Performance
- Dark Mode script phải giữ nguyên vị trí trong `<head>` — không được di chuyển
- Dùng grep/sed hoặc find-replace để thêm vào tất cả 14 file cùng lúc — tránh sai sót khi làm tay từng file

---

## 4. ACCEPTANCE CRITERIA

```
[ ] Search Console meta tag có trong <head> của tất cả 14 file
    Kiểm tra: grep "google-site-verification" tất cả .html files
    Kết quả: ___________

[ ] GA4 script có trong <head> của tất cả 14 file
    Kiểm tra: grep "G-EVY152B7GE" tất cả .html files
    Kết quả: ___________

[ ] Thứ tự trong <head> đúng: charset → verification → GA4 → viewport → title → SEO tags → dark mode script
    Kiểm tra trên ít nhất 2 file (index.html + 1 tool)
    Kết quả: ___________

[ ] Dark Mode script vẫn nằm trong <head>, không bị di chuyển
    Kết quả: ___________

[ ] Lighthouse Performance vẫn ≥ 90 sau khi thêm GA4 (async không ảnh hưởng)
    Score: ___________

[ ] 0 console errors trên tất cả trang
    Kết quả: ___________

[ ] Tất cả tool chức năng vẫn hoạt động bình thường (regression check nhanh)
    Kết quả: ___________
```

---

## 5. DELIVERABLES

- [ ] 14 file HTML đã cập nhật — tất cả có Search Console tag + GA4 script
- [ ] Commit file brief này với status: `DONE`

---

> **Bước tiếp theo:**
> Owner paste prompt giao việc vào Claude CLI (Dev Agent):
> ```
> Bạn là Dev Agent của dự án TextHelfer.
> Đọc task brief tại: docs/tasks/TASK-013-analytics.md
> Sau khi đọc xong:
> 1. Tóm tắt lại những gì bạn hiểu về task này trong 3–5 câu
> 2. Liệt kê những gì bạn sẽ làm (không code ngay)
> 3. Hỏi nếu có gì chưa rõ
> Chờ xác nhận từ Owner trước khi bắt đầu code.
> ```

---

**PM Sign-off:** [x] ĐÃ KÝ DUYỆT ✅
**Ngày ký duyệt:** 2026-03-09
**Ghi chú ký duyệt:** Task sạch — verification tag + GA4 trên toàn bộ 14 file, dark mode không bị ảnh hưởng, 0 console errors.
