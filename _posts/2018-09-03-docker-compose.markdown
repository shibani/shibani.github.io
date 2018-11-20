---
layout: post
title:  "Docker Compose"
date:   2018-09-03 15:23:55 -0500
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

  .subtitle{
    font-size:14px;
    font-weight: bold;
    display:block;
    margin-top:20px;
    margin-bottom:0px;
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
  <li><a href="#what-is-it"><span class="menu-item">What is Docker Compose?</span></a></li>  
  <li><a href="#example"><span class="menu-item">An example</span></a></li>  
  <li><a href="#version"><span class="menu-item">Setting the Version</span></a></li>  
  <li><a href="#app-service"><span class="menu-item">Including Services</span></a>
    <ul>
      <li><a href="#app-service"><span class="menu-item">App service</span></a></li>  
      <li><a href="#db-service"><span class="menu-item">DB Service</span></a></li>  
    </ul>
  </li>
  <li><a href="#port"><span class="menu-item">Mapping a Port</span></a></li>  
  <li><a href="#pg-host"><span class="menu-item">Setting the Postgres Database</span></a></li>  
  <li><a href="#volumes"><span class="menu-item">Specifying Volumes</span></a></li>  
  <li><a href="#depends-on"><span class="menu-item">Specifying Dependencies</span></a></li>  
  
  <li><a href="#running-the-project"><span class="menu-item">Running the Project</span></a></li>  
</ul> 
<hr />   
&nbsp;  
<p style="color:#900; font-weight:bold; text-transform:uppercase;">Docker Compose</p>  
<span id="what-is-it">**What is Docker Compose?**</span>  

As mentioned in the previous Docker posts, the final piece of getting a project up and running in Docker is the `Docker Compose` file.  

The `Docker Compose` file is a yml file that specifies all the services required by a project.  This can include the `app` or `web app` service, and the `database` service, as shown in the example below. 
<span id="example"></span> 
```bash
---
version: '3'
services:
  app:
    image: phoenix:1.3.4
    build: .
    ports:
      - "4000:4000"
    environment:
      # Variables to connect to our Postgres server
      PGUSER: postgres
      PGPASSWORD: xxxx
      PGDATABASE: xxxx
      PGPORT: 5432
      # Hostname of our Postgres container
      PGHOST: db
    volumes:
      - ./src:/app
    depends_on:
      - db
  db:
    image: postgres:10
    environment:
      # Set user/password for Postgres
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: xxxx
      # Set a path where Postgres should store the data
      PGDATA: /var/lib/postgresql/data/pgdata
    restart: always
    volumes:
      - pgdata:/var/lib/postgresql/data
# Define the volumes
volumes:
  pgdata:
```    
<span id="version">**Setting the Version**</span>   
The first line of the Docker file declares the version of the file format. Compose files that do not declare a version default to `version 1`, which is a legacy version.  The most current format version is 3.x.  
&nbsp;  
    
**Next we declare our** `services`, **which include our** `app` **and** `db` **configurations.**
&nbsp;  
&nbsp;  
    
<span id="app-service">**App service**</span>   
The `app` service specifies that it is created using `phoenix 1.3.4` as its base image. When `docker-compose up` or `docker-compose build` is called on this file, this script will build the current directory, specified by `build: .`
Here `docker-compose build` will simply build the project, while `docker-compose up` will build the project AND start the containers.
Once the project has been built, it is recommended to start and stop containers gracefully by running docker start <container_name> or docker stop <container_name> as mentioned earlier.  

<span id="port">**Mapping a Port**</span>  
Our `app` configuration also specifies a `PORT` - in this case, the Docker container is being exposed at `port 4000` and then being mapped to the local machine's `port 4000`. We can choose to map the container's `port 4000` to the local machine's `port 3000` by specifying this instead: `"3000:4000"`, where the format follows `HOST:CONTAINER`, in which case we will now be able to access our project by navigating to `localhost:3000` (we initially set our container port in the `Dockerfile` in the line that read `EXPOSE 4000`.  

<span id="pg-host">**Setting the Postgres Database**</span>  
When using databases in a containerized setup, we are no longer mapping to our database at `localhost`. Our database will now be running within our container, and thefore our `PGHOST` is set to `db`.

<span id="volumes">**Specifying Volumes**</span>  
`Volumes` are the preferred mechanism for **persisting data** to docker containers. The `volumes` configuration above is set to `./src:/app`. This implies that whatever is included within the tree under the `./src` folder in our local project will be continuously copied, mapped to and synced with the `/app` directory in our container.  Volumes are managed by Docker and are isolated from the core functionality of the host machine. In this way, our container can reflect all changes that are made to our local project files.  

<span id="depends-on">**Specifying Dependencies**</span>  
Including this setting will ensure that the service specified -- in our case `db` -- will start up before our app does.

<span id="db-service">**DB service**</span>  
Similary, our `db` configuration also specifies `image` (in this case `Postgres 10`), `environment` and `volumes` settings.

It should finally be noted that the `Docker Compose` file can be further customized in order to suit your particular project's configuration, and that the file above is by no means the only variation. It should simply serve as an example to illustrate some of the settings that are commonly used, while others are more specific to the project I was working on.  
&nbsp;  
    
<span id="running-the-project">**Running the Project**</span>  
We are now ready to get our project up and running.  
&nbsp;  
&nbsp;  
    
1. We start by cding into the root of our project folder and running `docker build -t <name_for_the_image> .`
2. We should now be able to run `docker images` and see our newly created image listed. At this point it is an inert image and is not running.  
3. We can next run `docker-compose build`, which will build our container, after which we can list our containers by typing `docker ps -a`
4. The next step might require some project-specific setup such as `./mix deps.get` and `./mix ecto.create`
5. We are now ready to run `docker-compose up` and should be able to see our project if we navigate to `localhost:4000`  
&nbsp;  
&nbsp;  
    
<span style="font-size:12px;">
  For more information on <strong>Docker Compose</strong> and all available configuration options, please visit: <https://docs.docker.com/compose/compose-file/>
</span>  

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

**To start and stop your containers** -->
