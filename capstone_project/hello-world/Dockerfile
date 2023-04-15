FROM node:lts-slim 

RUN apt-get update -y \
    && apt-get upgrade -y \
    && apt-get install -y \
    ssh \
    sshpass \
    sudo \
    software-properties-common

WORKDIR /home/ansible_controller

RUN useradd -rm -d /home/ansible_controller -s /bin/bash -g root -G sudo -u 1001 ansible_controller
RUN echo ansible_controller:12345 | chpasswd
RUN echo "ansible_controller ALL=(ALL:ALL) NOPASSWD: ALL" | EDITOR="tee -a" visudo
RUN mkdir -p /home/ansible_controller/.ssh

# Done with ansible config lets Create app directory
WORKDIR /usr/src/app
# Copy dependency definitions
COPY package.json /usr/src/app
COPY package-lock.json /usr/src/app
# Install dependecies
RUN npm install # again npm ci didnt work, as same at github actions
# Get all the code needed to run the app
COPY . /usr/src/app
# Expose the port the app runs in
EXPOSE 3000
EXPOSE 22
# Serve the app
#CMD ["/bin/bash","-c", "service ssh start && tail -f /dev/null && npm start"]
CMD ["/bin/bash","-c", "service ssh start && npm start"]
