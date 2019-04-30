## 팝업창만 스크롤 되고, 부모창은 그대로

```html
<html>
  <body>
    <div id="popup">
      <div id="popup-content">
        ...
      </div>
    </div>
    <div id="page-content">
      <div>
        ...
      </div>
    </div>
  </body>
</html>
```

```css
html, body, #page-content {
  height: 100%;
  overflow: auto;
}
 
#popup {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
 
#popup-content {
  height: 100%;
  overflow: scroll;
}
```

스크롤하고자하는 영역(팝업)보다 부모의 영역이 더 커서 부모창도 스크롤이 된다.

‘#popup’ 영역 바깥의 영역의 높이 값을 100%로 디바이스(브라우저) 높이와 같게 설정하고 
‘overflow’ 설정을 ‘auto’로 주어 ‘#popup’의 영역보다 더 큰 영역을 갖을 수 없도록 하여 스크롤이 영향을 받지 않도록 하자
