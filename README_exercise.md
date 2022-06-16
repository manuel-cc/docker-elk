# Instructions for exercise

## Deploy the production ready ELK stack

* Ansible --> Issue on installing ansible on my machine. Will install at hte end by command line

- installation of docker and docker compose, cf https://docs.docker.com/engine/install/ubuntu/
- create ssh key and clone the repository (custom clone to be able to write on my machine and fetch it on the remote)
- sudo docker compose up -d
- TEST: "docker ps" to check that the containers are running properly

## Make sure present at reboot

* 2 possibility.
- create a cron job
- use "restart: always"

Note: For this exercise, I made the hypothesis that the docker containers won't be stopped normally so the second option would work.

- TEST: Made a reboot of the machine. The instances was spinned up properly at reboot.
