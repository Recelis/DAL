# Containerisation

Containers are environments in a Linux operating systems which are scoped using `Linux namespaces`. This means that resources cannot be shared outside of a container.

<!-- Look into Linux namespaces more. -->

## Docker

Docker is basically a front-end for `containerd` which creates and manages the running of `containers`.

There are two aspects to Docker, the open sourced docker engine and Docker Desktop which is a propriety desktop app for managing your containers.

I use `Colima` as my docker runtime because of MacOS (but can be used for Linux as well).

There are a few parts to a dockerised system: the `Dockerfile` which is used to build the `docker image`. There can also be the `compose` file which is a plugin that allows you to manage multi-container Docker images if you are using `Docker Compose`.

### docker ps

To view all running docker containers, use command `docker ps`.

### .dockerignore

The dockerignore file ignores files from being copied into the docker container. There should be one corresponding to each Dockerfile.

#### Docker build

[docs](https://docs.docker.com/get-started/docker-concepts/building-images/build-tag-and-publish-an-image/)

Before you can run your Docker containers, you need to build the images.

This is `docker build .`

This will return a message with a hash for the container name.

`Successfully built d2ff6fae5b55`

Then you can run the container:

`docker run d2ff6fae5b55`

#### Docker build with tagging

You can tag the image with a more human readable name.

`docker build -t my-app --target prod .`

Here you've given the tag my-app and the target prod. Targets will be explained in the multistage build section.

You can now this more easily as:

`docker run -p 80:80 my-app`
This command also sets the port mapping i.e. the port 80 of the container is now exposed to the port 80 in the host.

You can also give your tags a namespace too to group them together.

`docker build -t my-username/my-image .`

### docker compose

The docker compose file `compose.yml` file uses YAML to define the different services that you use.

```yml
services:
  app:
    user: root
    build:
      target: dev
    ports:
      - "3000:3000"
    volumes:
      - ./app:/app
      - /app/node_modules
    environment:
      NODE_ENV: development
    command: yarn dev
  api:
    user: root
    build:
      target: dev
    environment:
      NODE_ENV: development
    volumes:
      - ./api:/api
      - /api/node_modules
    command: yarn dev
```

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

#### Multi-environment Docker-compose

You can specify multiple environments by first having your shared `docker compose` file and then creating a more specific `docker-compose.prod.yml` file.

```yml
services:
  app:
    build:
      target: prod
    ports:
      - "80:80"
    environment:
      NODE_ENV: production

  api:
    build:
      target: prod
    environment:
      NODE_ENV: production
    command: yarn run start
```

#### Building and running your multi-environment Docker-compose

```yml
# Build your prod image first
docker-compose -f docker-compose.yml -f docker-compose.prod.yml build
# Run the prod container
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up
```

This will copy your docker-compose.prod.yml file over your docker-compose.yml file overriding anything that is shared.

#### Multi-stage Docker deployments

The `build: target: prod` in the docker-compose file relates to the build target. So for the app, we will build to the prod target. This prod target is defined as a **stage** in the Dockerfile.

```bash
# Install deps first for better caching
COPY package*.json ./
RUN yarn install

# Copy rest of app (in dev, you’ll override this with a bind mount)
COPY . .

RUN chown -R node:node /app
USER node

EXPOSE 3000
CMD ["yarn", "dev"]

# =========================
# Build Stage (for Production)
# =========================
FROM dev AS build
RUN yarn build

# =========================
# Production Nginx Stage
# =========================
FROM nginx:alpine AS prod

WORKDIR /usr/share/nginx/html

# Copy built React files into Nginx folder
COPY --from=build /app/dist .

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

##### Building off different stages

When setting a stage as `prod` and setting the target to `prod`, you can make sure that all the previous stages have been run by defining your stages as dependent on each other.

e.g. `FROM dev AS build`

When viewing the entire file:

```bash
FROM node:24-alpine AS dev

WORKDIR /api
COPY ./package.json .

RUN yarn install

COPY . .

CMD ["yarn", "dev"]


FROM dev AS build
RUN yarn run build

FROM build AS prod

CMD ["yarn", "start"]
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
