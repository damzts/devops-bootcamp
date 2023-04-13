# DevOps Bootcamp Capstone Project
  - [1. Setup your development environment](#1-setup-your-development-environment)
    - [Repository](#repository)
    - [Application](#application)
  - [2. Containerize the application](#2-containerize-the-application)
  - [3. Create a CI Pipeline](#3-create-a-ci-pipeline)
  - [4. Update "Hello World!" to "Hello DevOps!"](#4-update-hello-world-to-hello-devops)

## Description
In this project, you will be using some of the tools and technologies you have learned during the bootcamp.

## Features and Requirements
- Node Js application will be provided
- Containerization using docker
- CI pipeline using Github Actions (Including automated testing,build)
- Update the Node JS application content using Ansible

## Pre-requisites

* [Docker](https://docs.docker.com/desktop/) installed
  * `docker --version` should show the docker version
* [Git](https://github.com/git-guides/install-git) installed
  * `git --version` should show the git version
* [Node JS](https://nodejs.org/en/download/package-manager/)
  * `npm version` should show the node version
* Have a [github account](https://github.com/join)
* Code editor of your preference - [VS Code](https://code.visualstudio.com/download) recommended

## 1. Setup your development environment
### Repository
- Go to the github repository https://github.com/itjuana-bootcamp/DevOps
- Fork the repository
### Application
- Go to folder `hello-world`
- Run `npm install` .
- Run `npm test` . All tests need to pass.
- Run `npm start` to run the application.
- Check http://localhost:3000 is reachable and displaying "Hello World!"

## 2. Containerize the application
- Using docker, containerize the application.
- Build the container and run it to make the application available on http://localhost:3000

## 3. Create a CI Pipeline 
- Create a CI pipeline which 
     - will trigger on push and on pull request
     - Run the tests
     - Only if tests are success, build the container

## 4. Update "Hello World!" to "Hello DevOps!"
- Update the node js application to display "Hello DevOps!" instead of "Hello World!" using ansible.

