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
</style>

<p style="color:#900; font-weight:bold; text-transform:uppercase;">Docker Compose</p>  
**What is Docker Compose?**  

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
      PGPASSWORD: postgres
      PGDATABASE: yelp_dev
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
      POSTGRES_PASSWORD: postgres
      # Set a path where Postgres should store the data
      PGDATA: /var/lib/postgresql/data/pgdata
    restart: always
    volumes:
      - pgdata:/var/lib/postgresql/data
# Define the volumes
volumes:
  pgdata:
```
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
