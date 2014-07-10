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

### Notes

* installing the docker registry will take quite a while. the step will look like this:

        docker_registry | docker ports="5000:5000" image="registry"

### Building things


    # clone the repo
    git clone git@github.com:jamiemccrindle/docker_ansible_build.git
    # go into the build directory
    cd docker_ansible_build
    # start up vagrant
    vagrant up
    # run ansible with lots of debugging
    ansible-playbook build.yml -i inventory/vagrant_build -vvvv
    
    
### Todo

    * Figure out why Jenkins doesn't see changes to the git repo
    * Run Jenkins in a container
    
### Troubleshooting

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