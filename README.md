# docker Jenkins

### GID
- Get the Docker group ID on your host

```sh
stat -c %g /var/run/docker.sock
968
```

### setup

- ARG_DOCKER_GID=968 #replace this number with the one you obtained
- edit Dockerfile.jenkinsfile

```sh
ARG DOCKER_GID=968
```

### deploy

```sh
docker compose up -d
```

- open your browser http://localhost:8080

### logs

- verify logs

```sh
docker compose logs jenkins | head -50
```

- check container

```sh
docker exec -it jenkins-blueocean bash

java -version

java -Xmx768m -Xms256m -version

free -h
```

### destroy

```sh
docker compose down -v
```

### ssh config jenkins

- create ssh standard

```sh
ssh-keygen -t ed25519 -C "yourMachineName@email.com"
```

- Jenkins Setup: Navigate to Manage Jenkins > Credentials > System > Global credentials > Add Credentials.
- Kind: Choose "SSH Username with private key."
- ID: Enter a unique name (e.g., github-ssh-key).
- Username: Enter git.
- Private Key: Select "Enter directly" and paste the entire content of your private key file.

### references

- docker compose specification [deploy with docker swarm](https://docs.docker.com/compose/compose-file/05-services/#deploy)

- docker [compose file legacy](https://docs.docker.com/compose/compose-file/05-services/#deploy)

- install [ssh agent plugin](https://plugins.jenkins.io/ssh-agent/)

- install [sonarqube scanner](https://plugins.jenkins.io/sonar/)
