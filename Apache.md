## Purpose: This will hold the fancy commands/info/thoughts and such I discover about Apache and Linux commands releated (From Research)

---

# Links
---
https://www.digitalocean.com/community/tutorials/how-to-set-up-an-apache-mysql-and-python-lamp-server-without-frameworks-on-ubuntu-14-04

This shows how to install basic appache... it still needs to be stigged and securerd with cert as well as run on non-default port, but this is a good start, however for this case I don't think I would use a package manager... probs will be easier to download source and build??, that depends on how the platform is going to run... So either source or package manager... thinking about it more, if this is a base image for a container it should install no poblem with a package manager (internal or internet based), so I will keep it simple and use that (realistically we should maintain our own internal rpm/yum/asnisble server so technically we will be able to control the version and packages we download/use) 

With that said, bash might be the best way to build this since I don't want to waste too much time figuring out how to interface a python lib with a yum server or other package manager. Plus, this is appache, not the hardest thing on the block to automate
---


# Tutorials

## How to use openssl to verify cert and how to test if webserver is running with bash:

https://superuser.com/questions/272265/getting-curl-to-output-http-status-code

https://stackoverflow.com/questions/7885785/using-openssl-to-get-the-certificate-from-a-server

https://www.ibm.com/support/pages/openssl-commands-check-and-verify-your-ssl-certificate-key-and-csr

- Basically use curl to grab the http header status
- And then download the cert and compare it to the one you uploaded


- Random notes about this:
```
172.17.0.2
curl -k https://172.17.0.2:5555
curl -v -k https://127.0.0.1:443

curl -sI -k https://127.0.0.1:443 | head -1
HTTP/1.1 200 OK


openssl s_client -connect 127.0.0.1:443 2>/dev/null </dev/null |  sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' | openssl x509 -in - -text -noout

TEST=($(cat server.crt | sha256sum))

```
