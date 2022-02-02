# CSRF

## Concepts
- اگر سرور بررسی نکند که آیا درخواست واقعا از کاربر آمده است یا نه، این حمله رخ می دهد

## Exploit
- در خواست تغییر ایمیل را به Burp می بریم، اگر CSRF Token نداشت، از طریق Burp یک PoC می سازیم
- اگر CSRF Token داشت از روش های زیر استفاده می کنیم :
  - متد را از POST به GET تغییر می دهیم
  - نام پارامتر و مقدار CSRF Token را از درخواست حذف می کنیم
  - اگر CSRF Token به هیچ مقداری درون کوکی tied نشده باشد
    - با کاربر A لاگین کرده و CSRF Token درخواست تغییر ایمیل را برمی داریم
    - در خواست تغییر ایمیل کاربر B را متوقف کرده و CSRF Token کاربر A را به جای CSRF Token آن قرار می دهیم و PoC را ایجاد می کنیم
    - نکته: از آنجائیکه CSRF Token یک بار مصرف است، باید درخواست تغییر ایمیل کاربر A را drop کرده باشیم
  - اگر CSRF Token به مقداری از Cookie، به جز SessionId وصل شده باشد (مثلا csrfkey)
    - بررسی می کنیم که در کجای سایت می شود این کوکی (csrfkey) را در مرورگر قربانی تزریق کرد (مثلا در بخش search ممکن است این کوکی به همراه مقداری که جستجو کردیم، به کوکی ها اضافه گردد) که نتیجه این کار یک URL می شود
    - سپس PoC را ایجاد کرده و تگ img را به جای script اجرای خودکار قرار می دهیم تا URL ای که برای تزریق Cookie ایجاد کرده بودیم هم اعمال گردد
```HTML
<img src="https://acbd1fe91ead2563c00539b4008c0086.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrfKey=TwzBeSszkg9tjKT3u97crNV7yWLVl0iU" onerror="document.forms[0].submit()">
``` 
- اگر مقدار referrer در سمت سرور چک شود و مقدار خالی Referrer را هم قبول کند
  - با اضافه کردن HTML زیر به PoC (همان بخش body)، هدر Referrer را حذف می کنیم تا این بررسی بای پاس شود
```HTML
<meta name="referrer" content="no-referrer">
```
- اگر مقدار referrer در سمت سرور چک شود و درون referrer به دنبال pattern ای از آدرس سایت بگردد 
  - ابتدا PoC را ایجاد کرده و سپس تغییر زیر را در بخش ابتدایی PoC (درون body) اعمال می کنیم
  - از آنجائیکه مرورگرها برای امنیت بالاتر، هدر Referrer را از URL حذف می کنند، برای اطمینان از اینکه URL درون درخواست قرار داده شود، کد زیر را بخش Head اضافه می کنیم
```HTML
<script>history.pushState('', '', '/?ac0a1f761f007f86c0d41ced006e00da.web-security-academy.net')</script>
```
```JAVASCRIPT
Referrer-Policy: unsafe-url
```

## Burp PoC
- Right-click on the Request > Engagement tools > Generate SCRF PoC
- Options > Choose "incluse auto-submit script"
- Click on Regenerate
- Click on Copy HTML
- Upload the HTML file in an exploit server and deliver it to victim

## Mitigation
- Generate an unpredictable token that can not be used (large value, random, unique)
- Embed the token in the HTML page in a hidden form
- Verify the token when the form is submitted 
- If there is no CSRF token, use *"Current Password"* fro change password
