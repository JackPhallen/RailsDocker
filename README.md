# RailsDocker
Docker setup for Ruby On Rails


## Initial Setup

First we need to build our Docker container. Docker containers provide an isolated linux environment, similar to a virtual box, but without hosting an entire Linux Operating system. You can learn more about Docker here: https://docs.docker.com/

### Step 1: Download

First you need to install Docker for your machine. Simply download Docker Desktop from https://www.docker.com/products/docker-desktop

Next either clone this repository or download the Dockerfile. If you are using bundler, download the Dockerfile from 'withbundler/Dockerfile', otherwise use the Dockerfile in from 'withoutbundler/Dockerfile'.


### Step 2: Build container

Copy Dockerfile into the root directory of your Rails project and run the following command. Be sure to include the '.' at the end of the command!
NOTE: 'railscont' can be replaced with any lowercase name you wish to use for your container name
```
docker build -t railscont .
```

### Step 3: Run container

When you run the container, it will also launch your rails server. To test this out, run the following command.
```
docker run -itP railscont
```

Because the server is running in a Docker container, you need to use the Docker container's port to access the application, not the port configured by Rails. This can be changed later... To see which port the Docker container is running on, run the following command

```
docker ps
```

You will see output similar to this
```
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                     NAMES
39872479e018        railscont            "bundle exec rails s…"   About a minute ago   Up About a minute   0.0.0.0:32773->3000/tcp   wizardly_kilby
```

Take a look at the 'PORTS' column to find the docker port. In this case, my Rails server is accessible at "http://0.0.0.0:32773/"

That is it! You now have a rails server running on Docker. However, you will need to rebuild the docker container (steps 2 and 3) anytime you modify your rails server. In the next step, we will setup a volume to automatically copy new changes to the docker container.

### Step 4: Create volume

To create a volume, run the following command

```
docker run -itP -v $(pwd):/app railscont
```

Now all of our changes will be loaded to the container without rebuilding. You will still need to rebuild if you make changes to the Gemfile or other dependencies.

## Usage

### Run container (also runs Rails server)
```
docker run -itP railscont
```

### Running commands in container

If the container is running, use 'docker exec it railscont' to execute commands in the container. 

```
docker exec -it railscont echo "I'm inside the container!"
```

If the container is not running, use 'docker run -it railscont <command>' 

For example here is how we can create a new controller using the bundler
```
docker run -it railscont bundle exec rails generate controller Welcome index
```


### Open container's bash
Rather than typing "docker exec" before every command, you can attach to the containers bash to run commands inside the container. Just run the following command: 
```
docker exec -it railscont bash
```

Now you can run commands directly inside the container 
```
echo "I'm inside the container!"
```


