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
### Repository
> - Go to the github repository https://github.com/itjuana-bootcamp/DevOps ✅
> - Fork the repository ✅
### Application
> - Go to folder `hello-world` 
> - Run `npm install` . ✅
> - Run `npm test` . All tests need to pass. ✅
> - Run `npm start` to run the application. ✅
> - Check http://localhost:3000 is reachable and displaying "Hello World!" ✅

## 2. Containerize the application
> - Using docker, containerize the application. ✅
> - Build the container and run it to make the application available on http://localhost:3000 ✅

In the examples shown at the course, they were using an optimized "npm build" app, hosted over nginx server at default port 80.
I decided to go with the running test environment with npm start localhost 3000 so its easier in step 4, to make changes to the app through ansible.
I'm aware that optimized build is whats really is supposed to be deployd, plus I performed an ansible-playbook to update /src folder in app container.

Anyway, the image is a node slim, half smaller than lts itself from 1,34 Gb to 0,6 Gb. tried to use the alpine version but that doesn't even have apt-get.

1. [`hello-world Dockerfile`](./hello-world/Dockerfile)
2. to run
```
sudo docker run -p 3000:3000 damzts/devops-bootcamp-docker
```

## 3. Create a CI Pipeline
> - Create a CI pipeline which 
>     - will trigger on push and on pull request ✅
>     - Run the tests ✅
>     - Only if tests are success, build the container ✅

1. [`GitHub Workflow`](../.github/workflows/understanding.yml)
2. [`Docker Hub image`](https://hub.docker.com/r/damzts/devops-bootcamp-docker/tags)
 
## 4. Update "Hello World!" to "Hello DevOps!"
> - Update the node js application to display "Hello DevOps!" instead of "Hello World!" using ansible. ✅

### Filesystem
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
└── Root
```

Build two services:
- ansible_base(controller) and bind ansible_files folder
- hello-world(target1) and redirect helloworld_app 3000 to my 3000

1. [`docker-compose.yaml`](./docker-compose.yaml)
2. to run
```
sudo docker-compose up
```
3. to access ansible_controller
```
sudo docker exec -w /home/ansible_controller/ansible_files/ -ti ansible_controller bash
```
4. to perfom app update
```
ansible-playbook -i inventory.ini setHelloDevOps.yaml
```

