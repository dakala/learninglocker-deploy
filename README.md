# Learning Locker Version 2 Open Source installer

This is the HT2 Learning Locker open source installer. It's designed to walk you through the process of
downloading the code, running any steps needed (such as compilation) and generally setting up a complete
working instance. To do this it attempts to detect your operating system, install any software needed
then pull the code down from Github, run the build steps and install to whichever directory you've
specified. You'll be prompted for everything required while you run the script.

Please run the required update script for your distro to ensure you have the latest version of all packages.

.e.g
```
yum update
```
or
```
apt-get update && apt-get upgrade
```




### QuickStart

**You must run this script as root user. Typically this can be done by running `sudo su -`**

To install or update Learning Locker, run the install script via one of the commands below. Then, follow the prompts given.

**Install with cURL**
```sh
curl -o- -L http://lrnloc.kr/installv2 > deployll.sh && bash deployll.sh
```
**Install with Wget**:
```sh
wget -qO deployll.sh http://lrnloc.kr/installv2 && bash deployll.sh
```

Alternatively, you can download the "llv2_build.sh" file from this repository and run the following command as the root user, then follow the prompts given.

```sh
bash llv2_build.sh
```



### Amazon Machine Images (AMI)

You can also install Learning Locker using an Amazon Machine Image (AMI). 
Our AMIs are available in the list of community AMIs by searching for 'learning locker' in the following EC2 zones:

```
us-east-1 (N. Virginia)        |  eu-west-2 (London)
us-east-2 (Ohio)               |  ap-southeast-1 (Singapore)
us-west-1 (N. California)      |  ap-southeast-2 (Sydney)
us-west-2 (Oregon)             |  ap-northeast-2 (Seoul)
ca-central-1 (Canada Central)  |  ap-northeast-1 (Tokyo)
eu-west-1 (Ireland)            |  ap-south-1 (Mumbai)
eu-central-1 (Frankfurt)       |  sa-east-1 (Sao Paulo)
```

The AMIs are built with all required software on and as such are designed to be used to try out Learning Locker with little 
friction or need for systems administration experience. You will need to be familiar with starting AMIs and the Amazon interface 
in general which is outside the scope of this document.

When you start the AMI, you will need to allow access to port 80 from your IP in order to be able to view the Learning Locker 
user interface. You will also need to ssh to the AMI using the default 'ubuntu' user and the keypair you specified at launch time
in order to read the /home/ubuntu/ll_credentials.txt file which contains autogenerated default login credentials for a sample
organisation and user. This file will be generated when the server is fully running and can take up to 5 minutes to be created.



### Configuration
The software creates a `.env` file in your install directory and within the `xapi/` sub-directory. You can
configure things here if you want to use external mongo or redis servers or change any ports in use.
In addition, the software uses nginx to route traffic. Routing rules are defined in the learninglocker
nginx configuration which is installed as part of the script. This varies depending on OS but are
currently:

	CentOS / Fedora : /etc/nginx/conf.d/learninglocker.conf

	Debian / Ubuntu : /etc/nginx/sites-available/learninglocker.conf


### Running / restarting
The system works by creating four Node.js server instances running under PM2 (a Node process management suite) which get traffic routed
to them from the nginx config. In order to make sure that everything restarts automatically, the build
process creates a restart script which you can run as follows:

	service pm2-{USER} restart

The user in this is the user you opted to install Learning Locker under. If you went with the default of
'node' then this would be:

	service pm2-node restart


### Supported Operating systems
	CentOS
	Fedora
	Ubuntu
	Debian

### Software Required - Installed automatically
	git
	python
	build tools (ie: GCC)
	xvfb
	curl
	nodejs
	yarn
	pm2
	nginx

### Software Required - Optional installs
	MongoDB
	Redis

In addition to the above, the software will require access to an instance of MongoDB and Redis. By default,
the configuration points to instances on localhost and will offer to install mongo & redis for you if you
want to get everything working on a single server.


### Recommended hardware
While there is no specific hardware requirement beyond a reasonably modern 64bit x86_64 computer with 2GB of
RAM, you should pay attention to your memory usage. If you're running redis and / or mongo on the same server
as learning locker then you'll need more memory than without. Server load will depend on the use cases for
your particular organisation.
