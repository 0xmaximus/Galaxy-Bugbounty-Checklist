## What is Sensitive Data Exposure
 Information disclosure, also known as information leakage, is when a website unintentionally reveals sensitive information to its users. Depending on the context, websites may leak all kinds of information to a potential attacker, including:

- Backup and Config Files Data
- API Token's
- Technical details about the website and its infrastructure

The dangers of leaking sensitive user or business data are fairly obvious, but disclosing technical information can sometimes be just as serious. Although some of this information will be of limited use, it can potentially be a starting point for exposing an additional attack surface, which may contain other interesting vulnerabilities. The knowledge that you are able to gather could even provide the missing piece of the puzzle when trying to construct complex, high-severity attacks.

Occasionally, sensitive information might be carelessly leaked to users who are simply browsing the website in a normal fashion. More commonly, however, an attacker needs to elicit the information disclosure by interacting with the website in unexpected or malicious ways. They will then carefully study the website's responses to try and identify interesting behavior. 
<br>
## API Key Disclosure - Hunting

- Hunting with Burp Suite

   - Install [Logger++](https://github.com/PortSwigger/logger-plus-plus) Extention 
   - Paste the ``` Response.Body == /(?i)([a-z0-9]+){0,}((_|-){0,}(\\s){0,})(APIkey|secret|accesstoken|apiToken|localhost)(\\s){0,}(=|:|is|>){1,}/ AND Response.Headers CONTAINS "application/javascript" ``` Regex in Filter Filed
<br> 

## Backup Files - Fuzzing

- [FFUF](https://github.com/ffuf/ffuf) and cyspadSniper Wordlist
  - ffuf -u https://target.com/FUZZ -w cyspadSniper.txt -mc 200

- [FUZZULI](https://github.com/musana/fuzzuli)
  - echo https://target.com|fuzzuli -mt all -w 32 -ex .rar,.zip,.tar,.gzip,.backup,.bak -p

### References:
* https://portswigger.net/web-security/information-disclosure
