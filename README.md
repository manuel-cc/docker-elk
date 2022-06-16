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

12:22 - almost there, with curl on kibana url, I hit SSL get record issue
---
 curl -sk -vvv "https://kibana.test1.thehip.app:5601"
*   Trying 159.100.241.166:5601...
* TCP_NODELAY set
* Connected to kibana.test1.thehip.app (159.100.241.166) port 5601 (#0)
* ALPN, offering h2
* ALPN, offering http/1.1
* successfully set certificate verify locations:
*   CAfile: /etc/ssl/certs/ca-certificates.crt
  CApath: /etc/ssl/certs
* TLSv1.3 (OUT), TLS handshake, Client hello (1):
* error:1408F10B:SSL routines:ssl3_get_record:wrong version number
* Closing connection 0
---
But the url works properly for HTTP protocol.
---
curl -sk -vvv "http://kibana.test1.thehip.app:5601"
*   Trying 159.100.241.166:5601...
* TCP_NODELAY set
* Connected to kibana.test1.thehip.app (159.100.241.166) port 5601 (#0)
> GET / HTTP/1.1
> Host: kibana.test1.thehip.app:5601
> User-Agent: curl/7.68.0
> Accept: */*
>
* Mark bundle as not supporting multiuse
< HTTP/1.1 302 Found
< location: /login?next=%2F
< x-content-type-options: nosniff
< referrer-policy: no-referrer-when-downgrade
< kbn-name: kibana
< kbn-license-sig: 5567f157e090844671f9a1a739d004b044ba49f76fa81f9c897a15faa836f9a2
< cache-control: private, no-cache, no-store, must-revalidate
< content-length: 0
< Date: Thu, 16 Jun 2022 10:23:44 GMT
< Connection: keep-alive
< Keep-Alive: timeout=120
<
* Connection #0 to host kibana.test1.thehip.app left intact

---
