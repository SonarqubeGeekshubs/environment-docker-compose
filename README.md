# Sonarqube Compose recipes

## Requirements

 * Docker 20.10.21

## Steps

### Docker

Run with docker command:
```shell
docker run -d --name sonarqube_geekshubs -p 9000:9000 -p 9092:9092 sonarqube
```

Access to url `http://localhost:9000`

## Analysis

Extract a user token with _analysis rol_ enabled from sonrqube server.
### Maven

Maven:
```shell
mvn sonar:sonar \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=sqa_4ca76618fb3ab502f0c427a5a1c82c247d8f9608
```

### Sonar Scanner

Using `sonar-scanner`:
```shell
sonar-scanner \
  -Dsonar.projectKey=sonar-test \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=sqa_4ca76618fb3ab502f0c427a5a1c82c247d8f9608
```

### Docker with maven

Docker for maven project:
```shell
docker run -it --rm -v ~/.m2.docker:/root/.m2 \
-v $PWD:/usr/src/ -w /usr/src/ \
maven:3.6.3-jdk-11 \
mvn clean verify sonar:sonar -Dsonar.login=sqa_4ca76618fb3ab502f0c427a5a1c82c247d8f9608 \
-Dsonar.host.url=http://10.1.0.141:9000
```

## Docker compose recipes

### With Postgres Database

Run following [sonarqube-postgres](sonarqube-postgres/README.md) configuration file to start SonarQube and a PostgreSQL database.

Run in [sonarqube-postgres](sonarqube-postgres) dir:
```shell
$ git clone git@github.com:SonarqubeGeekshubs/environment-docker-compose.git
$ cd environment-docker-compose/sonarqube-postgres
$ docker-compose up -d
```

### Jenkins

Run following [sonarqube-jenkins](sonarqube-jenkins/README.md) configuration file to start SonarQube, a PostgreSQL database and Jenkins CI.

Run in [sonarqube-jenkins](sonarqube-jenkins) dir:
```shell
$ git clone git@github.com:SonarqubeGeekshubs/environment-docker-compose.git
$ cd environment-docker-compose/sonarqube-jenkins
$ docker-compose up -d
```
## Quality Game

Checkout Repository: https://github.com/mechero/code-quality-game

Update .env file:
```shell
BACKEND_VERSION=1.2.0
UI_VERSION=1.0.4
SONAR_SERVER=http://host.docker.internal:9000
SONAR_TOKEN=4e992e6874ef0f26f0bbc1b247d1dedda2819849
LEGACY_DATE=2020-12-31
EARLYBIRD_DATE=2021-01-14
CAMPAIGNSTART_DATE=2021-01-14
```

Docker:
```shell
docker-compose -f code-quality-game-docker-compose.yml up
```
