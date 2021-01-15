# iot-stack
This will build a complete full IoT stack on a number of different computer types. 

##Pre-requisites
* A computer connected to the internet
* A DNS domain that you can control and add entries to
* Some time
* A person doing the install who is not a potato

There are q few things required so we use [Docker](https://docs.docker.com/install/) to get them all running on one computer. Docker can run on a Raspberry Pi, on Windows or Linux.
[Docker-compose](https://docs.docker.com/compose/install/) is used to run/start/stop the system, it come pre-installed on Docker for Windows, but you need to nstall it on Linux (and the raspberry Pi).

## Install
1. Install [Docker](https://docs.docker.com/install/)
2. Install [Docker-compose](https://docs.docker.com/compose/install/) (if needed)
3. Install git (if on a windows computer)
4. Clone this repository
5. Make some changes to the docker-compose.yml file provided in the that was just downloaded
5. Run docker-compose to install and run the necessary components

### Install docker
Follow the documentation [here](https://docs.docker.com/install/) for your operating system/computer.
You can check to see if it is installed by running the following command from the command line:
`sudo docker run hello-world`
You may get a permissions error if you are on inux, if so do the following:
`sudo groupadd docker`
`sudo usermod -aG docker $USER`
Log out, then log back in again and retry the hellow world test.
### Install docker-compose (if on a linux based computer)
Information on how to installcan be found [here](https://docs.docker.com/compose/install/)
### Install git
git is a system to manage source code and file. We use it to manage the files that the IoT stack uses. You need to install git to copy these files onto your computer. It is usually pre-installed on linux computers, but if not information on how to install it can be found [here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
### Clone this repository
Create a folder on your computer where you want to store the IoT stack files. From the command line, go to this folder and then run:
`git clone https://github.com/aklciot/iot-stack.git`
### Create DNS entries
TBA
3 DNS entries are needed, one each for thingsboard, Nodered and mosquitto (MQTT)
### Modify the docker-compose.yml file
The docker-compose command will be used to run the iot stack, but you need to make some changes to it so it works for you. 
1. Change to the folder just created by your clone command `cd iot-stack`
2. Edit the file (nano is an editor on most linux systems) `nano docker-compose.yml`
3. Look for the line

          - traefik.http.routers.thingsboard.rule=Host(`thingsboard-iottest.innovateauckland.nz`)
    and change the DNS name to your DNS name for thingsboard
4. Look for the line

          - traefik.http.routers.nodered.rule=Host(`nodered-iottest.innovateauckland.nz`)
    and change the DNS name to your DNS name for Nodered
5. Look for the line

          - "traefik.tcp.routers.mqtt.rule=Host(`mqtt-iottest.innovateauckland.nz`)"
    and change the DNS name to your DNS name for mosquitto (MQTT)
### Run the stack
Run the following command:
`docker-compose up -d`
This will take quite a while depending on your internet speed and how powerful the computer is. But once finished, your IoT stack should be ready for you.

# IoT Stack components
## Nodered
This is where flows and controls are created an managed. To access it enter the following URL on the computer that the IoT Stack is running `http://localhost:1880`. If you are on a different computer on the same network you will need to find out the ip address on the IoT Stack computer. the from a browser on a different computer `http://iot-stack-ip-address:1880`
An example flow will be on NodeRed as an example. This flow takes data from Innovate Auckland's system and stores it in Thingsboard (see below).
## MQTT
This is a simple system to take IoT messages and distribute them to where they are needed.
## Thingsboard
This is a data storage and visualisation system. Nodered stores the data in Thingsboard which is then configured with dashboards so poeple can make use of the data.