# Ficheros docker-compose para lanzar SonarQube y Jenkins

Este repositorio contiene ficheros docker-compose para lanzar fácilmente los ejemplos de código que se tratan en el curso

## Contexto
El repositorio forma parte del contenido de mi cursos de udemy:
* [Domina Sonarqube](https://www.udemy.com/course/domina-sonarqube/?referralCode=EF59257E7D8DC3026D6D)

Si te interesa aprender más sobre Sonarqube, Puedes comprobar si hay promociones vigentes para los cursos en la sección de cursos de [devopsbrains.com](https://devopsbrains.com/cursos/)

## Prerrequisitos
* Docker
* Docker compose
* Cuenta de ngrok (opcional. Necesario si se quiere exponer los servicios con ngrok)

# Configuración de ngrok
* Si quieres exponer alguno de los servicios en Internet con ngrok necesitas:
    * Tener una cuenta de [ngrok](https://ngrok.com/) creada y generar un token de autenticación y un dominio.
    * Si quieres saber más sobre ngrok, tengo un vídeo de [Youtube](https://youtu.be/UW8BObHdi08) con una guía rápida del tema
    * Crea un fichero .env a la raíz del repositorio para indicar tu token y dominio de ngrok
```
NGROK_AUTHTOKEN=<tu token>
NGROK_DOMAIN=<tu dominio>
```

# Modos de lanzamiento
* Sonarqube: `docker-compose up`
* Sonarqube + ngrok: `docker-compose --profile public up`
* Jenkins: `docker compose -f compose.jenkins.yaml up`
* Jenkins + ngrok: `docker compose -f compose.jenkins.yaml --profile public up`

NOTA: Con una cuenta gratuita de ngrok solo tienes un dominio y solo puedes lanzar una sesión de ngrok a la vez así que solo podrás exponer o Sonar o Jenkins a la vez.

# Primera conexión a Jenkins
Tras lanzar Jenkins, se puede ir abrir en el navegador http://localhost:8080/

* Podemos saltar la instalación de plugins, porque los plugins necesarios para los ejercicios del curso ya están instalados. "Select plugins to install" > seleccionar "None" > "Install"
* Creamos un usuario administrador
* Seleccionamos la URL por defecto para el servidor: "http://localhost:8080/"
* La configuración de credenciales, nodeJs, SonarScanner, etc. se explica en las lecciones del curso

# Problemas conocidos
* [Fallo checkout de github](https://github.com/jenkinsci/helm-charts/issues/728)

Un alumno tuvo este problema y lo solventó añadiendo esta línea en tu Dockerfile tras la instrucción FROM

```
RUN git config --global --add safe.directory "*"
```

* [fallo en la descarga de los plugins](https://community.jenkins.io/t/issue-while-upgrading-plugins-on-latest-jenkins/9846)

Al perecer las descargas de los mirrors de Jenkins pueden fallar por momentos. En mi caso tuve problemas con ftp.halifax.rwth-aachen.de y por eso opté por incluir la instalación de los plugins en el Dockerfile con jenkins-plugin-cli
