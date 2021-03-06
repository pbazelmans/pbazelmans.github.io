---
layout: post
title:  "Generate a CSR with OpenSSLlll"
subtitle: A simple way to generate a CSR
tags: [openssl, csr]
cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/code.png
---
Here a short post about how to generate a **certificate signing request** with the use of an OpenSSL script. After the CSR is generated, it can be signed by a Microsoft CA Server.

In his example I have used a san.cfg file, which give me the ability to add SAN names in de CSR.

{% highlight ruby %}
#!/bin/sh
# I always forget the syntax
# Can also substitute sha256 and rsa:2048, if needed

openssl req -config san.cfg -nodes -sha512 -new -newkey rsa:4096 -keyout private.key -out server.csr
{% endhighlight %}

Here the content of the `san.cfg` file.

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

If you want to sign the CSR with your own Microsoft CA server, you can use `certutil.exe` with the following syntax.

{% highlight ruby %}
certreq -submit -attrib "CertificateTemplate:CertTemplateName" server.csr server.cer server.p7b server.pfx
{% endhighlight %}