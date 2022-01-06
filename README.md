# How to install and configure Foreman 3.0 on Ubuntu 20.04

First install ubuntu to a vm or the target host using the official image from https://ubuntu.com/ \
Then log in to the system and set up a fix IP in my demo the IP address will be 192.168.1.140 we will use this address further in the project. \
You can find the netplan configuration in the file 00-installer-config.yaml. 

Once it's compleated let's start the installation 
# Foreman installation
>sudo apt-get update \
>sudo apt-get upgrade \
>sudo apt-get install ca-certificates wget

## Setting up the FQDN in our example it will be foreman.example.com
>sudo hostnamectl set-hostname foreman.example.com \
>echo "192.168.1.140 foreman.example.com" | sudo tee -a /etc/hosts \

Then verify the hostname using the command

> ping -c 2 $(hostname -f)

Here it's a good ide to reboot the system 

>sudo apt update && sudo apt upgrade \
>sudo systemctl reboot

## Installing Foreman

### Installing Puppet6 

>wget https://apt.puppet.com/puppet6-release-focal.deb \
>sudo apt install ./puppet6-release-focal.deb

### Adding Foreman repositories

>sudo apt -y install gpg ca-certificates \
>echo "deb http://deb.theforeman.org/ focal 3.0" | sudo tee /etc/apt/sources.list.d/foreman.list \
>echo "deb http://deb.theforeman.org/ plugins 3.0" | sudo tee -a /etc/apt/sources.list.d/foreman.list \
>wget -q https://deb.theforeman.org/pubkey.gpg -O- | sudo apt-key add - \
>sudo apt-get update \
>sudo apt-get install foreman-installer \
>sudo apt-get update

### Running the installer with the correct configuration 
If you just run the foreman insaller you need lots of extra steps to make it complete for that reaseon \
It's recommended to use extra parameters. First summarize which services do we need ofr our local environment: \
- DNS server to translate our local dns to IP address (foreman.example.com) \
- DHCP service to provide correct dhcp configuration for the TFTP boot \
- TFTP service for booting the installation images from network. 

So let's create a command line (or snply copy from the run_foreman_installer.txt)

>Please change the values below according to your local network settings \
>Here the settings are Foreman server IP 192.168.1.140 \
>nameservers 192.168.1.140 and 8.8.8.8 \
>gateway 192.168.1.1 (this is my router)

### In case you make any changes please check the __BOLD__ values

sudo foreman-installer --foreman-proxy-dns true \
--foreman-proxy-dns-managed true \
--foreman-proxy-dns-interface __enp0s3__ \
--foreman-proxy-dns-zone __foreman.example.com__ \
--foreman-proxy-dns-reverse 2.0.192.in-addr.arpa \
--foreman-proxy-dhcp true \
--foreman-proxy-dhcp-managed true \
--foreman-proxy-dhcp-interface __enp0s3__ \
--foreman-proxy-dhcp-range __"192.168.1.150 192.168.1.200"__ \
--foreman-proxy-dhcp-gateway __192.168.1.1__ \
--foreman-proxy-dhcp-nameservers __"192.168.1.140, 8.8.8.8"__ \
--foreman-proxy-tftp true \
--foreman-proxy-tftp-managed true \
--foreman-proxy-tftp-servername __192.168.1.140__

# Provisioning a bare metal host.
# Creating password for provisioning template
You can use mkpasswd to create hashed password for your users
>$ apt-get install whois \
>$ mkpasswd -m sha-512 __[YOUR USER PASSWORD]__ 

Or to make it more secure by adding some generated sand using pwgen
>$ apt-get install pwgen \
>$ mkpasswd -m sha-512 -S $(pwgen -ns 16 1) __[YOUR USER PASSWORD]__
