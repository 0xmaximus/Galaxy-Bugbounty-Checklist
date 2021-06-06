### 1.Response manipulation </br>

        1) Enter correct OTP
        2) Intercerp response
        3) Enter wrong OTP
        4) Intercerp response and chaneg it with correct response




### 2.Direct bypass

        1) just try to access the next endpoint directly (you need to know the path of the next endpoint). 
        2) If this doesn't work, try to change the Referrer header as if you came from the 2FA page.
        
        example :
        site.com/login/otp_verification
        site.com/login/new_password


### 3.Referrer Check Bypass
        
        Try to navigate to the page which comes after 2FA or any other authenticated page of the application.
        If there is no success, change the refer header to the 2FA page URL.
        This may fool application to pretend as if the request came after satisfying 2FA Condition


### 4.Developer’s Check

        1) Right click on submit button (continue or etc ...)
        2) Inspect element
        3) Fuctions like “checkOTP(event)”
        4) Type function in console

[https://shahjerry33.medium.com/otp-bypass-developers-check-5786885d55c6](https://shahjerry33.medium.com/otp-bypass-developers-check-5786885d55c6)<br><br/>


### 5.X-Forwarded-For

        add X-Forwarded-For: 127.0.0.1 in request
        If it did not work try :
        X-Originating-IP
        X-Forwarded-Fo
        X-Remote-IP
        X-Remote-Addr
        X-Client-IP
        X-Host
        X-Forwared-Host
        

### 6.Session permission

        Using the same session start the flow using your account and the victims account. 
        When reaching the 2FA point with both account, complete the 2FA with your account but do not access the next part.
        Instead of that, try to access to the next step with the victims account flow.
        If the back-end only set a boolean inside your sessions saying that you have successfully passed the 2FA you will be able to bypass the 2FA of the victim.

### 7.Reusing token
        
        Maybe you can reuse an already used token inside the account to authenticate.


### 8.Sharing unused tokens

        Check if you can get for your account a token and try to use it to bypass the 2FA in a different account.
        
      
### 9.Reveal any kind of OTP codes in the response

        Is the token leaked on a response from the web application?


### 10.OTP bypass by Brute force (no Rate Limit)

        Burp Suite intruder


### 11.CSRF/Clickjacking

        Check if there is a CSRF or a Clickjacking vulnerability to disable the 2FA.


### 12.Bypass 2FA arbitrary input

        null 
        000000
        0
        ASADSas

### 13.Change request method



[More good stuff](https://book.hacktricks.xyz/pentesting-web/2fa-bypass)
