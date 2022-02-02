# Search Engine

## [Google](https://www.google.com/)
- site:gm.com -s -"search-careers" -www ext:jsp
- site:gm.com -s -"search-careers" -www inurl:admin
- site:gm.com -s -"search-careers" -www inurl:login 
- site:gm.com -s -"search-careers" -www intitle:admin

## [Shodan](https://www.shodan.io/)
- ```hostname:gm.com```
- ```net:"193.8.138.0/24" port:443```
- ```net:"193.8.138.0/24" apache```
- ```net:"193.8.138.0/24" iis```
- ELK Misconfiguration: ```country:ir port:5601```
- MongoDB Misconfiguration: ```country:ir port:27017```

## [Censys](https://censys.io/)
- ```location.country.code:US and tags:scada```
