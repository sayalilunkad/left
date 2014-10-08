---
layout: post
title: Working with Devstack
comments: true
---
This post is for all those who are just starting with OpenStack. The easiest and best way to start your journey in OpenStack Development would be through Devstack. So I will walk you through the basic OpenStack setup using Devstack and some useful things that would help you while you use Devstack.

It is recommended to use virtual machine to install Devstack as you can easily get rid of it without having much to loose, in case of any crashes. Also it would save time if you keep a snapshot of the Devstack setup on the virtual machine so that in the future you would save the installation time.
So lets begin with the setup!

Initial Setup
--------------
I am using 64-bit Ubuntu 12.04 for demonstating the setup.

1. [Install VirtualBox](https://www.virtualbox.org/wiki/Linux_Downloads)
2. Create a new virtual machine with minimum 2GB RAM (required for Devstack to run well) and install 64-bit Ubuntu 12.04
3. Run the virtual machine
4. Open the console
5. Run an update

		$ sudo apt-get update

6. Install git

		$ sudo apt-get install git


Setting up Devstack
-------------------
Devstack is a shell script used to deploy a complete OpenStack development environment. The Devstack script installs all the dependencies required to run OpenStack so there is no need to install anything separately. It is a good place to start for beginners starting with OpenStack development.


1. Open the console and clone the devstack repo which contains a script that will install OpenStack. You should clone this repo where you want your devstack to reside. This command will fetch the devstack repo to your local machine.

		$ git clone https://github.com/openstack-dev/devstack.git

2. Now you will find a devstack directory where you cloned the repo

		$ cd devstack

3. Run the script to install OpenStack

		$ ./stack.sh

The script will take some time to build. It will ask you to enter passwords for the default services enabled in the script.

After it is done it will return a URL for horizon. Open that link in your browser and you can acess the OpenStack Dashboard!

By default you get two usernames-admin and demo. The password will be the password you set for keystone(which is the identity service provided by OpenStack) which you can see in the local.config file in the devstack directory too.

Here you are, done with the basic configuration for OpenStack!

(Reminder - Don\`t forget to take a snapshot of the virtual machine once you have reached this point!)

What the scripts do?
-------------------

Now if you take a look at your devstack directory you will find some very useful scripts. Here I will tell you how these scripts are used.

1. **stack.sh** </br>
We have run this script initially to install Openstack. This script allows you to specify configuration options of what git repositories to use, enabled services, network configuration and various passwords. You need to run this script everytime you make any change to the OpenStack configuration or enable any new service.

2. **unstack.sh** </br>
Stops all the services started by stack.sh (mostly) mysql and rabbit are left running as OpenStack code refreshes do not require them to be restarted.
Note that any changes made to the code should remain intact after running unstack.sh unless set otherwise.

3. **rejoin-stack.sh** </br>
This script rejoins an existing screen, or re-creates a screen session from a previous run of stack.sh.
This means that if you have previously run stack.sh and you want to rejoin the session you run rejoin-stack.sh. Doing this restores all the data from the previous sessions. So do not be scared of loosing your data after rebooting your machine. </br>
*Just run  ./unstack.sh* </br>
*Then, reboot your machine*</br>
*When you want to join the session again run*
*./rejoin-stack.sh*</br>
And you have all your data in place! You can check by accessing the dashboard on the same URL as before.
4. **run_test.sh**</br>
OpenStack havana release added a tool called bash8 in order to do style checking in Devstack. This tool is run on the entire project using run_test.sh script. This will give information regarding any stray white spaces.

5. **clean.sh**</br>
Does its best to eradicate traces of a Grenade run except for the following: - both base and target code repos are left alone - packages (system and pip) are left alone. This is used if you want to erase all the OpenStack related files from your system.

6. **exercise.sh**</br>
This script runs all the examples present in the devstack/exercises directory and reports on the results.
The exercises directory contains the test scripts used to validate and demonstrate some OpenStack functions. These scripts know how to exit early or skip services that are not enabled.

Configuring services in OpenStack
----------------------------------
The stackrc is the primary configuration file for DevStack which contains all of the settings that control the services started and the repositories used to download the source for those services. stackrc sources the local.config to perform the default overrides.

By default the following services are enabled when you install OpenStack using Devstack:

* Nova  (API, Certificate, Object Store, Compute, Network, Scheduler, VNC proxies, Certificate Authentication)
* Cinder (Scheduler, API, Volume)
* Glance (API and Registry)
* Horizon
* Keystone
* MYSQL
* RabbitMQ
* Tempest

Additional services that are not enabled by default can be enabled in the local.config.
local.config should be copied from devstack/samples to the main devstack folder.
Any tweaks to the configuration like disabling any service can be done from the local.config

Now we will see how to enable the other services in OpenStack.
Some services that are not enabled by default are given below. Select the required service and add the respective lines to your local.config.

**Swift - Object Storage**

		ENABLED_SERVICES+=,s-proxy,s-object,s-container,s-account

**Ceilometer - Metering Service**

		ENABLED_SERVICES+=,ceilometer-acompute,ceilometer-acentral,ceilometer-collector,ceilometer-api
		ENABLED_SERVICES+=,ceilometer-alarm-notify,ceilometer-alarm-eval

**Neutron - Networking Service**

		ENABLED_SERVICES+=,q-svc,q-agt,q-dhcp,q-l3,q-meta,neutron

**Heat - Orchestration Service**

		ENABLED_SERVICES+=,heat,h-api,h-api-cfn,h-api-cw,h-eng

**Apache fronted for WSGI**

		APACHE_ENABLED_SERVICES+=keystone,swift

**Enable Logging**

You can also enable logging in Devstack, as it does not log by default.

		LOGFILE=/opt/stack/logs/stack.sh.log
		VERBOSE=True
		LOG_COLOR=True
		SCREEN_LOGDIR=/opt/stack/logs

**Reclone each time**

If you want to reclone the Devstack repo each time you run stack.sh then you can enable reclone too in the local.config.

		RECLONE=yes


After adding the required service you will need to run stack.sh again in order to enable the service. To check if the service is enables you can check the OpenStack Dashboard admin tab -> System Info -> services.



Where to find the code?
------------------------
Once you have all the setup running you might want to take a look at the code running behind all this.
All the code related to OpenStack will be found here in

		/opt/stack

Now you can start exploring the code right away!


References
-----------
Some of the links I referred to when I started:-</br>
The official [Devstack](http://devstack.org/) site.  </br>
[OpenStack](http://docs.openstack.org/developer/openstack-projects.html) Developer Documents.

I tried to collect all the things required for a beginner to setup OpenStack with ease. I hope this post is useful to all. Please let me know if there is anything I missed out on or any place you get stuck.

