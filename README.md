# foreman-ubuntu

# Creating password for provisioning template
You can use mkpasswd to create hashed password for your users\
>$ apt-get install whois \
>$ mkpasswd -m sha-512 __[YOUR USER PASSWORD]__ 

Or to make it more secure you can add some generated sand by using pwgen \
>$ apt-get install pwgen \
>$ mkpasswd -m sha-512 -S $(pwgen -ns 16 1) __[YOUR USER PASSWORD]__
