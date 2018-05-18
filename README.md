# LoRa server docker/vagrant setup

### Basic LoRa server setup based on original [LoRa Docker-Setup][1].

### Running the server:

The environment is orchestrated using Vagrant/Docker for automated setup.

**On Windows**

* Since Docker config used is based on linux containers, windows users will have to run it inside a VM. So, download and install [Vagrant][2] and [Virtualbox][3].

* Go to project directory and run `vagrant up` in terminal. Initial start will take sometime, but proceeding starts will be relatively quicker. Additionally, by default, Docker containers are rebuilt at every boot. To change this setting, go to [Vagrantfile][4] and commend-out indicated lines in *docker-compose provision* block.

* This will setup Vagrant and docker containers and deploy application on address `https://localhost:<PORT>` (where `<PORT>` corresponds to a valid open port chosen by Vagrant).

* *You will get the exact web-address which you can enter in your browser to access app-server UI (once the script finishes successfully)*.

* If for some reason, docker containers do not build, you can ssh into VM using `vagrant ssh` and then use the command `docker rm $(docker ps -aq) && docker-compose up -d` to try building again.

**On Linux**

**NOTE**: You can also setup Vagrant in Linux and use the same procedure as provided for Windows. Otherwise, below is how to setup without using Vagrant.

* Download and install docker and docker-compose.

* Go to project directory and run `docker-compose up -d`.

* Ensure port `8080` is free or the containers won't run (unless configured to specifically use another port). Application will be deployed on address `https://localhost:8080`.

### App-Server credentials:

Default credentials for the App-Server are:

Username: `admin`  
Password: `admin`

[![App-Server Dashboard UI][5]][5]

  [1]: https://github.com/brocaar/loraserver-docker
  [2]: https://www.vagrantup.com/
  [3]: https://www.virtualbox.org/
  [4]: https://github.com/Jaskaranbir/lora-server-docker/blob/master/Vagrantfile
  [5]: https://i.imgur.com/faa7nDP.png
