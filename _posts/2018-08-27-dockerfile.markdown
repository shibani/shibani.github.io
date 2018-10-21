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
</style>

<p style="color:#900; font-weight:bold; text-transform:uppercase;">The Dockerfile</p>  
**What is a Dockerfile?**  

A Dockerfile is a text document that contains a list of commands that execute in succession to create a Docker image. Ordinarily, when creating a project, a user would call each of these commands in terminal manually to build the project, but with docker this is automated with each command being added into a script that runs them all in order to assemble the image when executed.  

Consider the Dockerfile below. I used this dockerfile to build an image for an Elixir/Phoenix/React project.

```bash
FROM elixir:1.7.1

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

**What went into this Dockerfile and why -- a line by line explanation**   
&nbsp;  
```
FROM elixir:1.7.1  
```  

Every Dockerfile needs to start with a `FROM` command.  The `FROM` line initializes a new build stage and sets the Base Image for your project. The image can be any valid image – it is especially easy to start by pulling an image from `Docker Hub`. Here since my project was using Elixir, I was able to use an Elixir image found on `Docker Hub` to be the starting point of my Dockerfile.  

Note that there are many images for Elixir.  These range from more pared-down versions (e.g. alpine-elixir, in general alpine versions do not give you access to the bash shell as they themselves are built on stripped-down Linux `distros` that do not come with bash installed) to versions that include more bells and whistles (e.g. alpine-elixir-phoenix, which gives you elixir and phoenix but less granular control)

**More on Linux distros and bash:**
If you select an image that supports the bash shell, a great advantage is that you'll be able to ssh into your container and cd into the directories in it. This `Elixir` image is based on `Debian`.

**Setting Elixir to 1.7.1 instead of latest**
It is possible to set `Elixir` to pull from the `latest` base image.  This would be specified by saying `FROM elixir:latest`.  However, I chose to select a specific version of Elixir so that subsequent builds of my image would not undergo the risk of version incompatibilities by pulling in newer versions of Elixir.
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp; 

```
mix archive.install --force  https://github.com/phoenixframework/archives/raw/master/phx_new-1.3.4.ez  
```  

This line installs `Phoenix`  

According to the Phoenix docs, the `mix archive.install` command installs an archive locally, while the `--force` flag forces installation without a shell prompt; primarily intended for automation in build systems. This is helpful if there are interactive commands during the phoenix installation process where user input is required. Docker is allowed to ignore these and use defaults if this flag is included.  Finally we specify the Github repo from where Phoenix can be cloned from.  
More information can be found at: <https://hexdocs.pm/mix/Mix.Tasks.Archive.Install.html>
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp;  
```
apt-get update  
```
This command downloads package lists from Linux repositories and updates them to get information on the newest versions of packages and their dependencies. In a nutshell, `apt-get update` doesn't actually install new versions of software. Instead, it updates the package lists for upgrades for packages that need upgrading, as well as new packages that have just come to the repositories.  

**apt**   
`Advanced Package Tool`, or `APT`, is a free software user interface that works with core libraries to handle the installation and removal of software on Debian, Ubuntu and other Linux distributions. APT simplifies the process of managing software on Unix-like computer systems by automating the retrieval, configuration and installation of software packages, either from precompiled files or by compiling source code.

**apt-get**  
`apt-get` is the command-line tool for handling packages, and may be considered the user's "back-end" to other tools using the `APT` library.
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp;   
```
curl -sL https://deb.nodesource.com/setup_8.x | bash  
```  
This line downloads `Node.js`, required for `React`
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp;      
```
apt-get install -y apt-utils  
```  
The `apt-utils` package contains command line utilities related to package management with `apt`.  
More information can be found at: <https://packages.debian.org/jessie/apt-utils>
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp; 
```
apt-get install -y nodejs  
```  
This line installs `Node.js` in `Ubuntu`, which is the particular flavor of `Debian` that we'll be using.
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp; 
```
apt-get install -y build-essential  
```  
This package is required for building `Debian` packages.  
More information can be found at: <https://packages.debian.org/jessie/build-essential>
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp; 
```
apt-get install -y inotify-tools  
``` 
These programs can be used to monitor, watch and act upon filesystem events in `Ubuntu`
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp;  
```
mix local.rebar --force  
```
`rebar` is an Erlang build tool that makes it easy to compile and test Erlang applications, port drivers and releases. It is included here because it's required by `mix`  

`mix` is a build tool for `Elixir` that enables creating, compiling, and testing projects. It also helps to manage dependencies, among other things.
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp;  
```
ENV APP_HOME /app  
```  
This line sets an environmental variable named `APP_HOME` to the `/app` directory
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp;  
```
RUN mkdir -p $APP_HOME
```  
Here the `-p flag` tells mkdir to create any directories of subdirectories specified by the command. `-p` in this case is short for `parents`. In this situation however, we're just creating the one `/app` directory so the `-p` flag does not make a difference.
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp;  
```
WORKDIR $APP_HOME
```  
We now tell the script to set `/app` as the `working directory`.  It will cd into the `working directory` and leave us in the here once all commands are run.
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp;  
```
EXPOSE 4000
```  
We then expose a port on the host machine, here we set it to 4000. We will now be able to view our project in a browser when we navigate to port 4000.
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp;  
```
CMD ["mix", "phx.server"]
```
Finally, we create the `CMD` instruction. Dockerfiles should specify at least one of a `CMD` or an `ENTRYPOINT` instruction. An `ENTRYPOINT` instruction can be defined when using the container as an executable. A `CMD` instruction can be used for executing a custom command in a container. In this case our custom command is `mix phx.server` which will get our project up and running so we can access it at localhost:4000.
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp;  
<p style="color:#900; font-weight:bold; text-transform:uppercase;">HOW TO RUN THIS DOCKERFILE</p>  

Every `instruction` in the Dockerfile we just covered creates a new layer. We can now run this file by typing `docker build -t <name_of_your_image> .` in terminal. Here the `-t flag` sets the `tag` or name of your project. Also note the trailing period in this instruction which indicates the folder where the `Dockerfile` is located. 

You should now be able to run `docker images` in terminal and see your newly created image listed with the `tag` that you assigned it. At this point it is an inert image and is not running.  
&nbsp;  
&nbsp;    
&nbsp; 
<p style="color:#900; font-weight:bold; text-transform:uppercase;">WHEN TO RUN THIS DOCKERFILE</p>  

Ordinarily this might be a great starting point from which to run our project but we are not ready to do so yet, simply because we need to set up our other `services`. While we've successfully managed to set up the `web app` portion of our project, we still have to set up our `database`. We can accomplish this by creating another file that specifies our various `services`. This file is our `Docker Compose` file.  
&nbsp;  
&nbsp;  
&nbsp;    
&nbsp; 
**docker-compose.yml** will be discussed in the next post.
&nbsp;  
&nbsp;  
&nbsp;    
&nbsp; 

<!-- **To build your project**  
1. Start by creating a folder for your project. cd into the root.
2. Creating the `Dockerfile` above. <sup>1</sup>       
3. Then create the `docker-compose.yml` file. <sup>2</sup>  
4. Add the mix file and change its permissions to make it executable by running `chmod +x mix` in terminal while in the root directory of your project.   
5. Then run `docker build -t <name_of_your_image> .`.  
Here the -t is a flag that sets the `tag` of your project.   
6. You should now be able to run `docker images` and see your newly created image listed. At this point it is an inert image and is not running.  
7. Now run `docker-compose build` to get an image of your other services like your database, etc created, and your `volumes` built. <sup>3</sup>
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
