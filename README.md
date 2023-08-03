### Jenkins course 
https://www.youtube.com/watch?v=6YZvp2GwT0A 

Jenkins k8s reference 
https://www.jenkins.io/doc/book/installing/kubernetes/ 

Jenkins docker compose setup
https://github.com/Douglas0n/jenkins-docker 
how to install

- Create a file named .env and add the following:
```bash
$ USER_NAME=<< username here >> # to create the docker volume.
JENKINS_AGENT_SSH_PUBLIC_KEY=<< leave empty for now >>
```

- Run Jenkins controller:
```bash
$ docker compose up -d 
``` 

- Get the password to proceed installation: 
```bash
$ docker logs jenkins | less 

sudo docker exec ${CONTAINER_ID or CONTAINER_NAME} cat /var/jenkins_home/secrets/initialAdminPassword

```

- Go to http://localhost:8080/ and enter the password. 

Select Install Suggested Plugins, create the admin user and password, and leave the Jenkins URL http://localhost:8080/. 

### Configuring Jenkins Agent 

- Use ssh-keygen to create a new key pair: 
```bash
$ ssh-keygen -t rsa -f jenkins_key
``` 

- Go to Jenkins and to Manage jenkins > Manage credentials. 

- Go to Jenkins, under Stores scoped to Jenkins, click Global credentials, next click Add credentials and set the following options:

-- Select SSH Username with private key.
-- Limit the scope to System.
-- Give the credential an ID.
-- Provide a description.
-- Enter a username.
-- Under Private Key check Enter directly.
-- Paste the content of private key in the text box.

- Click Ok to save. 

- Paste the public key on the JENKINS_AGENT_SSH_PUBLIC_KEY variable, in the .env file. 

- Recreate the services:


```bash
$ docker-compose down
$ docker-compose up -d
``` 


### Jenkins infrastructure 

Master server:
- controls pipelines 
- Schedules Builds 

Agents/Minions 
- Perform the build 


- Develper commits some code to repository
- Commits trigers pipelines 
- Jenkins server distributes to agents to run 
- Agent selected based on configured labels
- Agent runs build 

Agent tupes:
- Pernament Agents 
  -- Dedicated Servers for running jobs 
- Cloud agents
  -- Ephemeral/Dynamic Agents spun up on demand 

to make a doker as cloud agent 

### Kubernetes plugin for Jenkins 

Build tyoes:
- Freestyle Build
  -- simplest method to create a build
  -- feels like a shell scripting
- Pipelines 
  -- Use the Groovy Syntax 
  -- Use stages to break down Components of builds 

  
  Pipeline Stages:
  - clone
  - build
  - test
  - package
  - deploy



