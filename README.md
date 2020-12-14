# Very simple practice of running an ASP.NET Core Web Api with Docker Desktop.

1. For Windows 10 Home, OS build must be 1903 or higher. Noe that Windows container is not supported for Windows 10 Home. You will only be able to use Linux Container from  Docker Desktop and cannot switch to Windows container. 

![container-swich-grayed-out](https://user-images.githubusercontent.com/8789577/101279055-bb0c8680-37e9-11eb-9699-bae004f0d993.jpg)

Why? check this : https://github.com/docker/for-win/issues/9701#event-4076070225

2. Install latest docker desktop (if not installed) : https://docs.docker.com/docker-for-windows/install-windows-home/?fbclid=IwAR3LWY5cZ3agrtom1fFhMxQSGTttImiaMRNDDei6UrNQwMK7BAsw6ZOAndU

3. Create a simple ASP.NET Core Web Api project from Visual Studio 2019 (Leave the docker support option.)

4. To run your app into docker, first you need to build a Docker Image with your app. And then this image needs to run inside docker container : 

4.1 Add Docker support to your project (There will be an OS option to choose. Select Linux - because Windows 10 Home does not support Windows Docker) : 

![add-docker-support](https://user-images.githubusercontent.com/8789577/101279116-4259fa00-37ea-11eb-9e50-0ab917f8fdc6.jpg)

4.2 From Visual Studio, select Tools->Command Line->Developer Command prompt

4.3 Build a docker image for your project with below command = **docker build -f Dockerfile -t mydockertrial:development ..** (https://github.com/Microsoft/DockerTools/issues/167)

here, [mydockertrial] = docker image name, user defined. [developerment] = tag name, user defined.

After this command is executed, you will see your image in Docker Desktop images panel : 

![my-docker-image](https://user-images.githubusercontent.com/8789577/101279262-42a6c500-37eb-11eb-86b8-b26ee8657cc3.JPG)

4.4 Run this image in Docker Container with command : **docker run -p 1234:80 --name testdockercontainer mydockertrial:development**

here, [testdockercontainer] = my docker container name, user defined. 

You will see your container in Docker Desktop container panel : 

![my-docker-container](https://user-images.githubusercontent.com/8789577/101279324-c06ad080-37eb-11eb-96f5-368f87374f41.jpg)

5. Our docker image is ready and running in Dokcer Container. It means we should be able to see : **http://localhost:1234/swagger/index.html**, but there is no response 

![no-response](https://user-images.githubusercontent.com/8789577/101279354-f27c3280-37eb-11eb-82c4-c7cf2c21a958.JPG)

6. To fix it : 
- Comment out the if condition (line#40) from Startup.cs

![strtpu](https://user-images.githubusercontent.com/8789577/101279390-2fe0c000-37ec-11eb-821f-641ca517903e.JPG)

- Stop your running container with command : **docker stop testdockercontainer**
- Repeat step 4.1 to 4.5
- Hit **http://localhost:1234/swagger/index.html**, now you will see response : 

![working-swagger](https://user-images.githubusercontent.com/8789577/101279435-73d3c500-37ec-11eb-9942-3974427019c3.JPG)



