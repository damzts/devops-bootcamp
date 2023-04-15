# DevOps Bootcamp Capstone Project
  - [1. Setup your development environment](#1-setup-your-development-environment)
  - [2. Containerize the application](#2-containerize-the-application)
  - [3. Create a CI Pipeline](#3-create-a-ci-pipeline)
  - [4. Update "Hello World!" to "Hello DevOps!"](#4-update-hello-world-to-hello-devops)

## Description
In this project, you will be using some of the tools and technologies you have learned during the bootcamp.
> - Node Js application will be provided
> - Containerization using docker
> - CI pipeline using Github Actions (Including automated testing,build)
> - Update the Node JS application content using Ansible
> * [Docker](https://docs.docker.com/desktop/) installed
> * [Git](https://github.com/git-guides/install-git) installed
> * [Node JS](https://nodejs.org/en/download/package-manager/)
> * Have a [github account](https://github.com/join)
> * Code editor of your preference - [VS Code](https://code.visualstudio.com/download) recommended

## 1. Setup your development environment
### Repository ✅
> - Go to the github repository https://github.com/itjuana-bootcamp/DevOps
> - Fork the repository ✅
### Application ✅
> - Go to folder `hello-world` 
> - Run `npm install` . 
> - Run `npm test` . All tests need to pass.  
> - Run `npm start` to run the application. 
> - Check http://localhost:3000 is reachable and displaying "Hello World!" ✅

## 2. Containerize the application ✅
> - Using docker, containerize the application.
> - Build the container and run it to make the application available on http://localhost:3000

In the examples shown at the course, they were using an optimized "npm build" app, hosted over nginx server at default port 80.
I decided to go with the running test environment with npm start localhost 3000 so its easier in step 4, to make changes to the app through ansible.
I'm aware that optimized build is whats really is supposed to be deployd, I performed an ansible-playbook to update src/ in app container but extra steps made me better go easy with the test env.

Anyway, the image is a node slim, half smaller than lts itself from 1,34 Gb to 0,6 Gb. tried to use the alpine version but that doesn't even have apt-get.
### hello-world Dockerfile
```
FROM node:lts-slim 

#install dependencies
RUN apt-get update -y \
    && apt-get upgrade -y \
    && apt-get install -y \
    ssh \
    sshpass \
    sudo \
    software-properties-common

#configure ssh server and users
WORKDIR /home/ansible_controller
RUN useradd -rm -d /home/ansible_controller -s /bin/bash -g root -G sudo -u 1001 ansible_controller
RUN echo ansible_controller:12345 | chpasswd
RUN echo "ansible_controller ALL=(ALL:ALL) NOPASSWD: ALL" | EDITOR="tee -a" visudo
RUN mkdir -p /home/ansible_controller/.ssh

# lets run the helloworld app
WORKDIR /usr/src/app
# Copy dependency definitions
COPY package.json /usr/src/app
COPY package-lock.json /usr/src/app
# Install dependencies
RUN npm install #npm ci didnt work, same error as github actions
# Get all the assets needed to run the app
COPY . /usr/src/app
# Expose the port the app runs in
EXPOSE 3000
EXPOSE 22
# Serve the app
CMD ["/bin/bash","-c", "service ssh start && npm start"] # a previous command can stop the next command so keep it mind.
```

## 3. Create a CI Pipeline ✅
> - Create a CI pipeline which 
>     - will trigger on push and on pull request
>     - Run the tests
>     - Only if tests are success, build the container

### Github Actions Workflow
```
name: understanding workflows
#trigger on push and pull_request
on:
    workflow_dispatch:
    push:
        branches: [ "main" ]
    pull_request:
        branches: [ "main" ]

jobs:
    #install app and run tests
    test:
        name: Test - Dockerize - Push image
        runs-on: ubuntu-latest
        strategy:
            matrix:
                node-version: [16.x]
                # See supported Node.js release schedule at https://nodejs.org/en/about/releases/
        steps:
        - uses: actions/checkout@v3
        - name: Use Node.js ${{ matrix.node-version }}
          uses: actions/setup-node@v3
          with:
            node-version: ${{ matrix.node-version }}
        - name: Install dependencies
#           run: npm ci didnt work, package jsons not in sync, i couldnt fix that problem so instead
          run: npm install
          working-directory: ./capstone_project/hello-world
        - name: Run tests cases
          run: npm test
          working-directory: ./capstone_project/hello-world
        - name: Log in to docker
          env:
            DOCKER_USER: ${{secrets.DOCKER_USER}}
            DOCKER_PASSWORD: ${{secrets.DOCKER_PASSWORD}}
          run: |
            docker login -u $DOCKER_USER -p $DOCKER_PASSWORD
          if: success()
        - name: Set up Docker Buildx
          uses: docker/setup-buildx-action@v2
          if: success()
        - name: Build and push
          uses: docker/build-push-action@v4
          with:
            context: ./capstone_project/hello-world
#             file: ./Dockerfile
            push: true
            tags: ${{secrets.DOCKER_USER}}/devops-bootcamp-docker:latest
          if: success()
```

## 4. Update "Hello World!" to "Hello DevOps!" ✅
> - Update the node js application to display "Hello DevOps!" instead of "Hello World!" using ansible.

```
.
├── ansible_files
│   ...
│   └── setHelloDevOps.yaml
├── ansible_base
│   ...
│   └── Dockerfile
├── hello-world
│   ├── ...
│   └── Dockerfile
├── docker-compose.yaml
└── R
```

