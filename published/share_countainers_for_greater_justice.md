There's a long list of mission critical software not developed by you: Bash, Apache, OpenSSL etc etc. These binaries often end up running as root, with the default options, directly on production servers. This is bad for security, bad for maintainability and downright horrible for resource usage.   

A case in point: for any webserver exposed to the internet, the following are mandatory:

* Use [HTTPS](https://letsecure.me/secure-web-deployment-with-lets-encrypt-and-nginx/)
* Use the most widely accepted version of TLS 
* Never downgrade to an [insecure protocol](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-3566)
* [Update regularly](http://nginx.org/en/security_advisories.html)
* Log errors
* Do not get owned by a script kiddie


Obviously this is a non-comprehensive list, but `sudo apt-get install nginx -y` is typically not going to give you even this basic setup. One problem is you are pinning yourself to the stable version of the package released for your linux distribution. Another is that there's no real community around ubuntu package management. The nginx community is focused on *their* latest stable release, so you need to upgrade your *linux distro* to have a meaningful conversation with them.  
 
Run nginx using a docker image maintained by a sensible community instead:
```bash
$ docker run -v /some/nginx.conf:/etc/nginx/nginx.conf:ro \ 
    gcr.io/google_containers/nginx-slim:0.12
```

I don't have the time or inclination to get into the specifics, or delve into the horrors of stand-alone containers, but I believe this moves the needle ever so slightly towards more maintainable infrastructure. I'll stop preaching now. 



