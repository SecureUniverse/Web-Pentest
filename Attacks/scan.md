# Scan

## Nikto
- ```nikto -h http://192.230.148.3```
  - The interesting bits of information are highlighted in yellow  
- ```nikto -h http://192.230.148.3/index.php?page=arbitrary-file-inclusion.php -Tuning 5 -Display V```
  - All the requests with the response code 200 are listed in the verbose mode
  - The highlighted line indicates that the LFI vulnerability was detected
- ```nikto -h http://192.230.148.3/index.php?page=arbitrary-file-inclusion.php -Tuning 5 -o nikto.html -Format htm```
  - Saving the scan result as an HTML file

## ZAProxy
- Click on "Manual Explore", enter the target IP address in the Input field and click on "Launch Browser", A browser session will be started with ZAP HUD
- Click on "Continue to your target"
- Browse the site (bWAPP)
- Configuring ZAProxy to use authenticated session
  - Navigate to the sitemap and find the login request
  - Right click on the POST request, navigate to "Include in Context" and select on "Default Context"
  - Click on the Authentication tab under Default Context menu and select "Form-based Authentication" for the selected method
  - Click on the Select button and select the POST login request (The form fields will automatically be filled)
  - Set the Username parameter to "login" and Enter "Login" in the "Regex pattern identified in Logged Out response messages"
  - Click on the Users tab
  - Click on the "Add" button and add a new user with username "bee" and password" bug
  - Click on the "OK" button
  - Click on the User lock icon
  - Right click on the Site (http://192.210.141.3), navigate to "Include in Context" and select on "Default Context" (Session Properties window will appear)
  - Click on the "OK" button. Right click on the Site (http://192.210.141.3), navigate to Attack and select "Spider"
  - A dialog box will appear, select the "bee" user and click on "Start Scan" button
  - Right click on the Site (http://192.210.141.3), navigate to Attack and select "Active Scan"
  - A dialog box will appear, select the "bee" user and click on "Start Scan" button
  - After the scan completes, click on the "Alerts" tab

# Acunetix
- [Reference](https://www.programmersought.com/article/84304947552/)
- Install
  - ```acunetix_13.0.200217097_x64_.sh```
- Run
  - Insert "https://kali:3443/" in browser
- Config
  - ```sudo cp -rf wvsc /home/acunetix/.acunetix/v_200217097/scanner/```
  - ```sudo cp -rf license_info.json /home/acunetix/.acunetix/data/license/```
- Change Email & Password
  - ```./change_credentials.sh```

# [wpscan](https://github.com/wpscanteam/wpscan)
