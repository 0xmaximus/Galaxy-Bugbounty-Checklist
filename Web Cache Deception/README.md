## Web Cache Deception Summary

Web Cache Deception (WCD) is an attack in which an attacker deceives a caching proxy into improperly storing private information sent over the internet and gaining unauthorized access to that cached data. It was proposed by Omer Gil, a security researcher in 2017.

## How does web cache deception attack work?

When the browser makes a request to a website, the connection usually passes through the CDNs (Content Delivery Network).

CDNs are a geographically distributed network of proxy servers and their data centers, which caches the local copies of web content to provide faster access to the users by reducing their network latency, and thus reducing the load on web servers.

Caching servers have no safeguards to authenticate users and prevent information, and it only stores non-user specific static or public content. And all the user-specific dynamic contents get routed to the main servers of the website or service a user interacts with.

The web caching deception (WCD) attack works by the technique of path confusion attack.

It manipulates the URL path by which the cache server is forced to store, and the sensitive data gets revealed as public content.


<p align="center">
  <img
    src="https://github.com/cyspad/security-mindmaps/blob/88b8e4be9bfed88a2f2b542a97970619735b02eb/wcd-background.png"
  >
</p>


## Simple Manual Testing
    1) Frist Check Cacheing with non-exits static files in paths (https://target.tld/profile/payments/nonexistent.css)
    2) If the response is not 404 and payments information is still displayed, the attack scenario is applicable
    

## How to test Web Cache Deception with BurpSuite

    1) Install Web Cache Deception Scanner Extension in Burp Suite
    2) Select Target (Target Tab in Burp Suite)
    3) Right Click --> Extensions --> Web Cache Deception Scanner -> Web Cache Deception Test
    
<br>
<p align="center">
<img
src="https://github.com/cyspad/my-share-image-repo/blob/04ab3d0d2e241ca48c44d9d650fec25686b7a9e9/Web-Cache-Deception%20Scanner.jpg"
>
</p>


Happy Hunting ;)
