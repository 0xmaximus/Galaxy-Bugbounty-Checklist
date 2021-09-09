# SSRF
> Server Side Request Forgery (SSRF) is simply an attack where the server will make a request (act like a proxy) for the attacker either to a local or to a remote source and then return a response containing the data resulting from the request.

> We can say that the concept of SSRF is the same as using a proxy or VPN where the user will make a request to a certain resource, then the proxy or VPN Server will make a request to that resource, then return the results to the user who made the request.

From SSRF, various things can be done, such as:

- Local/Remote Port Scan
- Local File Read (using file://)
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
```
- If the web application is deployed on Windows Server, to access files using the `file:///` protocol, you can use `file:///<drive_letter/PathToFile`. Example : `file:///d:/hello.txt`

## Interacting With Internal Service
> The real power of SSRF is where the attacker can interact with the internal application/service/network in Local Network, imagine if there is a vulnerable application/service in the internal network where the attacker cannot reach the application/service because it is on a different network, but if there is SSRF vulnerability the Attacker might be able to do that.
