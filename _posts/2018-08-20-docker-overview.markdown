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

  html {
    scroll-behavior: smooth;
  }

  a{
    text-decoration:none;
  }

  a:hover, a:active, a:visited, a:focus{
    text-decoration:none;
  }

  ul.contents{
    margin:15px 0px 20px 20px;
    color:#2a7ae2;
  }

  .menu-item{
    font-size:16px;
    font-weight:bold;
    color:#0099ff; 
    color:#1a92bb;
    color:#2a7ae2;
  }

  li a .menu-item:hover{
    text-decoration:none !important;
    color:#0099ff; 
  }
</style>
<hr />  
<p class="menu-item" style="margin-top:15px;">In this post:</p>
<ul class="contents"> 
  <li><a href="#what-is-docker"><span class="menu-item">What is Docker?</span></a></li>  
  <li><a href="#what-is-the-kernel"><span class="menu-item">What is the Kernel?</span></a></li>  
  <li><a href="#what-is-a-vm"><span class="menu-item">What is a VM?</span></a></li>  
  <li><a href="#why-use-docker"><span class="menu-item">Why use Docker?</span></a></li>  
  <li><a href="#basic-terminology"><span class="menu-item">Some Basic Docker Terminology</span></a>  
    <ul>
      <li><a href="#containers"><span class="menu-item">Containers</span></a></li>  
      <li><a href="#images"><span class="menu-item">Images</span></a></li>  
      <li><a href="#docker-hub"><span class="menu-item">Docker Hub</span></a></li>  
      <li><a href="#docker-cli"><span class="menu-item">Docker CLI</span></a></li>  
      <li><a href="#docker-server"><span class="menu-item">Docker Server</span></a></li>  
      <li><a href="#docker-file"><span class="menu-item">DockerFile</span></a></li>  
      <li><a href="#docker-compose"><span class="menu-item">Docker Compose</span></a></li>  
      <li><a href="#volumes"><span class="menu-item">Volumes</span></a></li>  
    </ul></li>
</ul> 
<hr />   
&nbsp;  
<p style="color:#900; font-weight:bold; text-transform:uppercase;">Docker: A brief introduction</p>  
<span id="what-is-docker">**What is Docker?**</span>  
![docker]({{ "assets/images/docker-diagram.png"| absolute_url }}){: .right-image }
Docker is a tool that helps us build, deploy, and run applications from within isolated environments on our computer called `Containers`. Containers are very similar to `Virtual Machines (VMs)` but they differ from VMs in that they **share** the kernel of the system they’re running on while allocating different segments of hard disk space and CPU resources similar to VMs.  This gives Docker Containers a significant performance boost when compared with VMs.  


<span id="what-is-the-kernel">**What is the Kernel?**</span>  
Every OS has a `kernel` - the `kernel` is a running software process that governs access between the programs that run on your computer and the physical hardware it is built of. When an application wants to save a file to the hard drive, the `kernel` is responsible for taking that file and persisting it to the hard disk.


![vm]({{ "assets/images/vm-diagram.png"| absolute_url }}){: .right-image }
<span id="what-is-a-vm">**What is a VM?**</span>  
In comparison to Docker, a `virtual machine (VM)` is an emulation of a computer system, it imitates dedicated hardware. Like Docker, the virtual machine is sandboxed from the rest of the system, meaning that the software inside a virtual machine can’t escape or tamper with the computer itself. However, unlike Docker, a `hypervisor` or `virtual machine monitor` (VMM) creates and runs virtual machines, and each VM has its own Guest OS and kernel as shown in the diagram to the right.


&nbsp;  
<p id="why-use-docker" style="color:#900; font-weight:bold; text-transform:uppercase;">Why use Docker?</p>  
Since VMs need to boot a full OS, starting 100 VMs would take much longer than starting the same number of containers that can do this work in subsecond time.  This implies that with Docker, we get performance gains when compared to VMs.  

Additionally, it is often observed that the code written for our application works on the development environment but not on staging or production. The reasons could be missing software, system configuration, different software versions, or any number of additional services. System setups are time consuming, since there are dependencies for different platforms. Also different applications using the same software stack can require varying software versions.  

These and similar cases can be resolved with Docker. We can be assured that the same versions of every tool and program are running within the containers of all members of a distributed team.  These members no longer need to have a certain version of Postgres installed or run tools like RVM or NVM to be in sync with every other member.  

Finally, as mentioned earlier, Docker shares the kernel with host machine, thereby optimizing performance further.  

&nbsp;  
<p id="basic-terminology" style="color:#900; font-weight:bold; text-transform:uppercase;">Some Basic Docker Terminology</p>  
<span id="containers">**Containers**</span>
&nbsp;    
`Containers` can easily be created from `Docker Images` (see below) and guarantee that whatever machine they’re installed on, they will reproduce the same environment time and time again, promoting flexibility and portability.  Developers can rest assured that their containerized application will run on any other machine regardless of custom settings and software versions.

It is important to note that `Docker for Mac` and `Docker for Windows` both run `Linux containers` in a `Linux VM`, and that newer versions of `Docker` are beginning to use the `HyperVisor` to optimize performance, especially in the case of Windows machines.  

&nbsp;  
<span id="images">**Docker Images**</span>  
`Docker Images` are snapshots of a docker container. Users can create a Docker image by running `docker build` in a directory with a `dockerfile`. The image can then by turned into container by calling `docker run` on the image.  

A `Docker image` consists of read-only layers each of which represents a Dockerfile instruction. The layers are stacked and each one is a delta of the changes from the previous layer.  

*Images are immutable.*  If any changes need to be made to the configuration of an image (and subsequently the container), the image needs to be rebuilt from scratch after editing the Dockerfile’s instructions.   

&nbsp;  
<span id="docker-hub">**Docker Hub**</span>  
Docker Hub is the Docker repository of (public and private) Dockerfiles, much like Github is to code.  It is located at at `hub.docker.com`    

&nbsp;  
<p style="color:#900; font-weight:bold; text-transform:uppercase;">The Docker CLI</p>  

In essence, Docker is a platform built around creating and running Containers.  It accomplishes this with the help of the `Docker CLI`.

The first step to working with Docker is installing the Docker CLI tools by downloading the Docker CE (Community Edition) package here: <https://www.docker.com/get-started>.  

&nbsp;  
<span id="docker-cli">**Docker CLI**</span>  
The command line interface can be verified by running `docker version` on the command line in terminal. It allows us to issue commands and sends them to the `Docker Server`.  The Docker CLI must be installed to create and run a Docker project.

&nbsp;  
<span id="docker-server">**Docker Server**</span>  
The Docker Server or `Docker Daemon` in turn does the work of executing these commands, creating images and running containers, etc. We only communicate with the `Docker Server` through the `Docker CLI`.  

&nbsp;  
<p style="color:#900; font-weight:bold; text-transform:uppercase;">Dockerizing a Project</p>  
<span id="docker-file">**DockerFile**</span>  
To dockerize a project, we add a `Dockerfile` to it, and if using more than one `service`, possibly a docker-compose.yml file as well.  

A `Dockerfile` is a file that contains a list of instructions on how to set up a container.  These are commands a user could successively call on the command line to assemble a project if he or she were manually creating the project.  

Dockerfiles can be created from scratch or they can be found on `Docker Hub`.  Dockerfiles can be used as `parent images` to compile or provide a basic framework for other dockerfiles.  

For more information on setting up a Dockerfile, [see this post](/docker/2018/08/27/dockerfile.html).

&nbsp;  
<span id="docker-compose">**Docker Compose**</span>  
Docker Compose is a tool for running multi-container applications.  

`docker-compose.yml` is a yaml file with a set of configurations that is used when starting up and running an application that relies on several tools such as an MVC framework container and a database container.    

In this situation, we might call the framework container our `app service` or `web service`, and our database container the `db service`.

For more information on setting up a docker-compose.yml file, [see this post](/docker/2018/09/03/docker-compose.html).

&nbsp;  
<span id="volumes">**Volumes**</span>  
Much of Docker's magic happens in `volumes`. Since `Docker images and containers` are `stateless` and `immutable`, we need the help of some tool to maintain the state of our container. This is where `Volumes` come in.

Volumes are specified in the `docker-compose.yml` file, and are Docker's way of persisting data. Volumes help to map and copy files from our local system to our container and back, keeping them continually in sync. We can specify volumes to be read-only by adding an `:ro` flag in our `docker-compose.yml`.  If set, this will mean our volumes will not actively map and copy our local files into our container.

In order to continue persisting data and files to volumes, it is important to use the same containers once they are built and created. The initial create command is `docker-compose up`.  

After that, it is recommended to `start` and `stop` containers gracefully by running `docker start <container_name>` or `docker stop <container_name>`.  
