## 1) Internal IP Address disclosure

On any IIS server where you get a 302 you can try stripping the Host header and using HTTP/1.0 and inside the response the Location header could point you to the internal IP address:

Response disclosing the internal IP:

```
GET / HTTP/1.0       

HTTP/1.1 302 Moved Temporarily
Cache-Control: no-cache
Pragma: no-cache
Location: https://192.168.5.237/owa/
Server: Microsoft-IIS/10.0
X-FEServer: NHEXCHANGE2016

```

## 2) Execute .config files

The web.config file plays an important role in storing IIS7 (and higher) settings. It is very similar to a .htaccess file in Apache web server. Uploading a .htaccess file to bypass protections around the uploaded files is a known technique. Some interesting examples of this technique are accessible via the following GitHub repository: https://github.com/wireghoul/htshells

In IIS7 (and higher), it is possible to do similar tricks by uploading or making a web.config file. A few of these tricks might even be applicable to IIS6 with some minor changes. The techniques below show some different web.config files that can be used to bypass protections around the file uploaders.

### Defaults extensions

* PHP Server
    ```powershell
    .php
    .php3
    .php4
    .php5
    .php7

    # Less known PHP extensions
    .pht
    .phps
    .phar
    .phpt
    .pgif
    .phtml
    .phtm
    .inc
    ```
* ASP Server
    ```powershell
    .asp
    .aspx
    .config
    .cer and .asa # (IIS <= 7.5)
    shell.aspx;1.jpg # (IIS < 7.0)
    shell.soap
    ```
* JSP : `.jsp, .jspx, .jsw, .jsv, .jspf, .wss, .do, .action`s
* Perl: `.pl, .pm, .cgi, .lib`
* Coldfusion: `.cfm, .cfml, .cfc, .dbm`

