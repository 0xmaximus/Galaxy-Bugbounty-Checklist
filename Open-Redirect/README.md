# Open Redirect

If you are reading this & thinking, what are open url redirects?, then simply put open redirects are urls such as 
`https://www.example.com/?go=https://www.google.com/`, which when visited will go from example.com -> google.com.

The endpoint you are investigating will contain some type of redirect parameter or URL which will redirect upon success.

Imagine you are attempting to login to example.com and the endpoint you are on is, example.com/login.php?returnUrl=/help. 
Your job as a hacker is to then see if you can redirect to your site after logging in.

Typically companies/bug bounty programs consider open redirects as low impact, so this means that not only are they easy to find, 
but if any filtering does exist it is usually relatively easy to bypass. 

It is a good idea to hold onto some open url redirects when hunting as these can be used to bypass server side request forgery (SSRF) 
filters and you can turn your redirect into a high impact bug. 

With that said open url redirects aren't only used for bypassing SSRF filters. Let's explore what can be done!

## Finding open url redirects

To begin with let's start with finding an open url redirect and explore common places to look for them. 

I will always start with dorking since Google knows more about a target than me, so let's see what google knows first by using site:example.com and then playing with the following dorks: (and also try come up with your variants, you never know what you will discover!)

    inurl:go
    inurl:return
    inurl:r_url
    inurl:returnUrl
    inurl:returnUri
    inurl:locationUrl
    inurl:goTo
    inurl:return_url

None found? Ok no problem, lets start using their site and look at common places. From my experience most sites usually redirect the user after some type of action such as logging in, logging out, password change, signup. The parameter can usually be found in the URL, or sometimes you need to hunt in .js files for referenced parameters. It is highly likely that the login page will handle some type of redirect parameter so make sure to look deeply!. Once you have discovered one parameter name used for redirecting then typically developers will re-use code/parameter names throughout so test this parameter on every endpoint you discover.
Using the open url redirect.

By this time we would of found atleast one open url redirect, and if not, get back to hunting! ;) So once we do actually have a valid bug, what can we do? Below are the most common things I will try with an open url redirect:

## 1.Grab tokens via mis-configured apps/login flows

Imagine the following scenario. When logging into redacted.com you notice in the url `returnto=/supersecure`, and after successfully logging in,
the website redirects to `/supersecure?token=39e9334a` with your login token, and then to the main website.

So this means if we set it to `returnto=//myevilsite.com` and send our victim the login url, if the website was vulnerable upon the user successfully logging in,
the user will be redirected to our site which enables the attacker to steal their login token.

Check the Referer header for leaks as well as playing with various characters to check how they handle it server-side. You can view an example of this here.

- - - -

## [2.Bypassing blacklists for SSRF/RCE](https://github.com/0xmaximus/Galaxy-Bugbounty-Checklist/blob/main/SSRF/README.md#bypassing-ssrf-filters-via-open-redirection)

Some websites will blacklist some requests to only allow requests to theirsite.com or `/localendpoint`. 

Armed with an open redirect on their domain, depending on their framework and how they handle redirects, you can sometimes bypass their blacklsit and achieve [SSRF](https://github.com/0xmaximus/Galaxy-Bugbounty-Checklist/tree/main/SSRF) or RCE (depending on the circumstances). 

Imagine you have an endpoint which takes an `?url=` parameter but it will only allow you to input local endpoints, such as `/example`. Now imagine you also have an open redirect at `/redirect?goto=//127.0.0.1/`. 

Pointing `?url=` to this endpoint may cause their web application to trust the user input (since it is pointing to local endpoint), but process the redirect & show you sensitive information. Remember this is a redirect from their domain which means you have level of trust via their domain (think if you need the Referrer header to contain their domain, now you can)

- - - -

## 3.XSS via javascript:alert(0)

This does not work everytime and is dependent on how they are redirecting. If it's a 302 redirect then it will not work, but if they are redirecting via javascript then it will work.

```javascript
<script>
top.location.href='reflectedhere';
</script>
```

- When looking for these types of XSS vulnerabilities (via redirect), always look for strings such as window.location, top.location.href, location.. 
  If you see a redirect via these methods then you will be able to achieve XSS as long as no filtering is stopping you.

- I run into filters trying to prevent third party redirects all the time. However before even thinking about trying to bypass the filter, one of the most common issues researchers run into when testing login flows chained with an open url redirect is not encoding the values correctly. For example, `https://example.com/login?return=https://mysite.com/`.

- The website/browser may get confused with how the return parameter is formatted so it always good to try just normal encoding, and failing that, double encoding. See below for an example:

```javascript
https://example.com/login?return=https://example.com/?returnurl=https%3A%2F%2Fwww.google.com%2F

https://example.com/login?return=https%3A%2F%2Fexample.com%2F%3Freturnurl%3Dhttps%253A%252F%252Fwww.google.com%252F
```

> Notice we've got two redirects in one? We need to double encode the last redirect so the browser decodes it last and redirects. Sometimes if you don't encode properly the browser won't redirect correctly.


- At the end we some amazing payloads here:

```javascript
\/yoururl.com 
\/\/yoururl.com 
\\yoururl.com 
//yoururl.com 
//theirsite@yoursite.com 
/\/yoursite.com 
https://yoursite.com%3F.theirsite.com/ 
https://yoursite.com%2523.theirsite.com/ 
https://yoursite?c=.theirsite.com/ (use # \ also) 
//%2F/yoursite.com 
////yoursite.com 
https://theirsite.computer/ 
https://theirsite.com.mysite.com 
/%0D/yoursite.com (Also try %09, %00, %0a, %07) 
/%2F/yoururl.com 
/%5Cyoururl.com 
//google%E3%80%82com

```

- You can also use [this](https://tools.intigriti.io/redirector/) link to generate open redirect payloads
