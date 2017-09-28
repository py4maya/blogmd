---
title: bpf 语法
date: 2017-09-26 10:36:01
---
### Berkeley Packet Filter (BPF) syntax

The *expression* consists of one or more *primitives*. Primitives usually consist of an *id* (name or number) preceded by one or more qualifiers.  There are three different kinds of qualifier:

#### type
qualifiers say what kind of thing the id name or number refers to.  Possible types are *host* ,  *net*  , *port*, and *portrange* .  
E.g., `host foo', `net 128.3', `port 20', `portrange 6000-6008'.  
If there is no type qualifier, *host* is assumed.

#### dir
qualifiers specify a particular transfer direction to and/or from *id* .  Possible directions are *src* , *dst*  , *src or dist* and *src and* *dst*  .
E.g., `src foo', `dst net 128.3', `src or dst port ftp-data'.
If there is no dir qualifier, src or dst is assumed.

For some link layers, such as SLIP and the ``cooked'' Linux capture mode
used for the ``any'' device and for some other device types, the *inbound</b> and <b>outbound</b> qualifiers can be used to specify a desired direction.

#### proto
qualifiers restrict the match to a particular protocol.
Possible
protos are: <b>ether</b>, <b>fddi</b>, <b>tr</b>, <b>wlan</b>, <b>ip</b>, <b>ip6</b>, <b>arp</b>, <b>rarp</b>, <b>decnet</b>, <b>tcp</b> and <b>udp</b>.
E.g., `ether src foo', `arp net 128.3', `tcp port 21', `udp portrange 7000-7009'.
If there is no proto qualifier, all protocols consistent with the type are assumed.

E.g., `src foo' means `(ip or arp or rarp) src foo'
(except the latter is not legal syntax), `net bar' means `(ip or
arp or rarp) net bar' and `port 53' means `(tcp or udp) port 53'.

[bpf详细介绍](http://biot.com/capstats/bpf.html)
