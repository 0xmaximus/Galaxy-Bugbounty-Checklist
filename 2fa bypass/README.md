## 1.Response manipulation

        1) Enter correct OTP
        2) Intercerp response
        3) Enter wrong OTP
        4) Intercerp response and chaneg it with correct response
        
        

## 2.OTP bypass by Brute force (no Rate Limit) **

        Burp Suite intruder



## 3.Developer’s Check

        1) Right click on submit button (continue or etc ...)
        2) Inspect element
        3) Fuctions like “checkOTP(event)”
        4) Type function in console

[https://shahjerry33.medium.com/otp-bypass-developers-check-5786885d55c6](https://shahjerry33.medium.com/otp-bypass-developers-check-5786885d55c6)



## 4.Host Header Poisoning

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


## 5.Reveal any kind of OTP codes in the response

## 6.Change request method
