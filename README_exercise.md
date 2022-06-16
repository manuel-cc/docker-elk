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

## Generate SSL internal communication with ELK stack

- Add tls commit from tls branch
--> To go faster on git cherry-pick, I call this readme README_exercice.md, to avoid collision on cherry pick
- Follow instructions on tls/README.md
- Verify on the docker compose logs that the logs are correct:
- TEST: sudo docker compose logs -f -t
- TEST: "(tmux) Ctrl-B - [ - Ctrl-S" to search for log on tmux

- TEST: second verification: calling "curl https://localhost:9200" (ElasticSearch) and "curl https://localhost:5601" (Kibana)

Elastic search was open successfully, Kibana no.

- Attempt for kibana https external:
- follow https://kifarunix.com/enable-kibana-https-connection/#elasticsearch-util after generating p12 certificate from ELK tools
- Update docker compose to add the newly certificate + update kibana configuration
- restart the docker compose