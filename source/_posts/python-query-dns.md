---
title: python解析指定dns附加timeout
date: 2018-07-16 11:20:43
tags: [dnspython]
categories:
---

```python

import dns.resolver

Resolver = dns.resolver.Resolver()
Resolver.timeout = 0.2
Resolver.lifetime = 0.2
Resolver.nameservers = ["10.210.3.100"]

def Query(domain,type_="A"):
   return Resolver.query(domain,type_).response.answer

def foolQueryIps(domain,default=[]):
    queryIps = []
    try:
        result = Query(domain)
        for item in result:
            for item_ in item:
                queryIps.append(item_.address)
    except Exception,ex:
        queryIps = default
    return queryIps

if __name__ == '__main__' :
    domain = "slavemysql.newaaa.sec.intra.sina.com.cn"
    print foolQueryIps(domain)

```
