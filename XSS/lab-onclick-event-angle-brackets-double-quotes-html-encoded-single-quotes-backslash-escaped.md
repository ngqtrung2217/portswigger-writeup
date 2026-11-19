# Lab: Stored XSS into onclick event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped
- **Category:** XSS
- **Độ khó:** Practitioner
- **Link lab:** https://portswigger.net/web-security/cross-site-scripting/contexts/lab-onclick-event-angle-brackets-double-quotes-html-encoded-single-quotes-backslash-escaped


## Summary

Lab này có stored xss ở func comment. Fire alert để solve.

## Step-to-reproduce

Có event onclick mà chứa url mà ta control được -> khả nghi.

![](../0_images/XSS/lab-onclick-event-angle-brackets-double-quotes-html-encoded-single-quotes-backslash-escaped/Screenshot%20from%202026-11-20%2000-17-17.png)

Thử payload `http://a.com\';alert(1)//` và bị escape thành `\'`

![](../0_images/XSS/lab-onclick-event-angle-brackets-double-quotes-html-encoded-single-quotes-backslash-escaped/Screenshot%20from%202026-11-20%2000-20-19.png)

Thử dùng HTML encoding `'` --> `&apos`
Payload có dạng là `http://a.com&apos;alert(1)//`

![](../0_images/XSS/lab-onclick-event-angle-brackets-double-quotes-html-encoded-single-quotes-backslash-escaped/Screenshot%20from%202026-11-20%2000-24-06.png)

Nhận thấy ko bị escape nữa, và bị thiếu `;`

Hoàn thiện payload `http://a.com&apos;-alert(1)-&apos;`

![](../0_images/XSS/lab-onclick-event-angle-brackets-double-quotes-html-encoded-single-quotes-backslash-escaped/Screenshot%20from%202026-11-20%2000-27-33.png)

![](../0_images/XSS/lab-onclick-event-angle-brackets-double-quotes-html-encoded-single-quotes-backslash-escaped/Screenshot%20from%202026-11-20%2000-34-24.png)


## Root cause

URL khi submit được lưu nguyên và lưu trực tiếp vào onclick -> nối thành 1 chuỗi event handler JS và có escape `'` và `\`, nhưng có thể nó đã bỏ qua HTML-decode trước khi parse và thực thi.

Vậy `&apos;` ko bị nhận diện và khi bị thêm `\`. Và khi browser chạy thì `&apos` được decode thành `'`, đóng chuỗi JS và trở thành code thực thi. Payload `http://a.com&apos;-aler(1)-&apos;`

## Impact

Stored XSS: payload được lưu ở DB và ở phần comment (được nhiều người reach đến). Mã chạy cùng origin nên có đủ quyền của nạn nhân: đọc cookies, lấy CSRF token,...


