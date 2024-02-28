# Docker compose files to run Sonarqube, Jenkins and expose with Ngrok to the Internet

# Other languages / Otros idiomas / En autres langues
[![en](https://img.shields.io/badge/en-español-yellow.svg)](https://github.com/BrainsDevOps/sonarqube-udemy-docker-compose/blob/main/README.md)
[![es](https://img.shields.io/badge/en-español-yellow.svg)](https://github.com/BrainsDevOps/sonarqube-udemy-docker-compose/blob/main/README-es.md)
[![fr](https://img.shields.io/badge/en-français-red.svg)](https://github.com/BrainsDevOps/sonarqube-udemy-docker-compose/blob/main/README-fr.md)


This repository contains docker-compose file to easily run to the code examples in the udemy courses

## Context
This repository is part of my udemy courses:
* [Domina Sonarqube (Spanish)](https://www.udemy.com/course/domina-sonarqube/?referralCode=EF59257E7D8DC3026D6D)
* [Domina Github Actions (Spanish)](https://www.udemy.com/course/domina-github-actions/?referralCode=CBFBAF72C38BE758CFE1)

If you're interested in learning more about Sonarqube or Github actions, you can check if the are any discount coupons available in the
course section of [devopsbrains.com](https://devopsbrains.com/cursos/)

## Requirements
* Docker
* Docker compose
* ngrok account (Optional. Needed only if you want to expose the services in Internet)

# Ngrok configuration
* If you want to expose any of the services in Internet with ngrok, you'll need:
    * An [ngrok](https://ngrok.com/) account with an authentication token and a domain.
    * If you want to know more about ngrok, there is a [Youtube](https://youtu.be/UW8BObHdi08) vídeo available (Spanish), with a quick guide on the subject
    * Create a .env file at the repo root folder to declare your ngrok domain and token 
```
NGROK_AUTHTOKEN=<your token>
NGROK_DOMAIN=<your domain>
```

# Launch modes
* Sonarqube: `docker-compose up`
* Sonarqube + ngrok: `docker-compose --profile public up`
* Jenkins: `docker compose -f compose.jenkins.yaml up`
* Jenkins + ngrok: `docker compose -f compose.jenkins.yaml --profile public up`

NOTE: You'll have a single domain with free ngrok account and you can only have a single ngrok session. So you can only expose either SonarQube or Jenkins at the same time

# First conexion to Jenkins
* After launching Jenkins, you can open in the web browser http://localhost:8080/
* We can skip plugin installation, since the plugins needed for the course exercises are already installed. "Select plugins to install" > "None" > "Install"
* We create an admin user
* We choose the default server URL: "http://localhost:8080/"
* The configuration of credentials and tools such as NodeJs and SonarScanner is explained during the course

# Known problems
* [failure with github checkout](https://github.com/jenkinsci/helm-charts/issues/728)

A student had this problem and managed to solve it, by adding this instruction in the Dockerfile after the FROM insttruction

```
RUN git config --global --add safe.directory "*"
```

* [Plugin download failure](https://community.jenkins.io/t/issue-while-upgrading-plugins-on-latest-jenkins/9846)

The plugin download from Jenkins mirrors fail sporadically. In my case I had problems with ftp.halifax.rwth-aachen.de. That is why I ended up including the plugin installation in the Dockerfile with jenkins-plugin-cli
