# Information Gathering

## IP address
   - ```ping``` command

## Domain name info
   - [whois](https://whois.domaintools.com/)

## Technologies used
   - Web server
     - [Netcraft](https://sitereport.netcraft.com/?url=)
     - nc
       - ```nc www.nooranet.com 80```
       - ```GET / HTTP/1.1```
     - Check Response with Burp
     - ```nmap -p 80,443 -sV abc.ed```  
   - Web application
     - Review page source
     - Check cookies
     - Browser addon (Wappalyzer)  

## DNS records
   - [Robtex](https://www.robtex.com/)
   - nslookup
   - [dig](/Tools/dig.md)
   - Reverse IP lookup zone
     - ```for ip in $(seq 1 254); do host 193.8.139.$ip; done | grep -v "not found"``` 
   - [CRT.sh](https://crt.sh/)

## Other websites on the same server
   - [Robtex](https://www.robtex.com/)
   - [Bing](https://www.bing.com/): ```ip:[target ip]```
   - [YouGetSignal](https://www.yougetsignal.com/): Reverse IP Domain Check
   - [FomainTools](https://reverseip.domaintools.com/): accept IP range 

## Subdomains
   - [knock](https://github.com/guelfoweb/knock)
   - [Google](https://www.google.com/): ```site:nasa.gov -site:www.nasa.gov```
   - [Netcraft](https://searchdns.netcraft.com/): site ends with *".mci.ir"* 
   - Dictionary attack: ```for ip in $(cat subdomain.txt); do host $ip.megacorpone.com; done | grep -v "not found"```

## Unlisted files, directories
- Burp
  - Intruder
    - ```GET /$$ HTTP/1.0``` 
    - Wordlist: /usr/share/wordlists/dirb/common.txt
  - Crawling
    - Dashboard => Live passive crawl from proxy
    - Navigate to "Target" tab and the sitemap of the web application will be displayed
   
- Dirbuster
  - Enter the target URL and select “Auto Switch” in Work Method
  - Click on the “Browse” button to select the wordlist => /usr/share/wordlists/dirb/common.txt
  - Click on the Start button
  - Click on the "Results - List Views" button in order to see the results
  - Click on the "Results - Tree View" to get the results in tree format
  - Increase the threads up to 30 to get results faster. Enter 30 in the "Current number of threads" & Click on the Change button
  - Click on the dropdown button to get the contents inside the data directory
   
- Gobuster
  - ```gobuster dir -u http://192.156.207.3 -w /usr/share/wordlists/dirb/common.txt```
  - ```gobuster dir -u http://192.156.207.3 -w /usr/share/wordlists/dirb/common.txt -b 403,404```
    - ```-b```: specify the status codes which has to be ignored
  - ```gobuster dir -u http://192.156.207.3 -w /usr/share/wordlists/dirb/common.txt -b 403,404 -x .php,.xml,.txt -r```
    - ```-x```: find the files which have the specified extensions
    - ```-r```: follow any redirects or 302 status code pages 
  - ```gobuster dir -u http://192.156.207.3/data -w /usr/share/wordlists/dirb/common.txt -b 403,404 -x .php,.xml,.txt -r``` 
    - start scanning from the ```/data``` directory
   
 - Opendoor
   - ```opendoor --host http://192.156.207.3 -s directories -w /usr/share/wordlists/dirb/common.txt```
     - ```--host```: define the target URL
     - ```-s```: specify the scan mode which in this case is directories 
     - ```-w```: define the wordlist with its full path 
     - The blue links represent the redirection (status code 302) whereas the green represents
   - ```opendoor --host http://192.156.207.3 -s directories -w /usr/share/wordlists/dirb/common.txt -e php,txt.xml```
     - ```-e```: specify the file extensions which has to be checked on the target
   - ```opendoor --host http://192.156.207.3 -s directories -w /usr/share/wordlists/dirb/common.txt -e php,txt.xml --prefix data/```
     - ```--prefix```: append the target URL with the specified directory
   - ```opendoor --host http://192.156.207.3 -s directories -w /usr/share/wordlists/dirb/common.txt -e php,txt.xml --prefix data/ --reports html```
     - ```--reports```: reporting format to be used (json, html, text)
   
 - ZAProxy
   - Click on "Manual Explore", enter the target IP address in the Input field and click on "Launch Browser"
   - Click on "Continue to your target", Upon visiting the website, the website will be added to the Site map
   - Right Click on the target site under Sites, navigate to Attack and click on "Forced Browse Directory"
   - A new tab will appear on the bottom window: "Forced Browse"
   - Click on the List dropdown and select the common.txt wordlist
   - Start the attack by clicking the play button
   - The scan will start and the found directories and files will be added to the sitemap
   - After the scan completes, expand the directories. Check the files in data folder
 - [dirb](/Tools/dirb.md)
 - [PentestTools](https://pentest-tools.com/) 
 - Interesting files: 
   - */phpinfo.php*
   - */robots.txt*
