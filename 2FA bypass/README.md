### 1.Response manipulation

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
        
        Try to navigate to the page which comes after 2FA or any other authenticated page of the application. If there is no success, change the refer header to the 2FA page URL.             This may fool application to pretend as if the request came after satisfying 2FA Condition

### 4.Developer’s Check

        1) Right click on submit button (continue or etc ...)
        2) Inspect element
        3) Fuctions like “checkOTP(event)”
        4) Type function in console

[https://shahjerry33.medium.com/otp-bypass-developers-check-5786885d55c6](https://shahjerry33.medium.com/otp-bypass-developers-check-5786885d55c6)


### 5.Host Header Poisoning

        A common way to implement password reset functionality is to generate a secret token and send an email with a link containing the token. 
        If an attacker is able to change the host header they can then redirect the token to their website or server which can lead to password reset poisoning

        1) intercept the request and change the Host header to any website.
        2) Now check your mail if you have received the password reset link and contains bing.com in the url. If it does then its vulnerable to password reset poisoning

        Changing the host directly to any website doesn’t work most of the time. You can try to bypass this with below methods. **


        Add X-Forwarded-Host header :

        Host: redacted.com

        X-Forwarded-Host: bing.com

        or :

        Host: bing.com

        X-Forwarded-Host: redacted.com

[https://shahjerry33.medium.com/otp-bypass-developers-check-5786885d55c6](https://medium.com/@abhishake21/password-reset-poisoning-to-ato-and-otp-bypass-1a3b0eba5491)

### 6.Session permission

        Using the same session start the flow using your account and the victims account. When reaching the 2FA point with both account, complete the 2FA with your account but do not         access the next part. Instead of that, try to access to the next step with the victims account flow. If the back-end only set a boolean inside your sessions saying that you           have successfully passed the 2FA you will be able to bypass the 2FA of the victim.

### 7.Reusing token
        
        Maybe you can reuse an already used token inside the account to authenticate.

### 8.Sharing unused tokens

        Check if you can get for your account a token and try to use it to bypass the 2FA in a different account.
        
### 9.Reveal any kind of OTP codes in the response

        Is the token leaked on a response from the web application?

### 10.OTP bypass by Brute force (no Rate Limit)

        Burp Suite intruder

### 6.Change request method
