# DevOps Bootcamp Capstone Project
  - [1. Setup your development environment](#1-setup-your-development-environment)
    - [Repository](#repository)
    - [Application](#application)
  - [2. Containerize the application](#2-containerize-the-application)
  - [3. Create a CI Pipeline](#3-create-a-ci-pipeline)
  - [4. Update "Hello World!" to "Hello DevOps!"](#4-update-hello-world-to-hello-devops)

## Description
> In this project, you will be using some of the tools and technologies you have learned during the bootcamp.
- Node Js application will be provided
- Containerization using docker
- CI pipeline using Github Actions (Including automated testing,build)
- Update the Node JS application content using Ansible
* [Docker](https://docs.docker.com/desktop/) installed
* [Git](https://github.com/git-guides/install-git) installed
* [Node JS](https://nodejs.org/en/download/package-manager/)
* Have a [github account](https://github.com/join)
* Code editor of your preference - [VS Code](https://code.visualstudio.com/download) recommended

## 1. Setup your development environment
### Repository ✅
- Go to the github repository https://github.com/itjuana-bootcamp/DevOps
- Fork the repository ✅
### Application ✅
- Go to folder `hello-world` 
- Run `npm install` . 
- Run `npm test` . All tests need to pass.  
- Run `npm start` to run the application. 
- Check http://localhost:3000 is reachable and displaying "Hello World!" ✅

## 2. Containerize the application ✅
- Using docker, containerize the application.
- Build the container and run it to make the application available on http://localhost:3000
> In the examples shown at the course, they were using an optimized "npm build" app, hosted over nginx server at default port 80.
I decided to go with the running test environment with npm start localhost 3000 so its easier in step 4, to make changes to the app through ansible.
I'm aware that optimized build is whats really is supposed to be deployd, I performed an ansible-playbook to update src/ in app container but extra steps made me better go easy with the test env.

Anyway, the image is a node slim, half smaller than lts itself from 1,34 Gb to 0,6 Gb. tried to use the alpine version but that doesn't even have apt-get.

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
- Create a CI pipeline which 
     - will trigger on push and on pull request
     - Run the tests
     - Only if tests are success, build the container
```
.
├── backend
│   ├── Dockerfile
│   ...
├── compose.yaml
├── frontend
│   ├── ...
│   └── Dockerfile
└── README.md
```

## 4. Update "Hello World!" to "Hello DevOps!" ✅
- Update the node js application to display "Hello DevOps!" instead of "Hello World!" using ansible.

```
.
├── backend
│   ├── Dockerfile
│   ...
├── compose.yaml
├── frontend
│   ├── ...
│   └── Dockerfile
└── README.md
```

