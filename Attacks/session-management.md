# Session Management

## Bypassing Session Management Schema
- Modify SessionId value in Cookie, SessionId types are:
  - Cleartext
    - admin=1  
  - Hash
    - Fixed length 
      - MD5: 32 Bytes
      - SHA: 128 Bytes
  - Encode
    - Base64: ```=``` & ```.``` & ```\```
    - ```echo bG9nZ2VkaW49VHJ1ZTthZG1pbj1GYWxzZQ== | base64 -d```
  - Encryption 
    - Variable length 

## Cookie Attributes
- ```Cookie: nooranet=123456; secure; HttpOnly```

## Session Fixation
- SessionId must destroyed after logout

## Logout Functionality
- Use *Back* button to login, after logout

## Session Puzzling
- Use OTP for critical functionality, to prevent abuse with sessionId
