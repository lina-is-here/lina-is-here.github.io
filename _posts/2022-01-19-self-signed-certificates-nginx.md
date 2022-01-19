---
layout: post
title:  "How to generate a self-signed SSL certificate and use it in Nginx in docker-compose"
date:   2022-01-19
tags: linux, cli, openssl, ssl, nginx, docker-compose
---

While implementing barcode scanning using [QuaggaJS][quaggajs-repo], I realized that I have to have a secure origin 
in order for the live scanning feature to work on mobile devices. Exactly, as written in QuaggaJS README file:
{% highlight text %}
Important: Accessing getUserMedia requires a secure origin in most browsers, 
meaning that http:// can only be used on localhost. 
All other hostnames need to be served via https://. 
{% endhighlight %}

The simplest thing to do for the development environment is to generate a self-signed certificate.

<!--more-->

### Generating self-signed certificate

Only one command is needed:

{% highlight sh %}
$ openssl req -newkey rsa:2048 -nodes -keyout domain.key -x509 -days 365 -out domain.crt
{% endhighlight %}

The command will prompt for the information to include in the certificate request. You can fill them as you wish,
only the `Common Name (eg, fully qualified host name)` is required to generate the certificate.

### Nginx config

The following should be in the nginx config:

{% highlight nginx %}
server {
    listen      443 ssl http2;
    server_tokens off;
    ssl_certificate ssl/domain.crt;
    ssl_certificate_key ssl/domain.key;
}
{% endhighlight %}

Check the path for `ssl_certificate` and `ssl_certificate_key` in your configuration.
The suggested path works if the certificates directory is mounted to `/etc/nginx/ssl` as described below in 
docker-compose config.

### Docker-compose config

In order for the certificate to work, add the following volume (assuming `domain.crt` and `domain.key` are located in
 the `certs` directory at the root of the project) to the nginx container:

{% highlight yml %}
volumes:
  - ./certs:/etc/nginx/ssl
{% endhighlight %}
    
And don't forget to change the port:
{% highlight yml %}
ports:
  - 8443:443
{% endhighlight %}


[quaggajs-repo]: https://github.com/serratus/quaggaJS
