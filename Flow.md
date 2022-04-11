## Purpose: This will manage the flow of this project


## START

---

## 4/7/2022

- Begining research phase and ideas on how to approach, all of this appears to be begginer in form, but requires knowledge of many different items

- Idea layout before research:
  1. Build Linux with Apache (try to automate with python, but fallback on bash if too much time is wasted)
  2. Turn into docker image
  3. Jekins it out

- Rough mind map on process

--> Use Linux VM (RHEL pref, but fallback on Centos if neccsary) --> build environment, like conda for python, install docker, build basic webpage, etc --> Install apache with Bash --> configure apache with Bash --> Docker it up, and build that container --> test container to make sure it works --> Deploy with Jenkins??? --> Dockerhub???


Todo:

1. Setup KVM Server for RHEL
2. Build basic RHEL VMs
3. Play around and get apache automated and installed, use bash script to build and then build the docker container 
4. Figure out the hows and whys with Jenkins

---

## NEW DAY 4/8/2022

- Working on getting my vms created, going to build base VM and clone it out, using RHEL 8.5. I will use these clones as the base for my Docker and Jenkins as well as Apache builds

- Created my RHEL VM Template and clones, they are configured as a "minimal" headless build ready to do anything with. One for Docker and one for Jenkins, I will make more if I need to.

- Now I need to focus on Docker and Apache, mainly how I want ot apporch the appache install. Going to put appache on my docker box along with a docker install. Should work right? Also going to research some more so I don't hit too many hiccups...

- Installed Docker with no major issues, now looking into how to configure and build a "secure" apache image


Todo:

1. Start building automated apache script for docker
2. Test Script
3. Jenkins

---

## NEW DAY (4/9/2022)

- starting to build the Apache bash file, it will:
  1. Build an openssh X509 cert pair for apache (maybe)
  2. Call docker with it specilized config file for apache

- The Apache Docker file will:
  1. Copy over premade certs (or it will build them in container, still figuring this one out)
  2. Copy over an apache config bash script and run
  3. Copy over an apache stig bash script and run
  4. Copy over the custom html page and put it in the correct dir
  5. Bind ports so that server can be reached
  6. Run appache from docker hubs "apache" image (https://hub.docker.com/_/httpd) as a base
  7. Run apache with the additional configuration, thus our "secure" apache server

- The Apache Folder that contains the docker file will:
  1. Have the custom html page
  2. Have a Config Script:
      - Set a custom port for apache to listen on
      - Set hmtl deafult to /var/www/html/devsecops_practical
      - Copy or make X509 keys and copy to "/etc/pki/tls/", set secure permissions
      - Set TLS 1.2 only enforce (Stig item V-214230) (https://www.stigviewer.com/stig/apache_server_2.4_unix_server/2022-01-27/finding/V-214230)
  3. Have Stig Script:
      - Apply five STIGs to Server (Still need to pick these) gettting stigs from (https://www.stigviewer.com/stig/apache_server_2.4_unix_server/)
  4. Have tls certs?? I really think I can generate these inside of my container, thus I might just integrate into Config Script

- Built and finished the Config script, decided to put stigs into the config script so stigs are done as well. I tested on my container and I get my website and cert with no problems, the container starts fine as well. Frist time implementing STIGs and have it work.

TODO

1. Review DevSecOps_Practical to make sure you are on track
2. Begin Jenkins (Learn, Build, Configure, Implemnt)

---

## NEW DAY 4/10/2022

- Did some research on Jenkins and ready to install, going to install both Jenkins and Docker on one VM and build my Jenkins file based off of that.

- How this is going to work
  1. Build --> Simple I'll download my dockerfile and config from github and the build it on my Jenkins box
  2. "Run" --> Easy, I'll run my container
  3. "Test" --> okay I still need to reasearch this on the specifics, but I plan to use a simple bash script to test that will have exit codes to specifiy if the test passed or failed. Planing on using curl to test url and openssl to verify cert.
  4. "Push" --> Have to make a dockerhub account, but the its as easy as imputting my creds into Jenkins and telling it to upload

- After that I should basically be done

- Built my Jenkins file with all Stages. Tested and the build is fine and the push to Dockerhub is fine. Everything is fine, thus I think I can call it.

- All I need to do now is finish documentation and upload to Github. I will also download on of my images and test it.

- Documentation Complete, tested webserver, it is fine.

## Conclusion (4/10/2022)

- Project Complete, Images can be pushed/pulled to Dockerhub with little to no issue. 

- Use this project as a base if you would like to build Docker with Jenkins and spin up a pipeline. Some of the notes in here can really help with the gotchas.

- Overall this was a great quick project with loads of learning. 

