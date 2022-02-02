# API

## Concept
- در API ها Content-type به صورت application/json می باشد.

## Web service attack types
- SQL Injection
- Command Injection
- Code Execution
- XXE
- Broken Access Control
- XML Bombing

## Recon
  - ```site:ir intitle:wsdl | wadl```
  - ```site:ir inurl:?wsdl```
  - ```site:ir inurl:asmx?```

## SoapUI
- **Rest**
  - Define URI
  - Select Method
  - Paste JSON in left-bottom part

- **Soap**
  - File > New Soap Project
    - Project Name : shaparak
    - Initial WSDL : https://sep.shaparak.ir/payments/referencepayment.asmx?WSDL
  - Open functions and check Requests
    - Replace ? with parameters
  - Check Response in right window

- **Load Test**
  - Right-click on Request & select "Add to TestCase"
  - Right-click on TestCase & select "New LoadTest"

- **Integrate with Burp**
  - SOAPUI > File > Preferences > Proxy Setting: ```Manual```

## Buggy Framework
- WebGoat
