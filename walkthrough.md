## Intro
This repository holds the necessary code for setting up various environments using Ansible as a config management tool.

### Below are excerpts of the project implementation:
- [Setup of neccessary ansible roles](https://github.com/MekkyMayata/CI-JAASP/tree/master/roles)




#### configure Ansible for Jenkins Deployment
- Generate a personal access token from the github developer settings and grant neccessary permissions
![Generate An Access Token](https://user-images.githubusercontent.com/54307445/112208231-6960fe00-8c18-11eb-8795-e9c0323da312.png)
- Connect Jenkins to Github by supplying the created token
![Connect jenkins to Github](https://user-images.githubusercontent.com/54307445/112207858-f2c40080-8c17-11eb-8eb6-d0f78f8ef9b7.png)
- Select your github repository and the pipeline is created
![project-14-new-pipeline](https://user-images.githubusercontent.com/54307445/112207903-fd7e9580-8c17-11eb-9666-fa46f8fe00cb.png)




#### Ansible Jenkins Integration| How to run Ansible playbook from Jenkins pipeline job
1. Have the jenkins server and the server(s) that the ansible will run the tasks on. The ansible project code should also be ready.

2. Install ansible on jenkins server
 `sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm`
  `sudo yum install ansible -y`

3. Install ansible plugin on Jenkins UI
![install ansible-plugin-on-jenkins](https://user-images.githubusercontent.com/54307445/112209730-138d5580-8c1a-11eb-872c-28a45b9de3c8.png)

4. Configure ansible under manage jenkins >> global tool configuration
![configure ansible](https://user-images.githubusercontent.com/54307445/112209914-50f1e300-8c1a-11eb-9346-a260955e0fcf.png)

5. Edit/Create new job (PIPELINE JOB)
    - Have the jenkins file edited
    - Use the jenkins pipeline syntax generator to generate a git step.
    - Use the jenkins pipeline syntax to generate the ansible step and scm checkout step (also supply the ssh private key for jenkins to connect to the nodes when running the ansible configuration)
    ![ansible-pipeline-syntax](https://user-images.githubusercontent.com/54307445/112210422-f4db8e80-8c1a-11eb-85f8-174688545fc2.png)

    - add the syntax to the [jenkinsfile](https://github.com/MekkyMayata/CI-JAASP/blob/master/deploy/jenkinsfile)


#### Parameterizing Jenkinsfile For Ansible Deployment
To deploy to other environments, we make use of parameters in the [jenkinsfile](https://github.com/MekkyMayata/CI-JAASP/blob/master/deploy/jenkinsfile)
![project-14-parameterized-build](https://user-images.githubusercontent.com/54307445/112210970-a24ea200-8c1b-11eb-96fd-fcf18cc3f5b9.png)

[Next, setup the CI pipeline for the todo application](https://github.com/MekkyMayata/php-todo/blob/main/README.md)



#### N.B: 
MAKE SURE TO INSTALL THE NECESSARY ANSIBLE PLUGINS IN THE JENKINS HOME (BASED ON THE ANSIBLE TASKS YOU WILL BE RUNNING). 
Also, you may need to change ownership recursively, so the plugins can download in the jenkins home too.

```
   sudo chown -R jenkins:jenkins /var/lib/jenkins/.ansible/
   ansible-galaxy collection install community.general
   ansible-galaxy collection install community.postgresql
   ansible-galaxy collection install ansible.posix

```

