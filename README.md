# How-to-allow-remote-connections-in-Docker-

## Description

So before starting today's topic, let's have a quick look on what is Docker!!

Containerization is a method which allows us to isolate and run processes. This means, the processes that run on a container will have its own isolation and restrictions.And,Docker is an open source containerization platform that allows us to easily create, deploy and run applications by using containers.

## What are we gonna do today?

Today, we are setting up remote connections in docker such that we can manage the docker in one instance from the other one. 
For that we need two instances, let's say D1 and D2, that have docker installed.

## Steps

Firstly, login to your first instance D1 and let's start configuring its docker to allow remote connections.

### Step 1

Create a directory to store the configuration file using the following command:
```
sudo mkdir -p /etc/systemd/system/docker.service.d
```

### Step 2

Create a new file to store the daemon options:
```
sudo nano /etc/systemd/system/docker.service.d/options.conf
```

### Step 3

Now add the following lines on the file "/etc/systemd/system/docker.service.d/options.conf":
```
ExecStart=
ExecStart=/usr/bin/dockerd -H unix:// -H tcp://0.0.0.0:2375
```

Here, 2375 is a random port. you can use any other port as well.

### Step 4

Finally, reload and restart the docker daemon to save the changes.
```
sudo systemctl daemon-reload
```
```
sudo systemctl restart docker
```

### Step 5

Now, the second part of our task has come. Login to your second instance D2. Use the following command to connect to the Docker daemon of the instance D1:
```
echo "export DOCKER_HOST=tcp://X.X.X.X:2375" >> ~/.bashrc && source ~/.bashrc
```

Make sure to replace the "X.X.X.X" in the above command with the Private IP address of your D1 instance.

Yaaay! The connection has been established. 

Now you can manage the docker of your first instance from your second instance. For example, you will be able to delete the containers/images on your first instance from the second one and also if you pull a new image on this server, you can see them listed in the image list of the first instance.

## Conclusion

So this is how we establish or allow remote connections in Docker. Hope my small piece of work will be useful to you all. 
Thank you!
