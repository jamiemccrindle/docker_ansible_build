# Docker and Ansible builder

This is an example of using Docker, Ansible, Vagrant and Jenkins to build a self contained continuous 
integration environment. It was put together for the a presentation done at Docker London. The slides 
that go along with this code are here:

http://slides.com/jamiemccrindle/docker-and-ansible#/

## Getting started

### Requirements

* [Vagrant](http://www.vagrantup.com/)
* [git](http://git-scm.com/)
* [Python](https://www.python.org/)
* [Ansible](http://www.ansible.com/home)

### Vagrant SSH

Add the following to your .ssh/config file so that ansible can ssh into Vagrant

    Host vagrant
      HostName 127.0.0.1
      User vagrant
      Port 2222
      UserKnownHostsFile /dev/null
      StrictHostKeyChecking no
      IdentityFile ~/.vagrant.d/insecure_private_key

### Notes

* installing the docker registry will take quite a while. the step will look like this:

        docker_registry | docker ports="5000:5000" image="registry"
        
* the installation process should be idempotent so if it gets stuck, you should be able
  to rerun it

### Building things


    # clone the repo
    git clone git@github.com:jamiemccrindle/docker_ansible_build.git
    # go into the build directory
    cd docker_ansible_build
    # start up vagrant
    vagrant up
    # run ansible with lots of debugging
    ansible-playbook build.yml -i inventory/vagrant_build -vvvv
    
    
### Running it

Browse to [http://localhost:8080](http://localhost:8080) and click on the docker_and_ansible_app. Click 'build now' on the left hand side to 
kick off a build. Once the build has completed, browse to [http://localhost:8000](http://localhost:8000) to see the app running.
    
### Todo

* Figure out why Jenkins doesn't see changes to the git repo
* Run Jenkins in a container
    
### Troubleshooting

* It can take a while for jenkins to come up, which will result in a 503 when the app tries to get the cli, just rerun 

        failed: [vagrant] => {"dest": "/opt/jenkins/jenkins-cli.jar", "failed": true, "response": "HTTP Error 503: Service Unavailable", "state": "absent", "status_code": 503, "url": "http://localhost:8080/jnlpJars/jenkins-cli.jar"}
        msg: Request failed

## What it is doing under the hood

* Installs the latest version of Docker
* Installs Ansible
* Installs Java
* Installs a Private Docker Repository
* Installs and configures Jenkins
* Adds Jenkins Job to build our application
* The Jenkins Job:
    * Compiles the application
    * Builds a docker image
    * Pushes the docker image to the docker repository
    * Runs and tests the new docker