# Fichiers docker compose pour exécuter Sonarqube, Jenkins et exposer en Internet avec Ngrok

## Other languages / Otros idiomas / En autres langues
[![en](https://img.shields.io/badge/in-english-yellow.svg)](https://github.com/BrainsDevOps/sonarqube-udemy-docker-compose/blob/main/readme.md)
[![es](https://img.shields.io/badge/en-español-yellow.svg)](https://github.com/BrainsDevOps/sonarqube-udemy-docker-compose/blob/main/readme-es.md)
[![fr](https://img.shields.io/badge/en-français-red.svg)](https://github.com/BrainsDevOps/sonarqube-udemy-docker-compose/blob/main/readme-fr.md)

Ce repository contient des fichiers docker-compose pour exécuter facilement les exemples de code dans les cours udemy.

## Contexto
Ce repository fait partie de mes cours udemy :
* [Domina Sonarqube (Espagnol)](https://www.udemy.com/course/domina-sonarqube/?referralCode=EF59257E7D8DC3026D6D)
* [Domina Github Actions (Espagnol)](https://www.udemy.com/course/domina-github-actions/?referralCode=CBFBAF72C38BE758CFE1)

Si vous souhaitez en savoir plus sur Sonarqube, vous pouvez vérifier s'il y a des promotions en cours pour des cours dans la section des cours de[devopsbrains.com](https://devopsbrains.com/cursos/)

## Pré-requis
* Docker
* Docker compose
* Compte de ngrok (Facultatif. Nécessaire si vous voulez exposer des services en Internet avec ngrok)

# Configuration de ngrok
* Si vous voulez exposer n'importe quel service sur l'internet avec ngrok, vous devez:
    * Créer une compte de [ngrok](https://ngrok.com/) et générer un jeton d'authentification et un domaine.
    * Si vous voulez en savoir plus sur le ngrok, j'ai une vidéo [Youtube (Espagnol)](https://youtu.be/UW8BObHdi08) avec un guide rapide sur le sujet
    * Créer un fichier .env à la racine du repository pour indiquer votre jeton ngrok et votre domaine.
```
NGROK_AUTHTOKEN=<jeton d'authentification>
NGROK_DOMAIN=<domaine>
```

# Modes de lancement
* Sonarqube: `docker-compose up`
* Sonarqube + ngrok: `docker-compose --profile public up`
* Jenkins: `docker compose -f compose.jenkins.yaml up`
* Jenkins + ngrok: `docker compose -f compose.jenkins.yaml --profile public up`

NOTA: Avec un compte ngrok gratuit, vous n'avez qu'un seul domaine et vous ne pouvez lancer qu'une seule session ngrok à la fois. Vous ne pouvez donc exposer que Sonar ou Jenkins à la fois.

# Première connexion à Jenkins
* Après avoir lancé Jenkins, vous pouvez aller sur http://localhost:8080/ pour l'ouvrir dans le navigateur.
* Nous pouvons sauter l'installation des plugins, car les plugins nécessaires aux exercices du cours sont déjà installés. "Select plugins to install" > seleccionar "None" > "Install"
* Créer un utilisateur administrateur
* Sélectionner l'URL par défaut du serveur : "http://localhost:8080/".
* La configuration de credentials, nodeJs, SonarScanner, etc. est expliquée dans les leçons du cours.

# Problèmes connus
* [Problème de checkout de github](https://github.com/jenkinsci/helm-charts/issues/728)

Un étudiant a rencontré ce problème et l'a résolu en ajoutant cette ligne dans le fichier Docker après l'instruction FROM

```
RUN git config --global --add safe.directory "*"
```

* [échec du téléchargement des plugins](https://community.jenkins.io/t/issue-while-upgrading-plugins-on-latest-jenkins/9846)

Apparemment, les téléchargements des miroirs Jenkins peuvent parfois échouer. Dans mon cas, j'ai eu des problèmes avec ftp.halifax.rwth-aachen.de et c'est pourquoi j'ai choisi d'inclure l'installation de plugins dans le fichier Docker avec jenkins-plugin-cli.