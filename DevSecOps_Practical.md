
``` bash
Please attempt the following tasks. If you don’t make it all the way through, we’d still like to see the progress you made and your thought process!
All of the following tasks should be assumed to be referencing a strictly Linux environment. Please do not write any automation for Windows, nor any other operating system.
Please create a public Github repository and upload all artifacts from these tasks.

"Linux": Using Python, bash, or Ansible (your choice), please write automation to lay the groundwork for a "secure Apache webserver".
Your automation should install Apache and any other packages needed in these tasks.
"Deploy a self-signed X.509 public key certificate with the corresponding private key." Both files should be deployed to "/etc/pki/tls/". Please ensure your automation runs without a human-in-the-loop "(i.e. no prompts)", and that the "permissions/ownership on the certificate and private key are restrictive enough to protect the integrity of the webserver."
"Pick any five of the latest DISA Apache 2.4 STIGs and apply them using your automation."

"Docker": Write a Dockerfile to deploy a "containerized Apache webserver". Bonus points if you use "Docker Swarm" for this, but not required!
Host the webserver on a "non-standard port (e.g. not 80/443)".
"Ensure your webserver uses the PKI you created in step #1 to host over TLS."
Deploy the webserver to /var/www/html/devsecops_practical. This webserver only needs to display "“My awesome webpage!”" in basic HTML, nothing fancy.

"Jenkins": Create a "CICD pipeline in Jenkins that automates the deployment of the Apache webserver." Your "Jenkinsfile should be declarative", and should consist of the following stages:
"Stage “Build” – Build your container image."
"Stage “Run” – Run the image as a container."
"Stage “Test” – Inside the running container, verify your webserver is running on the correct port. Also use the “openssl” command to verify you can pull the webserver’s public key certificate. Fail if both are not successful."
Stage “Push” – Login to Dockerhub and push your container image. "You can assume your credentials are already configured within Jenkins, simply referencing them with a variable."

```

## Main Tasks:

**Linux**
- Build Apache Web Server "Securely" (TLS, Certs, Update to date, Stig'ed, etc)
- Automate the Build with Python or Bash (Proabably going to lean more towards python)
- Build and Deploy Self-signed cert (X.509), save to "etc/pki/tls" and make sure you set the correct permissions for certs!!!
- Get and automate the application of 5 of the latest Disa Stigs for apache!! (maybe I will stick to bash...)


**Docker**
- Write a dockerfile to deploy the apache as a container.
- Use Docker Swarm??? (Maybe depending on time contraints)
- Host the webserver on a non-standard port (change in dockerfile)
- put default web page in "/var/www/html/devsecops_practical" it should be a simple html that displays "My awesome webpage!!!"


**Jenkins**
- Use Jenkins to pipeline CICD to automate deployment of Apache webserver container...
- Deploy with the stages mentioned above
- The "Test" Stage looks tricky, need to verify web server is running on port (nc, curl?), also use openssl to pull the public cert
- Also the push stage might require some additonal research... but it seems straightforward in idea 
