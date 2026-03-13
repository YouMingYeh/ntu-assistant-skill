# Dashboard HTML Template Spec

Generate a single `dashboard.html`. Self-contained, all CSS in `<style>`. No external dependencies. No JavaScript.

## CRITICAL: Style Rules

The design must look like Notion or shadcn/ui. That means:

- **Only 3 colors**: white (`#fff`), black (`#111`), and light gray (`#e5e5e5` for borders)
- **No colored backgrounds**, no colored badges, no colored borders, no accent colors
- **No background colors on table cells or headers** — everything is white
- **Urgent deadlines**: just use bold text, no red, no colored badges
- **Links**: underlined `#111` text, nothing else
- **No box-shadow, no gradients, no border-radius larger than 4px**
- **No icons, no emoji** in the HTML itself
- Font: `system-ui, -apple-system, 'Segoe UI', sans-serif`
- Font size: `15px` body, `13px` for small/secondary text
- Line height: `1.6`
- Max width: `680px`, centered with `margin: 0 auto`
- Generous whitespace: sections separated by `3rem`, elements by `1rem`

## Required CSS

Use this exact CSS. Do not add to it, do not modify colors or fonts.

```css
* { margin: 0; padding: 0; box-sizing: border-box; }
body {
  font-family: system-ui, -apple-system, 'Segoe UI', sans-serif;
  font-size: 15px;
  line-height: 1.6;
  color: #111;
  background: #fff;
  max-width: 680px;
  margin: 0 auto;
  padding: 2rem 1.5rem 4rem;
}
h1 { font-size: 1.5rem; font-weight: 600; margin-bottom: 0.25rem; }
h2 { font-size: 1.1rem; font-weight: 600; margin-bottom: 1rem; padding-bottom: 0.5rem; border-bottom: 1px solid #e5e5e5; }
h3 { font-size: 1rem; font-weight: 600; margin-bottom: 0.5rem; }
h4 { font-size: 0.85rem; font-weight: 600; color: #555; margin: 1rem 0 0.25rem; text-transform: uppercase; letter-spacing: 0.03em; }
p, li { color: #333; }
a { color: #111; }
small { color: #888; font-weight: 400; }
header { margin-bottom: 3rem; }
header p { color: #888; font-size: 13px; }
section { margin-bottom: 3rem; }
article { margin-bottom: 2rem; padding-bottom: 2rem; border-bottom: 1px solid #e5e5e5; }
article:last-child { border-bottom: none; }
table { width: 100%; border-collapse: collapse; font-size: 14px; }
th { text-align: left; font-weight: 600; padding: 0.5rem 0.75rem; border-bottom: 2px solid #e5e5e5; }
td { padding: 0.5rem 0.75rem; border-bottom: 1px solid #f0f0f0; vertical-align: top; }
ul { list-style: none; }
ul li { padding: 0.35rem 0; }
ul li + li { border-top: 1px solid #f5f5f5; }
.urgent { font-weight: 600; }
.table-wrap { overflow-x: auto; }
@media print { body { max-width: 100%; padding: 1rem; } }
```

## HTML Structure

```html
<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>NTU {semester} Dashboard</title>
  <style>/* paste the CSS above */</style>
</head>
<body>

<header>
  <h1>{semester} 課程總覽</h1>
  <p>更新時間：{YYYY-MM-DD}</p>
</header>

<section>
  <h2>課表</h2>
  <div class="table-wrap">
    <table>
      <thead><tr><th>時間</th><th>一</th><th>二</th><th>三</th><th>四</th><th>五</th><th>六</th></tr></thead>
      <tbody>
        <!-- One <tr> per time slot, only include slots that have classes -->
        <tr><td>2 (09:10)</td><td>課程名稱</td><td></td><td></td><td></td><td></td><td></td></tr>
      </tbody>
    </table>
  </div>
</section>

<section>
  <h2>近期截止</h2>
  <div class="table-wrap">
    <table>
      <thead><tr><th>日期</th><th>課程</th><th>作業</th></tr></thead>
      <tbody>
        <!-- class="urgent" on <tr> for items due within 7 days -->
        <tr class="urgent"><td>2026-03-20</td><td>課程名稱</td><td>HW1</td></tr>
      </tbody>
    </table>
  </div>
</section>

<section>
  <h2>各課程</h2>

  <article>
    <h3>課程名稱 <small><a href="{cool_url}" target="_blank">COOL</a></small></h3>

    <h4>公告</h4>
    <ul>
      <li><strong>標題</strong> <small>2026-03-10</small><br>摘要內容</li>
    </ul>

    <h4>作業</h4>
    <ul>
      <li><a href="{url}" target="_blank">作業名稱</a> <small>截止 2026-03-30</small></li>
    </ul>

    <h4>教材</h4>
    <ul>
      <li><a href="{url}" target="_blank">filename.pdf</a></li>
    </ul>
  </article>

  <!-- repeat <article> for each course -->
</section>

<section>
  <h2>成績</h2>
  <div class="table-wrap">
    <table>
      <thead><tr><th>課程</th><th>分數</th><th>等第</th></tr></thead>
      <tbody>
        <tr><td>課程名稱</td><td>85.5</td><td>A</td></tr>
      </tbody>
    </table>
  </div>
</section>

</body>
</html>
```

## Rules

- **Use the CSS above exactly as written.** Do not invent new styles.
- If a section has no data, omit the entire `<section>`.
- Only include time slots that have classes (don't render 15 empty rows).
- Dates: `YYYY-MM-DD`.
- All links: `target="_blank"`.
- No JavaScript. No animations. No hover effects. No emoji.
- The page should look clean when printed.
