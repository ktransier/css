### CSS

#### In a parent div containing two floating child divs, match the parents height to the tallest child div

```css
#container { width:200px; overflow: hidden; }
.floated   { width:100px; float:left; }
```

```html
<div id="container">
  <div class="floated">A</div>
  <div class="floated">B</div>
</div>
```

[Source](http://stackoverflow.com/a/1294843/4233556)
