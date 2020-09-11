---
layout: post
title:  "Generate a CSR with OpenSSL"
tags: [openssl, csr]
---
Here a short post about how to generate a **certificate signing request** with the use of an OpenSSL script. After the CSR is generated, it can be signed by a Microsoft CA Server.

In his example I have used a san.cfg file, which give me the ability to add SAN names in de CSR.

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
countryName                = Country Name (2 letter code)
stateOrProvinceName        = State or Province Name (full name)
localityName               = Locality Name (eg, city)
organizationName           = Organization Name (eg, company)
commonName                 = Common Name (e.g. server FQDN or YOUR name)
[ req_ext ]
subjectAltName = @alt_names
[alt_names]
DNS.1   = aaa.example.com
DNS.2   = bbb.example.com
{% endhighlight %}

## Enroll in Microsoft CA server
certreq -submit -attrib "CertificateTemplate:_WebServer2Years" server.csr server.cer server.p7b server.pfx