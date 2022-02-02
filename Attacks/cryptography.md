# Cryptography

## Character Sets
- ASCII: 7 bits (128 characters)
- UTF8: 8 bits (256 characters)
- UTF16/32: 16 bits 

## Hash
- MD5 : 32 Byte
- SHA1 : 64 Byte
- SHA2 : 128 Byte

## Symbols
- ```or```     =>    ||
- ```and```    =>    &&
- ```space```  =>    +

## Encoding
- URL Encode : ```%```
  - ```&&```    =>    %26%26
  - ```||```    =>    %7c%7c
  - ```space``` =>    %20
  - ```‘```     =>    %27
  - ```>```     =>    %3E
  - ```<```     =>    %3C
  - ```;```     =>    %3B
- Base64 : ```==``` ```\``` ```.```
- Gzip : geometrical shape
- Hex : ```0~9 a~F```

# Symetrical Encryption
- **Stream Cipher**
  - Example 
    - RC4
    - A5 (Telecommunication)
    - E0 (Bluetooth)
- **Block Cipher**
  - Modes
    - ECB (ECB Block Shuffling)
    - CBC (CBC Bit Flipping & Oracle Padding)
    - CRT
  - Example
    - DES
    - 3DES
    - AES 


## Attacks (SSLv3 - TLS1.1)
- Heartbleed 
  - Metasploit: openssh_hearbleed
    - Auxiliary: ```use auxiliary/scanner/ssl/openssh_hearbleed```
    - Exploit: ```set verbose true```
- Poodle
  - Oracle Padding

## SSLyze
- Usage
  - ```sudo sslyze --regular ib.bki.ir```
  - Find ```vulnerable``` in output
