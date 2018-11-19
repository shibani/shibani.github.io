---
layout: post
title:  "The Dockerfile"
date:   2018-08-27 15:23:55 -0500
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
  <li><a href="#what-is-a-dockerfile"><span class="menu-item">What is a Dockerfile?</span></a></li>  
  <li><a href="#example"><span class="menu-item">An Example of a Dockerfile for an Elixir-Phoenix-React project</span></a>
    <ul>
      <li><a href="#installing-elixir"><span class="menu-item">Installing Elixir</span></a></li>  
      <li><a href="#installing-phoenix"><span class="menu-item">Installing the Phoenix framework</span></a></li>  
      <li><a href="#apt-get-update"><span class="menu-item">Installing required Linux update utilities</span></a></li>  
      <li><a href="#downloading-node"><span class="menu-item">Downloading Node, required by React</span></a></li>  
      <li><a href="#apt-cli"><span class="menu-item">Installing required Command Line Utilities</span></a></li>  
      <li><a href="#installing-node"><span class="menu-item">Installing Node</span></a></li>  
      <li><a href="#installing-build"><span class="menu-item">Installing Linux build tools</span></a></li>  
      <li><a href="#installing-watch"><span class="menu-item">Installing Linux watch and notify tools</span></a></li>  
      <li><a href="#installing-rebar"><span class="menu-item">Installing Rebar, required by Elixir</span></a></li>  
      <li><a href="#setting-app-home"><span class="menu-item">Setting a variable to point to the app's working directory</span></a></li>  
      <li><a href="#creating-app-home"><span class="menu-item">Creating the app's working directory</span></a></li>  
      <li><a href="#cd-app-home"><span class="menu-item">Cding into the app's working directory once created</span></a></li>  
      <li><a href="#expose-port"><span class="menu-item">Exposing a port to run your project at</span></a></li>  
      <li><a href="#cmd-or-entrypoint"><span class="menu-item">CMD or Entrypoint</span></a></li>
    </ul></li>  
  <li><a href="#how-to-run"><span class="menu-item">How to run this Dockerfile</span></a></li>
  <li><a href="#when-to-run"><span class="menu-item">When to run this Dockerfile</span></a></li> 
</ul> 
<hr />   
&nbsp;  
<span style="color:#900; font-weight:bold; text-transform:uppercase;">The Dockerfile</span>  
<span id="what-is-a-dockerfile">**What is a Dockerfile?**</span>  

A Dockerfile is a text document containing a list of commands that execute in succession to create a Docker image. Ordinarily, when creating a project, a user would call each of these commands in terminal manually to build the project, but with docker this is automated, and when the `dockerfile` is run, each command that has been added into the script runs in order, resulting in a fully assembled image upon successful execution. If any of the steps fail, however, a non-operational image is created, which then must be deleted and rebuilt after the failing step is fixed. This illustrates the immutable nature of `Docker images and containers`.

Consider the Dockerfile below, which I used to build an image for an Elixir-Phoenix-React project.  This dockerfile is very different from one that would be used to, say, build a website in nginx, but it serves as an example of some of the tools and utiilities we would need to include to set up our environment for the entire project.  
<span id="example"></span>
```bash
FROM elixir:1.7.4

RUN mix local.hex --force \
 && mix archive.install --force  https://github.com/phoenixframework/archives/raw/master/phx_new-1.3.4.ez \
 && apt-get update \
 && curl -sL https://deb.nodesource.com/setup_8.x | bash \
 && apt-get install -y apt-utils \
 && apt-get install -y nodejs \
 && apt-get install -y build-essential \
 && apt-get install -y inotify-tools \
 && mix local.rebar --force

ENV APP_HOME /app
RUN mkdir -p $APP_HOME
WORKDIR $APP_HOME

EXPOSE 4000

CMD ["mix", "phx.server"]
```  
&nbsp;  
<hr />
&nbsp;  
<span style="color:#900; font-weight:bold; text-transform:uppercase;">What went into this Dockerfile</span>  
<span id="installing-elixir">**Installing Elixir**</span>  
```
FROM elixir:1.7.4  
```  

The very first line in every Dockerfile is a `FROM` command.  The `FROM` line initializes a new build stage and sets the Base Image for your project. The image can be any valid image – it is especially easy to start by pulling an image from `Docker Hub`, located at `https://hub.docker.com`. Here since my project was using Elixir, I was able to search for `elixir` in Docker Hub and use an Elixir image as the starting point of my Dockerfile.  

![dockerhub]({{ "assets/images/docker-hub.png"| absolute_url }})

Note that there are many images for Elixir.  These range from more pared-down versions e.g. `slim` and `alpine`, to full-featured ones. In general, `alpine` versions are built to be the most lightweight. However, what you gain in size and speed by choosing them is often lost in terms of a full set of features. `alpine-elixir-phoenix` is an example of such an image, which gives you an optimized package of both elixir and phoenix but less granular control. Consider for instance that `alpine` versions do not come with `bash` preloaded (as they themselves are built on stripped-down versions of Linux). Full featured and `slim` versions include more bells and whistles, including `bash` and can therefore be easier to navigate around. One of the greatest conveniences `bash` affords is the ability to log into a container `docker exec -it <container name>` in order to inspect files in it. 

<!-- **More on Linux distros and bash:**
If you select an image that supports the bash shell, a great advantage is that you'll be able to ssh into your container and cd into the directories in it. This `Elixir` image is based on `Debian`. -->

**Setting Elixir to 1.7.4 instead of latest**
It is possible to set `Elixir` to pull from the `latest` base image.  This would be specified by saying `FROM elixir:latest`.  However, I chose to set my project to a specific version of Elixir so that subsequent builds of my image (by me or anyone else executing the dockerfile locally in their environment at a later date) would not undergo the risk of version incompatibilities with other software in the container as a consequence of pulling in newer versions of Elixir.  
&nbsp;  
<hr />
&nbsp;  
<span id="installing-phoenix">**Installing the Phoenix framework**</span>  
```
mix archive.install --force  https://github.com/phoenixframework/archives/raw/master/phx_new-1.3.4.ez  
```  

This line installs `Phoenix`  

According to the Phoenix docs, the `mix archive.install` command installs an archive locally, while the `--force` flag forces installation without a shell prompt; primarily intended for automation in build systems. This is helpful if there are interactive commands during the phoenix installation process where user input is required. Docker is allowed to ignore these and use defaults if this flag is included.  Finally we specify the Github repo from where Phoenix can be cloned from.  
&nbsp;  
<span style="font-size:12px;">
  More information can be found at: <https://hexdocs.pm/mix/Mix.Tasks.Archive.Install.html>
</span>  
&nbsp;  
<hr />
&nbsp;  
<span id="apt-get-update">**Installing required Linux update utilities**</span>
```
apt-get update  
```
This command downloads package lists from Linux repositories and updates them to get information on the newest versions of packages and their dependencies. In a nutshell, `apt-get update` doesn't actually install new versions of software. Instead, it updates the package lists for upgrades for packages that need upgrading, as well as new packages that have just come to the repositories.  

**apt**   
`Advanced Package Tool`, or `APT`, is a free software user interface that works with core libraries to handle the installation and removal of software on Debian, Ubuntu and other Linux distributions. APT simplifies the process of managing software on Unix-like computer systems by automating the retrieval, configuration and installation of software packages, either from precompiled files or by compiling source code.

**apt-get**  
`apt-get` is the command-line tool for handling packages, and may be considered the user's "back-end" to other tools using the `APT` library.  
&nbsp;  
<hr />
&nbsp;  
<span id="downloading-node">**Downloading Node, required by React**</span>  
```
curl -sL https://deb.nodesource.com/setup_8.x | bash  
```  
This line downloads `Node.js`, required for `React`
</span>  
&nbsp;  
<hr />
&nbsp;    
<span id="apt-cli">**Installing required Command Line Utilities**</span> 
```
apt-get install -y apt-utils  
```  
The `apt-utils` package contains command line utilities related to package management with `apt`.  
&nbsp;  
<span style="font-size:12px;"> 
More information can be found at: <https://packages.debian.org/jessie/apt-utils>  
<span>
&nbsp;  
<hr />
&nbsp;  
<span id="installing-node">**Installing Node**</span>  
```
apt-get install -y nodejs  
```  
This line installs `Node.js` in `Ubuntu`, which is the particular flavor of `Debian` that we'll be using.   
&nbsp;  
<hr />
&nbsp;  
<span id="installing-build">**Installing Linux build tools**</span>  
      
```
apt-get install -y build-essential  
```  
This package is required for building `Debian` packages.  
More information can be found at: <https://packages.debian.org/jessie/build-essential>  
&nbsp;  
<hr />
&nbsp;  
<span id="installing-watch">**Installing Linux watch and notify tools**</span>   
```
apt-get install -y inotify-tools  
``` 
These programs can be used to monitor, watch and act upon filesystem events in `Ubuntu`  
&nbsp;  
<hr />
&nbsp;  
<span id="installing-rebar">**Installing Rebar, required by Elixir**</span>
```
mix local.rebar --force  
```
`rebar` is an Erlang build tool that makes it easy to compile and test Erlang applications, port drivers and releases. It is included here because it's required by `mix`  

`mix` is a build tool for `Elixir` that enables creating, compiling, and testing projects. It also helps to manage dependencies, among other things.  
&nbsp;  
<hr />
&nbsp;  
<span id="setting-app-home">**Setting a variable to point to the app's working directory**</span>
```
ENV APP_HOME /app  
```  
This line sets an environmental variable named `APP_HOME` to the `/app` directory  
&nbsp;  
<hr />
&nbsp;     
<span id="creating-app-home">**Creating the app's working directory**</span>  
```
RUN mkdir -p $APP_HOME
```  
Here the `-p flag` tells mkdir to create any directories of subdirectories specified by the command. `-p` in this case is short for `parents`. In this situation however, we're just creating the one `/app` directory so the `-p` flag does not make a difference.  
&nbsp;  
<hr />
&nbsp;  
<span id="cd-app-home">**Cding into the app's working directory once created**</span>   
```
WORKDIR $APP_HOME
```  
We now tell the script to set `/app` as the `working directory`.  It will cd into the `working directory` and leave us in the here once all commands are run.  
&nbsp;  
<hr />
&nbsp;  
<span id="expose-port">**Exposing a port to run your project at**</span>
```
EXPOSE 4000
```  
We then expose a port on the host machine, here we set it to 4000. We will now be able to view our project in a browser when we navigate to localhost:4000.  
&nbsp;  
<hr />
&nbsp;  
<span id="cmd-or-entrypoint">**CMD or Entrypoint**</span>
```
CMD ["mix", "phx.server"]
```
Finally, we create the `CMD` instruction. Dockerfiles should specify at least one of a `CMD` or an `ENTRYPOINT` instruction. An `ENTRYPOINT` instruction can be defined when using the container as an executable. A `CMD` instruction can be used for executing a custom command in a container. In this case our custom command is `mix phx.server` which will get our project up and running so we can access it at localhost:4000.  
&nbsp;  
<hr />
&nbsp;  
<span id="how-to-run" style="color:#900; font-weight:bold; text-transform:uppercase;">HOW TO RUN THIS DOCKERFILE</span>  

Every `instruction` in the Dockerfile we just covered creates a new layer. We can now run this file by typing `docker build -t <name_of_your_image> .` in terminal. Here the `-t flag` sets the `tag` or name of your project. Also note the trailing period in this instruction which indicates the folder where the `Dockerfile` is located. 

You should now be able to run `docker images` in terminal and see your newly created image listed with the `tag` that you assigned it. At this point it is an inert image and is not running.  
&nbsp;  
<hr />
&nbsp;  
<span id="when-to-run" style="color:#900; font-weight:bold; text-transform:uppercase;">WHEN TO RUN THIS DOCKERFILE</span>  

Ordinarily this might be a great starting point from which to run our project but we are not ready to do so yet, simply because we need to set up our other `services`. While we've successfully managed to set up the `web app` portion of our project, we still have to set up our `database`. We can accomplish this by creating another file that specifies our various `services`. This file is our `Docker Compose` file.  
&nbsp;  
**docker-compose.yml** will be discussed in the next post.
 