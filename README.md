# docker-basics

## Config
###### Python
The python configuration contains filepaths, filenames, and credentials for connecting to the app database.

```
config/config.py
```

## Requirements
###### Python

The `requirements.txt` file is a list of python packages to install with the docker image.

```
requirements.txt
```

## Docker
## Build a single image and run a container
###### Build a single docker image with the Dockerfile

```
cd app
```

```
docker image build -t docker-basics:dev .
```

### Run
###### Run a docker container in detached mode (-d) with an interactive terminal (-it) and mount in the current directory as a volume (-v)

```
cd app
```
```
docker run -dit -v "$(pwd)":/usr/src docker-basics:dev
```

### Show running containers
###### Copy the value of the Container ID

```
docker ps
```

```
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
68f933a02c2b        docker-basics:dev   "python3"           48 seconds ago      Up 47 seconds                           intelligent_stonebraker
```

### Open a shell in the running container
###### To run a command in an existing container use docker exec with interactive terminal (-it) and pass in the Container ID and the name of the shell (bash)

```
docker exec -it 68f933a02c2b bash
```

###### Inside the container run the example python script
```
bash-4.4# python example.py
Hello! Basic Dockerized python environment worked correctly.
```

###### Alternatively the container can be run and a shell opened in the same command

```
docker run -dit -v "$(pwd)":/usr/src docker-basics:dev bash
```

###### Stop the running container

```
docker stop 68f933a02c2b
```

## Build and run one or more images simultaneously
###### A docker-compose configuration file can be used to build multiple images and run multiple containers simultaneously

The docker-compose builds and runs multiple images simultaneously. The app service `build` `context` option is set to the ./app directory. Again the same `Dockerfile` is used to build the app image. This time we are using docker compose to build and run the image.

```
    build:
      context: ./app
      dockerfile: Dockerfile
```

The app service volumes option `host:container` mounts in the `./app` directory as a volume into the container in `/usr/src`. This is similar to the `-v` option in the `docker run` command that was run previously.

```
    volumes:
      - './app:/usr/src'
```

A new service `db` has been added to illustrate the use of docker compose. The db service runs on a public image that is built and published already.

###### Start
`docker-compose -f docker-compose.yml up -d`

###### Show running containers
```
docker ps
```

###### Open a shell
```
docker exec -it 0f1b67634235 bash
```

###### Inside the container run the example python script
```
bash-4.4# python example.py
Hello! Basic Dockerized python environment worked correctly.
```

###### Stop
`docker-compose -f docker-compose.yml stop`
`docker-compose -f docker-compose.yml rm -f`
