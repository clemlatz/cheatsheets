# Docker

## Basic commands

List Docker CLI commands

```console
docker
```

Display Docker version and info

```console
docker --version
docker version
docker info
```

Execute Docker image

```console
docker run hello-world
```

List Docker images

```console
docker images
```

List Docker containers (running, all, all in quiet mode)

```console
docker container ls
docker container ls --all
docker container ls -aq
```

Build image from Dockerfile

```console
docker build -t username/image .
docker build -t username/image:0.1.0 .
```

Run a container from an image

```console
docker run -p 4000:80
```

SSH into a running container

```console
docker exec -ti container-name bash
```

## Dockerfile

```Dockerfile
# Use an official runtime as a parent image
FROM node:10

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install app dependencies from package.json
RUN npm install

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NODE_ENV=production

# Run npm start script when the container launches
CMD ["npm", "start"]
```

