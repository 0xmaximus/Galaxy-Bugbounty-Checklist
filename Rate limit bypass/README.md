### 1. Using different parameter, suppose rate limit is on signup, try to use:

    sign-up, Sign-up, SignUp
![image](https://user-images.githubusercontent.com/63053441/120425626-d06f0180-c383-11eb-9fec-22fdfc9c474e.png)<br></br>
![image](https://user-images.githubusercontent.com/63053441/120425794-16c46080-c384-11eb-94db-643501b2eae9.png)

[https://huzaifa-tahir.medium.com/methods-to-bypass-rate-limit-5185e6c67ecd](https://huzaifa-tahir.medium.com/methods-to-bypass-rate-limit-5185e6c67ecd)


### 2.Rate Limit Bypass Headers:

Most Application’s use X-Forwarded-For common method for identifying the originating IP address of the client.
We All know that using X-Forwarded-For: IP Header Can sometime’s Bypass Ratelimit Protection.
Sometimes Adding Two Times X-Forwarded-For: IP Header Instead of One time Can Bypass Ratelimit Protection

    X-Forwarded: 127.0.0.1
    X-Forwarded-By: 127.0.0.1
    X-Forwarded-For: 127.0.0.1
    X-Forwarded-For-Original: 127.0.0.1
    X-Forwarded-For-Ip: 127.0.0.1
    X-Forwarded-Host: 127.0.0.1
    X-Forward-For: 127.0.0.1
    Forwarded: 127.0.0.1
    Forwarded-For: 127.0.0.1
    Forwarded-For-Ip: 127.0.0.1
    X-Originating-IP: 127.0.0.1
    X-Remote-IP: 127.0.0.1
    X-Remote-Addr: 127.0.0.1
    X-Client-IP: 127.0.0.1
    X-Host: 127.0.0.1
    X-True-Ip: 127.0.0.1

    IP Header 2x times Instead of One time. (Tip from Kiraak Boy)
![image](https://user-images.githubusercontent.com/63053441/120426382-3d36cb80-c385-11eb-8da7-63a1bee081a0.png)

    Sometimes, it is showing 20 Request per account, you can bypass it by using different IP after 20 attempts
![image](https://user-images.githubusercontent.com/63053441/120426528-838c2a80-c385-11eb-8d79-3927c4c031c6.png)<br></br>
![image](https://user-images.githubusercontent.com/63053441/120427217-c39fdd00-c386-11eb-9e48-6e9079d50fa6.png)



### 3.Rate limit on OTP sms.
        
    1) Capture the request. 
    2) Remove the country code +91 to [ ] 
    3) Modify the number from xxxxx-xxxxx to +91 xxxxx-xxxxx
        
### 4.Add this characters after the email or mobile

    %00, %0d%0a, %09, %0C, %20(white-space value), %0
        
![image](https://user-images.githubusercontent.com/63053441/120427626-7708d180-c387-11eb-8db2-28e3d438313e.png)

        
        
### 5.Just by adding random parameter (example: ?bypass) on the last endpoint.

![image](https://user-images.githubusercontent.com/63053441/120427012-63a93680-c386-11eb-85a0-ca3079c6ca4f.png)
[https://twitter.com/xchopath/status/1245402225788063744/photo/1](https://twitter.com/xchopath/status/1245402225788063744/photo/1)


### 6.Changing user-agents or/and cookies




