# Purpose: This will record the process of my build and configuration of Jenkins server

---


## Install

- Installing docker first... Actaully to save time, going to use my docker vm for jenkins

- Verified docker still working and my web container is functional

- Installing Jenkins
  - https://www.jenkins.io/doc/book/installing/linux/
  - Easy install with documentation for RHEL, basically you add the jenkins repo, install openjdk and then `yum install jenkins`
  - Starting Jenkins, again doc provides this easy.. Done, started with no issue
  - Starting Post Intall Conifg:
  ![](Pics/20220410122205.png)  
  ![](Pics/20220410122704.png)  
  ![](Pics/20220410122803.png)  
  --> Dev Environment so this will suffice
  ![](Pics/20220410122853.png)  

  - Jenkins Install Complete, verified that both the "Credentials" and the "Credentials Binding" plugins were installed

- Now the build process begins

## Start Build: download repo from git

- Starting a new Jenkins file that I will put in the "content repo"

- Reasearching how to build and configure Jenkins file
- Building a "Multibranch Pipeline".. Done

- Changed up Github repo for a docker and jenkins repo both separate

- Reversed this decsion, all files now in same root and repo

- Congfiguring Jenkins for Git Rep polling, with the idea being if you change something it will autobuild and deploy.

- See [Enable GITPolling Tutorial](/Jenkins.md#tutorials)

- Configured Jenkins with Multibranch pipes for Github polling, now when I update my repo, it will build.

- Time to configure my Jenkinsfile. I know that Multibranch pipes automatically checkout my code from github so I will not need to make a stage for this, thus I can get right to building, running and testing and then pushing.

## Building My Jenkinsfile

- Discovered that the "post" stanza does not process environment vars, thus you can't use them there

- Discovered that if a stage fails, all sequential stages stop, this is useful!!

- Discovered that you need to add jenkins to the docker group to run docker stuff: (Don't forget to restart jenkins and docker services)

```
sudo usermod -aG docker jenkins
```

- Nice, now my Jenkinsfile sucessfully builds my docker images, next running them and testing

- Run script complete, now Jenkins builds and runs my docker

- Working Test Script

- Test Script complete, see [Apache](./Apache.md#tutorials) for more info

- Stop Script compelte

- Now whenever a change is made to github, jenkins downloads new source and compies it and tests. Thus all I need to do now is push to dockerhub

## Push to Dockerhub

- Creating dockerhub account... Done

- Updating my scripts so that my docker images will be in the name format:

```
komquest/webserver:tagname
```
- This is how dockerhub works, so I must comply. Testing my scripts again since I had to put in a "/" in name, how this doesn't mess anything up...

- All works fine, now setting up my dockerhub creds in Jenkins and will try to push

- Wow, the push script worked the first time

- COMPLETE, Github --> Build --> Docker Hub:

![](Pics/20220411010755.png)  