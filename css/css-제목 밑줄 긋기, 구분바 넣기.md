## 밑줄 긋기

가상요소를 활용한다.

```html

<h1 class="underscore"></h1>

```

```css
.underscore { margin: 10px; padding: 5px; }
.underscore:after { content: ""; display:block; width: 100px; border-bottom: 15px solid pink; border-radius: 15px; }
```

## 구분 bar

```html
<div id="content">
  <ul class="test">
    <li><a href="">login</a></li>
    <li><a href="">home</a></li>
    <li><a href="">sitemap</a></li>
  </ul>
</div>
```

```css
.test li { float: left, margin-right: 5px; }
.test li::after { padding-left: 5px; content: "|" }
.test li:last-child::after { content: "" }
```
