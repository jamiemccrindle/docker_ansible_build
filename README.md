### Docker and Ansible builder

This is an example of using Docker, Ansible, Vagrant and Jenkins to build a self contained continuous 
integration environment. It was put together for the a presentation done at Docker London. The slides 
that go along with this code are here:

http://slides.com/jamiemccrindle/docker-and-ansible#/

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