# Installing RabbitMQ on Ubuntu 15. 10

Here is guide and useful links that may help you when installing RabbitMQ on older version of Ubuntu, especially in Ubuntu 15.10.

As far i know there is no tutorial out there that explaining about how to install RabbitMQ in Ubuntu 15.10, and I decided to use tutorial installation for Ubuntu 18.04.

I also have tried tutorial for Ubuntu 20.04 and above and it appears did not work, I think because the installation package has too many differences and simply it is not compatible to Ubuntu 15.10.

Ok, let's begin :

**Installing Erlang**

Add the following source list : 

```
echo "deb https://packages.erlang-solutions.com/ubuntu bionic contrib" | sudo tee /etc/apt/sources.list.d/erlang-solution.list
```

And then update the local package cache with the newly added repository : 

```
sudo apt update
```

Install erlang :

```
sudo apt install erlang
```

Once the installation finished, you can test it out with this command :

```
erl
```

And it will shows something like this:

![image](https://user-images.githubusercontent.com/6629806/186396380-ca0bff0d-8fd3-464b-bbe3-5a31c3cf8ab0.png)


**Preparation**

Before we installing RabbitMQ, lets make sure configuration of your host is correct, because it can cause an error if something missing.

Type `hostname` in your terminal, you'll see your hostname, after that edit your host and make sure the IP Address and the hostname are correctly mapped. Edit your host file with nano editor :

 ```
 nano /etc/hosts
 ```
Here is my configuration looks like :
![image](https://user-images.githubusercontent.com/6629806/186467800-ebaeff1d-b8e4-45d3-beed-acad4b4c7613.png)


**Installing RabbitMQ**

And from now on, I'll follow instruction from RabbitMQ official page to install the RabbitMQ package on Ubuntu 18.04 (you can see the tutorial here [*Using RabbitMQ Apt Repositories on Cloudsmith*](https://www.rabbitmq.com/install-debian.html#apt-cloudsmith)). 

I have tried so many other tutorials and just end up with errors. One of the reason is the repo for the packages is already dead, since the version of Ubuntu it self is no longer supported.

Following the instruction below i also head up with some errors, but I'll mention it later how to deal with it, in case you face it too.

Let's start with installing this package :
```
sudo apt-get install curl gnupg apt-transport-https -y
```

Team RabbitMQ's main signing key :
```
curl -1sLf "https://keys.openpgp.org/vks/v1/by-fingerprint/0A9AF2115F4687BD29803A206B73A36E6026DFCA" | sudo gpg --dearmor | sudo tee /usr/share/keyrings/com.rabbitmq.team.gpg > /dev/null
```

If you follow the instruction in RabbitMQ page, you'll find the steps of adding key and repo for erlang, but we will skip it, because we have already install another version of erlang. So just move to another step.

Cloudsmith: RabbitMQ repository : 
```
curl -1sLf https://dl.cloudsmith.io/public/rabbitmq/rabbitmq-server/gpg.9F4587F226208342.key | sudo gpg --dearmor | sudo tee /usr/share/keyrings/io.cloudsmith.rabbitmq.9F4587F226208342.gpg > /dev/null
```

Add apt repositories maintained by Team RabbitMQ : 
```
sudo tee /etc/apt/sources.list.d/rabbitmq.list <<EOF
## Provides RabbitMQ
##
deb [signed-by=/usr/share/keyrings/io.cloudsmith.rabbitmq.9F4587F226208342.gpg] https://dl.cloudsmith.io/public/rabbitmq/rabbitmq-server/deb/ubuntu bionic main
deb-src [signed-by=/usr/share/keyrings/io.cloudsmith.rabbitmq.9F4587F226208342.gpg] https://dl.cloudsmith.io/public/rabbitmq/rabbitmq-server/deb/ubuntu bionic main
EOF
```

Update package indices :
```
sudo apt-get update -y
```

Install rabbitmq-server and its dependencies : 
```
sudo apt-get install rabbitmq-server -y --fix-missing
```


