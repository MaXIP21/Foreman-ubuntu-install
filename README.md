# How to install and configure Foreman 3.0 on Ubuntu 20.04

First install ubuntu to a vm or the target host using the official image from https://ubuntu.com/ \
Then log in to the system and set up a fix IP in my demo the IP address will be 192.168.1.140 we will use this address further in the project. \
You can find the netplan configuration in the file 00-installer-config.yaml. \

Once it's compleated let's start the installation 
# Foreman installation
sudo apt-get update \
sudo apt-get upgrade \
sudo apt-get install ca-certificates wget \


# Provisioning a bare metal host.
# Creating password for provisioning template
You can use mkpasswd to create hashed password for your users
>$ apt-get install whois \
>$ mkpasswd -m sha-512 __[YOUR USER PASSWORD]__ 

Or to make it more secure by adding some generated sand using pwgen
>$ apt-get install pwgen \
>$ mkpasswd -m sha-512 -S $(pwgen -ns 16 1) __[YOUR USER PASSWORD]__
