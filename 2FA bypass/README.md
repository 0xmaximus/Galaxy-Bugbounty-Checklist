### 1.Response manipulation </br>

    1) Enter correct OTP
    2) Intercept response
    3) Enter wrong OTP
    4) Intercept response and chaneg it with correct response


### 2.Status Code Manipulation </br>

    If Status Code is 4xx
    Try to change it to 200 OK and see if it bypass restrictions
    
    
### 3.Direct bypass

    1) just try to access the next endpoint directly (you need to know the path of the next endpoint). 
    2) If this doesn't work, try to change the Referrer header as if you came from the 2FA page.
        
    example :
    site.com/login/otp_verification
    site.com/login/new_password


### 4.Referrer Check Bypass
        
    Try to navigate to the page which comes after 2FA or any other authenticated page of the application.
    If there is no success, change the refer header to the 2FA page URL.
    This may fool application to pretend as if the request came after satisfying 2FA Condition


### 5.Developer’s Check

    1) Right click on submit button (continue or etc ...)
    2) Inspect element
    3) Fuctions like “checkOTP(event)”
    4) Type function in console

[https://shahjerry33.medium.com/otp-bypass-developers-check-5786885d55c6](https://shahjerry33.medium.com/otp-bypass-developers-check-5786885d55c6)<br><br/>


### 6.X-Forwarded-For

    add X-Forwarded-For: 127.0.0.1 in request
    If it did not work try :
    X-Originating-IP
    X-Forwarded-Fo
    X-Remote-IP
    X-Remote-Addr
    X-Client-IP
    X-Host
    X-Forwared-Host
        

### 7.Session permission

    Using the same session start the flow using your account and the victims account. 
    When reaching the 2FA point with both account, complete the 2FA with your account but do not access the next part.
    Instead of that, try to access to the next step with the victims account flow.
    If the back-end only set a boolean inside your sessions saying that you have successfully passed the 2FA you will be able to bypass the 2FA of the victim.

### 8.Reusing token
        
    Maybe you can reuse an already used token inside the account to authenticate.


### 9.Sharing unused tokens

    Check if you can get for your account a token and try to use it to bypass the 2FA in a different account.
        
      
### 10.Reveal any kind of OTP codes in the response

    Is the token leaked on a response from the web application?


### 11.OTP bypass by Brute force (no Rate Limit)

    Burp Suite intruder


### 12.CSRF/Clickjacking

    Check if there is a CSRF or a Clickjacking vulnerability to disable the 2FA.


### 13.Bypass 2FA arbitrary input

    null 
    000000
    0
    ASADSas

### 13.Change request method



[More good stuff](https://book.hacktricks.xyz/pentesting-web/2fa-bypass)
