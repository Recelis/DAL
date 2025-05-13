# Containerisation

Containers are environments in a Linux operating systems which are scoped using `Linux namespaces`. This means that resources cannot be shared outside of a container.

<!-- Look into Linux namespaces more. -->

## Docker

Docker is basically a front-end for `containerd` which creates and manages the running of `containers`.

There are two aspects to Docker, the open sourced docker engine and Docker Desktop which is a propriety desktop app for managing your containers.

I use `Colima` as my docker runtime because of MacOS (but can be used for Linux as well).

There are a few parts to a dockerised system: the `Dockerfile` which is used to build the `docker image`. There is also the `compose` file which is a plugin that allows you to manage multi-container Docker images.

### docker compose

The docker compose file `compose.yml` file uses YAML to define the different services that you use.

#### Building Docker-compose images

To build your images:

```bash
docker-compose build
```

When referencing your Dockerfile in e.g. /app from your docker-compose in your root directory. Your Dockerfile should still treat everything as though it were in your root.

i.e.

```Dockerfile
WORKDIR /app
COPY ./app/package*.json ./

RUN yarn

COPY ./app ./
```

#### Running Docker Compose Containers

To run your images:

```bash
docker-compose up

# You can combine building with running with:
docker-compose up --build
```

#### Stopping your Containers

To stop your images:

```bash
docker-compose stop
```

#### Stopping and clearing your Docker container data

If you want to clear your images:

```bash
docker-compose down
```

### Docker profiles

You can set a profile for a docker compose service.

```yml
db:
  image: postgres
  environment:
    POSTGRES_USER: user
    POSTGRES_PASSWORD: 123
  volumes:
    - postgres:/data/postgres
  ports:
    - "5432:5432"
  profiles:
    - dev
```

To run this:

```bash
 docker-compose --profile dev up --build -d # running in detached mode or auto builds is optional

 # to shut down, you'll need to call the profile again
 docker-compose --profile dev down
```

## containerd

This is a program written primarily in `go` for managing multiple containers.

## runc

Containerd calls `runc`, another go program which actually creates and destroys `containers`.

## OCI Open Container Initiative

This is the standards to which containers are built by.
