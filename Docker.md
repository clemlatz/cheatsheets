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

Build image from a Dockerfile (+ with tag)

```console
docker build -t username/image .
docker build -t username/image:tag .
```

Tag an image

```console
docker tag username/image username/image:tag
```

List available Docker images

```console
docker image ls
docker images
```

Run a container from an image (+ in detached mode)

```console
docker run -p 4000:80 username/image
docker run -d -p 4000:80 username/image
```

List Docker containers (running, all, all in quiet mode)

```console
docker ps
docker ps -a
docker ps -aq
```

Stop a container

```console
docker stop {container}
```

Push an image to Docker hub

```console
docker push username/image:tag
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

