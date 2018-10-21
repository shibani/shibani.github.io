---
layout: post
title:  "Docker - An Overview"
date:   2018-08-20 15:23:55 -0500
categories: docker
---
<style type="text/css">
  .left-image {
    float:left;
    margin-left:20px;
    width:400px;
  }
  .right-image {
    float:right;
    margin-left:20px;
    width:400px;
  }
</style>

<p style="color:#900; font-weight:bold; text-transform:uppercase;">Docker: A brief introduction</p>  
**What is Docker?**  
![docker]({{ "assets/images/docker-diagram.png"| absolute_url }}){: .right-image }
Docker is a tool that helps us build, deploy, and run applications from within isolated environments on our computer called `Containers`. Containers are very similar to `Virtual Machines (VMs)` but they differ from VMs in that they **share** the kernel of the system they’re running on while allocating different segments of hard disk space and CPU resources similar to VMs.  This gives Docker Containers a significant performance boost when compared with VMs.  


**What is the Kernel?**  
Every OS has a `kernel` - the `kernel` is a running software process that governs access between the programs that run on your computer and the physical hardware it is built of. When an application wants to save a file to the hard drive, the `kernel` is responsible for taking that file and persisting it to the hard disk.


![vm]({{ "assets/images/vm-diagram.png"| absolute_url }}){: .right-image }
**What is a VM?**  
In comparison to Docker, a `virtual machine (VM)` is an emulation of a computer system, it imitates dedicated hardware. Like Docker, the virtual machine is sandboxed from the rest of the system, meaning that the software inside a virtual machine can’t escape or tamper with the computer itself. However, unlike Docker, a `hypervisor` or `virtual machine monitor` (VMM) creates and runs virtual machines, and each VM has its own Guest OS and kernel as shown in the diagram to the right.


&nbsp;  
<p style="color:#900; font-weight:bold; text-transform:uppercase;">Why use Docker?</p>  
Since VMs need to boot a full OS, starting 100 VMs would take much longer than starting the same number of containers that can do this work in subsecond time.  This implies that with Docker, we get performance gains when compared to VMs.  

Additionally, it is often observed that the code written for our application works on the development environment but not on staging or production. The reasons could be missing software, system configuration, different software versions, or any number of additional services. System setups are time consuming, since there are dependencies for different platforms. Also different applications using the same software stack can require varying software versions.  

These and similar cases can be resolved with Docker. We can be assured that the same versions of every tool and program are running within the containers of all members of a distributed team.  These members no longer need to have a certain version of Postgres installed or run tools like RVM or NVM to be in sync with every other member.  

Finally, as mentioned earlier, Docker shares the kernel with host machine, thereby optimizing performance further.  

&nbsp;  
<p style="color:#900; font-weight:bold; text-transform:uppercase;">Some Basic Docker Terminology</p>  
**Containers**
&nbsp;    
`Containers` can easily be created from `Docker Images` (see below) and guarantee that whatever machine they’re installed on, they will reproduce the same environment time and time again, promoting flexibility and portability.  Developers can rest assured that their containerized application will run on any other machine regardless of custom settings and software versions.

It is important to note that `Docker for Mac` and `Docker for Windows` both run `Linux containers` in a `Linux VM`, and that newer versions of `Docker` are beginning to use the `HyperVisor` to optimize performance, especially in the case of Windows machines.  

&nbsp;  
**Docker Images**  
`Docker Images` are snapshots of a docker container. Users can create a Docker image by running `docker build` in a directory with a `dockerfile`. The image can then by turned into container by calling `docker run` on the image.  

A `Docker image` consists of read-only layers each of which represents a Dockerfile instruction. The layers are stacked and each one is a delta of the changes from the previous layer.  

*Images are immutable.*  If any changes need to be made to the configuration of an image (and subsequently the container), the image needs to be rebuilt from scratch after editing the Dockerfile’s instructions.   

&nbsp;  
**Docker Hub**  
Docker Hub is the Docker repository of (public and private) Dockerfiles, much like Github is to code.  It is located at at `hub.docker.com`    

&nbsp;  
<p style="color:#900; font-weight:bold; text-transform:uppercase;">The Docker CLI</p>  

In essence, Docker is a platform built around creating and running Containers.  It accomplishes this with the help of the `Docker CLI`.

The first step to working with Docker is installing the Docker CLI tools by downloading the Docker CE (Community Edition) package here: <https://www.docker.com/get-started>.  

&nbsp;  
**Docker CLI**  
The command line interface can be verified by running `docker version` on the command line in terminal. It allows us to issue commands and sends them to the `Docker Server`.  The Docker CLI must be installed to create and run a Docker project.

&nbsp;  
**Docker Server**  
The Docker Server or `Docker Daemon` in turn does the work of executing these commands, creating images and running containers, etc. We only communicate with the `Docker Server` through the `Docker CLI`.  

&nbsp;  
<p style="color:#900; font-weight:bold; text-transform:uppercase;">Dockerizing a Project</p>  
**DockerFile**  
To dockerize a project, we add a `Dockerfile` to it, and if using more than one `service`, possibly a docker-compose.yml file as well.  

A `Dockerfile` is a file that contains a list of instructions on how to set up a container.  These are commands a user could successively call on the command line to assemble a project if he or she were manually creating the project.  

Dockerfiles can be created from scratch or they can be found on `Docker Hub`.  Dockerfiles can be used as `parent images` to compile or provide a basic framework for other dockerfiles.  

For more information on setting up a Dockerfile, [see this post](/docker/2018/08/27/dockerfile.html).

&nbsp;  
**Docker Compose**  
Docker Compose is a tool for running multi-container applications.  

`docker-compose.yml` is a yaml file with a set of configurations that is used when starting up and running an application that relies on several tools such as an MVC framework container and a database container.    

In this situation, we might call the framework container our `app service` or `web service`, and our database container the `db service`.

For more information on setting up a docker-compose.yml file, [see this post](/docker/2018/09/03/docker-compose.html).

&nbsp;  
**Volumes**  
Much of Docker's magic happens in `volumes`.  

Volumes are specified in the `docker-compose.yml` file, and are Docker's way of persisting data. Volumes help to map and copy files from your local system to your container and back, keeping them continually in sync. You can specify volumes to be read-only by adding an `:ro` flag in your `docker-compose.yml`.  If set, this will mean your volumes will not actively map and copy your local files into your container.

In order to continue persisting data and files to volumes, it is important to use the same containers once they are built and created. The initial create command is `docker-compose up`.  

After that, it is recommended to `start` and `stop` your containers gracefully by running `docker start <container_name>` or `docker stop <container_name>`.  


<!-- **To build your project**  
1. Start by creating a folder for your project. cd into the root.
2. Creating the `Dockerfile` above. <sup>1</sup>       
3. Then create the `docker-compose.yml` file. <sup>2</sup>  
4. Add the mix file and change its permissions to make it executable by running `chmod +x mix` in terminal while in the root directory of your project.   
5. Then run `docker build -t <name_of_your_image> .`.  
Here the -t is a flag that sets the `tag` of your project.   
6. You should now be able to run `docker images` and see your newly created image listed. At this point it is an inert image and is not running.  
7. Now run `docker-compose build` to get an image of your other services like your database created, and your `volumes` built. <sup>3</sup>
8. In the root directory, run
`./mix deps.get  
./mix ecto.create  
./mix ecto.migrate`  
9. cd into the `src` folder and run `cd assets && npm install`
10. Finally cd one level back up to your root directory and run `docker-compose up`  

**Your container should now be up.**  
- Verify this by running `docker container ls`.  You should see 2 containers up and running.  
- Navigate to `localhost:4000` to view your site in a browser

**To start and stop your containers**   -->
