---
layout: post
title:  "Generate a CSR with OpenSSL"
tags: [openssl, csr]
---
I work a lot with SSL certificates, so OpenSSL in Cygwin is one of the standard tools I use on a daily base. I have collected the commands which I use often on this page.

{% highlight ruby %}
#!/bin/sh
## I always forget the syntax
## Can also substitute sha256 and rsa:2048, if needed
openssl req -config san.cfg -nodes -sha512 -new -newkey rsa:4096 -keyout private.key -out server.csr
{% endhighlight %}

## san.cfg

Here the content of san.cfg

{% highlight ruby %}
[ req ]
default_bits       = 2048
distinguished_name = req_distinguished_name
req_extensions     = req_ext
[ req_distinguished_name ]
countryName                = XX
stateOrProvinceName        = XX
localityName               = any-location
organizationName           = any-org
commonName                 = server.anydomain.com
[ req_ext ]
subjectAltName = @alt_names
[alt_names]
DNS.1   = server.anydomain.com
{% endhighlight %}

## Enroll in Microsoft CA server
certreq -submit -attrib "CertificateTemplate:_WebServer2Years" server.csr server.cer server.p7b server.pfx