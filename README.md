# Installing-Apache-Server-Inside-Of-An-Ubuntu-Docker-Container

Scenario: Level Up Solutions is a dynamic technology company that offers innovative web solutions to its clients. As part of their software development process, they have decided to adopt containerization using Docker to streamline their web application deployment. Docker provides Level Up Solutions with a flexible and efficient way to package, distribute, and run their applications in isolated containers, ensuring consistency and portability across different environments.

# Key Terms to remember:

Docker: A set of platform as a service products that use OS-level virtualization to deliver software in packages called containers.

Container: A standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.

Linux: A family of open-source unix-like operating systems based on the Linux kernel.

Visual Studio Code: A source-code editor developed by Microsoft for Windows, Linux and macOS.

EC2: Amazon Elastic Compute Cloud is a web service that provides secure, resizable compute capacity in the cloud.

Ubuntu: A Linux distribution based on Debian and composed mostly of free and open-source software.

Bash: A Unix shell and command language written by Brian Fox for the GNU Project as a free software replacement for the Bourne shell.

Apache: A software that is both a free and open-source. The software assist users with deploying their websites online.

HTML: HyperText Markup Language or HTML is the standard markup language for documents designed to be displayed in a web browser. It defines the content and structure of web content.

Port 80: Serves as the communication gateway for HTTP requests and responses between client computers and servers.

Port 8080: Serves as the communication gateway for HTTPS requests and responses between client computers and servers.

Docker Hub: Acontainer registry built for developers and open source contributors to find, use, and share their container images

# Docker Commands:

Docker Run: Used to create and start a Docker container based on a specified Docker Image.

Docker Version: The version of Docker installed on your System.

Docker Images: Lists the Docker images that are currently on your system.

Docker PS: Used to list the current running Docker containers.

Docker Pull: Downloads Docker images from the internet.

Docker Push: A command that is used to push or share a local Docker image or a repository to a repository.

Docker Start: Used to start any stopped container.

Docker Login: Log in to any public or private repository for which you have credentials.

# Foundational Tasks:

1. Run a Docker Ubuntu container running detached and on port 80

Note: keep in mind that there are different Ubuntu images. Depending on the one you use, you may have to install some tools that come standard in other images.

2. Using BASH shell update all packages within the container .

3. Using BASH install Apache2 on the Ubuntu container.

4. Add a custom webpage.

5. Verify you can reach the webserver from your browser.

Now that we are aware of what is required let’s get started!!!

Lets install Docker

curl -fsSL https://get.docker.com -o get-docker.sh 
sh get-docker.sh

I will be using the Remote-SSH plugin in VS code in order to access my Docker instance in AWS. 

```
Host Docker
  #IPV4 Address will change every time.
   HostName  18.212.149.38
   User ubuntu
   IdentityFile C:\Users\Babor\Documents\AWS\LevelUpInTech\Docker\DockerKeyPair.pem
```

Lets Create a Docker Container

$ sudo docker container run -d -p 80:80 -it --name luit_docker ubuntu bash

To double check that the container is running we can use this command:

$ sudo docker ps -a

The result from the command above is:

![Snipe 1](https://github.com/Mirahkeyz/Installing-Apache-Server-Inside-Of-An-Ubuntu-Docker-Container/assets/134533695/9bed484d-e92e-46a0-ace2-7907be2c8d20)

Now that we know the container is running, we have to access it to be able to update all packages, install Apache and add a custom webpage. We can accomplish this by running the following command:

$ sudo docker exec -it luit_docker bash

We now have the bash prompt and can start the process of the next task.

$ sudo apt update -y

All package index files are being upgraded.

![Snipe 2](https://github.com/Mirahkeyz/Installing-Apache-Server-Inside-Of-An-Ubuntu-Docker-Container/assets/134533695/1c56cfa7-cdab-4aa1-a1b9-1087cc397e54)

$ apt upgrade -y

Here, all packages are being upgraded.

![Snipe 3](https://github.com/Mirahkeyz/Installing-Apache-Server-Inside-Of-An-Ubuntu-Docker-Container/assets/134533695/f89060c4-7591-4205-9f05-d0726ed58281)

$ apt install apache2 -y

The command above installs Apache server.

![Snipe 4](https://github.com/Mirahkeyz/Installing-Apache-Server-Inside-Of-An-Ubuntu-Docker-Container/assets/134533695/ca796321-3c0b-4f34-81fd-7b71620ad07e)

Task 2 is now completed. The next step is to add a custom webpage.

Before we can pull the file we need to install Git.

$ apt install git -y

![Snipe 5](https://github.com/Mirahkeyz/Installing-Apache-Server-Inside-Of-An-Ubuntu-Docker-Container/assets/134533695/cca978e1-cbfa-4251-bdba-074c2e3d64fb)

Next we need to clone the repo with the index.html file.

$ git clone https://github.com/samcolon/LUITProject3.git

![Snipe 6](https://github.com/Mirahkeyz/Installing-Apache-Server-Inside-Of-An-Ubuntu-Docker-Container/assets/134533695/44d7ece5-0476-4393-b936-1d66ea8245bf)

![Snipe 7](https://github.com/Mirahkeyz/Installing-Apache-Server-Inside-Of-An-Ubuntu-Docker-Container/assets/134533695/4fb0a6b3-1e44-4594-bf6d-56ae944945ac)

Since systemd is not installed. we can use the following command to check on the Apache service.

$ service apache2 status

![Snipe 8](https://github.com/Mirahkeyz/Installing-Apache-Server-Inside-Of-An-Ubuntu-Docker-Container/assets/134533695/d9d9511f-ac17-4d60-bf1c-0b0b20c9b184)

Now run this to start Apache:

$ service apache2 start

After starting Apache head over to a browser and paste in your EC2 instance public IP address.

(Note: make sure you are allowing traffic from your EC2 instance to your IP in the instance security group)

![Snipe 9](https://github.com/Mirahkeyz/Installing-Apache-Server-Inside-Of-An-Ubuntu-Docker-Container/assets/134533695/9ae09d52-4453-4e11-bf54-18d66fccce5c)

 We have completed the Foundational section. Lets see what the Advanced section has in store.

# Advanced Tasks:

1. Push to Docker Hub.

2. Pull your new image and run on a different port than 80 while still mapping to 80 on the container. Verify you can reach the webserver using the updated port.

Let’s start by logging into our Docker hub with the command:

$ sudo docker login

Then type in our Docker hub username and password.

Before we can push the image to our repo we need to create an image of the container:

$ sudo docker commit luit_docker

![Snipe 10](https://github.com/Mirahkeyz/Installing-Apache-Server-Inside-Of-An-Ubuntu-Docker-Container/assets/134533695/28b4d9e5-c562-468e-bbd9-db5247b38098)

Next step is to tag the image:

$ sudo docker tag luit_website:latest mirahkeyz/levelupintech:latest

Let’s push the image to the Docker hub repo:

$ sudo docker push samcolon/levelupintech:latest

Next we go to our Docker hub repo to verify that the image is in there.





















































































