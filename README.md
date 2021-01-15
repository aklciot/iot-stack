# iot-stack
This will build a complete full IoT stack on a number of different computer types. 

## Pre-requisites
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

3 DNS entries are needed, one each for thingsboard, Nodered and mosquitto (MQTT)
This documentation assumes you have the 3 DNS entries all pointing to the IP address of the computer hosting the IoT Stack. This can be quite involved, there are some notes at the bottom of this page.
* `thingsboard.iot-stack.example`
* `nodered.iot-stack.example`
* `mqtt.iot-stack.example`

### Modify the docker-compose.yml file
The docker-compose command will be used to run the iot stack, but you need to make some changes to it so it works for you. 
1. Change to the folder just created by your clone command `cd iot-stack`
2. Edit the file (nano is an editor on most linux systems) `nano docker-compose.yml`
3. Look for the line

          - traefik.http.routers.thingsboard.rule=Host(`thingsboard-iottest.innovateauckland.nz`)
    and change the DNS name to your DNS name for thingsboard e.g. `thingsboard.iot-stack.example`
4. Look for the line

          - traefik.http.routers.nodered.rule=Host(`nodered-iottest.innovateauckland.nz`)
    and change the DNS name to your DNS name for Nodered e.g. `nodered.iot-stack.example`
5. Look for the line

          - "traefik.tcp.routers.mqtt.rule=Host(`mqtt-iottest.innovateauckland.nz`)"
    and change the DNS name to your DNS name for mosquitto (MQTT) e.g. `mqtt.iot-stack.example`
### Run the stack
Run the following command:
`docker-compose up -d`
This will take quite a while depending on your internet speed and how powerful the computer is. But once finished, your IoT stack should be ready for you.

# IoT Stack components
## [Nodered](https://nodered.org/)
This is where flows and controls are created an managed. To access it enter the following URL on the computer that the IoT Stack is running `https://nodered.iot-stack.example`. An example flow will be on NodeRed as an example. 

## [MQTT](https://mosquitto.org/)
This is a simple system to take IoT messages and distribute them to where they are needed. there is a good description [here](http://www.steves-internet-guide.com/mqtt/).
The IoT stack exposes 2 ports that clients can connect to:
* 1883 - this is a standard port and connections are unsecured and unencrypted
* 8883 - this is an encrypted port

The initial install does not come with users and passwords defined. This is recommended but will require a little effort, [documentation can be found here](http://www.steves-internet-guide.com/mqtt-username-password-example/)
## [Thingsboard](https://thingsboard.io/)
This is a data storage and visualisation system. Nodered can store data in Thingsboard which is then configured with dashboards so poeple can make use of the data.

A couple of things to get you started:
### Initial login
Username: `tenant@thingsboard.org`
Password: `tenant`
### Accessing Thingsboard
To access the web interface just use https://thingsboard.iot-stack.example.

When you need to send data to thingsboard you use the MQTT protocol, but because we already have MQTT installed we need another port. The port used for getting MQTT data into Thingsboard is:
* 7883 - MQTT port for Thingsboard

## DNS (Domain name system)
What is needed is a DNS name that takes the user to your host/traefik instance. This can be hosted anywhere, but you should be able to manage it. You need to ensure the DNS entry is pointing to the traefik instance

### If you do not have a static IP address.
This is a very likely occurrence if you are using Fibre/ADSL from a telco. Use a dynamic DNS system like [DuckDNS](https://www.duckdns.org/) to get a DNS name you can use. 
* Create an account on their site
* follow instructions to [install](https://www.duckdns.org/install.jsp) onto your host.
* If you are not a potato you will have a DNS name that points to our computer, and even if your ISP changes your IP address the DNS entry will change automatically for you.

You will now have a DNS name, e.g. aklciot-xyz.duckdns.org. You can use this for your service or get a specific domain name, see below.
	
### If you do have a static IP address. 
This could be the case if you are using a hosting provider like Linode or AWS etc. The host you set up will have an IP address you can use.
	
### Your own domain name
There are many registrars you can purchase/rent a domain name of your choice. I use [1stDomains](https://1stdomains.nz/0), but many others exist. Once the domain has been obtained create a DNS records for each of the 3 services. the examples used here were `thingsboard.iot-stack.example`, `nodered.iot-stack.example`, `mqtt.iot-stack.example`
* If you are using Duckdns, create a CNAME record pointing to your duckdns.org name
* If you are using a static IP address, create an A record pointing to that IP address

It is not really possible to state exactly how to do this, it will depend on the DNS provider you choose.