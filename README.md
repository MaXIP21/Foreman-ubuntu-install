# How to install Foreman 3.0 to Ubuntu 20.04 and provision bare metal servers with it 

# Creating password for provisioning template
You can use mkpasswd to create hashed password for your users
>$ apt-get install whois \
>$ mkpasswd -m sha-512 __[YOUR USER PASSWORD]__ 

Or to make it more secure by adding some generated sand using pwgen
>$ apt-get install pwgen \
>$ mkpasswd -m sha-512 -S $(pwgen -ns 16 1) __[YOUR USER PASSWORD]__
