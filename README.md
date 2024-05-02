# Sonarqube Compose recipes

## Requirements

Sonarqube: check official docs for [last pre-requisites](https://docs.sonarsource.com/sonarqube/9.9/requirements/prerequisites-and-overview)

Local configuration with docker and docker compose:
* Docker latest version checked: 23.0.5
* Docker compose latest version checked: v2.17.3

Also check **Docker Host Requirements** section on Dockerhub [Sonarqube](https://hub.docker.com/_/sonarqube)

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
  -Dsonar.login=<sonar-analysis-token>
```

### Sonar Scanner

Using `sonar-scanner`:
```shell
sonar-scanner \
  -Dsonar.projectKey=sonar-test \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=<sonar-analysis-token>
```

### Docker with maven

Docker for maven project:
```shell
docker run -it --rm -v ~/.m2.docker:/root/.m2 \
-v $PWD:/usr/src/ -w /usr/src/ \
maven:3.6.3-jdk-11 \
mvn clean verify sonar:sonar -Dsonar.login=<sonar-analysis-token> \
-Dsonar.host.url=http://10.1.0.141:9000
```

### Docker scanner cli

```shell
docker run \
--rm \
-e SONAR_HOST_URL="http://192.168.1.41:9000" \
-e SONAR_SCANNER_OPTS="-Dsonar.projectKey=org.sonarqube:sonarqube-scanner" \
-e SONAR_LOGIN="sqa_443b5ac66d698ea07571cc3bb2b931a4bcaa7300" \
-v $PWD:/usr/src \
sonarsource/sonar-scanner-cli
```

## Docker compose recipes

### With Postgres Database

Run following [sonarqube-postgres](sonarqube-postgres/README.md) configuration file to start SonarQube and a PostgreSQL database.

Run in [sonarqube-postgres](sonarqube-postgres) dir:
```shell
$ git clone git@github.com:SonarqubeGeekshubs/environment-docker-compose.git
$ cd environment-docker-compose/sonarqube-postgres
$ docker compose up -d
```

### Jenkins

Run following [sonarqube-jenkins](sonarqube-jenkins/README.md) configuration file to start SonarQube, a PostgreSQL database and Jenkins CI.

Run in [sonarqube-jenkins](sonarqube-jenkins) dir:
```shell
$ git clone git@github.com:SonarqubeGeekshubs/environment-docker-compose.git
$ cd environment-docker-compose/sonarqube-jenkins
$ docker compose up -d
```
## Quality Game

Checkout Repository: https://github.com/mechero/code-quality-game

Update .env file:
```shell
BACKEND_VERSION=1.2.0
UI_VERSION=1.0.4
SONAR_SERVER=http://host.docker.internal:9000
SONAR_LOGIN=a5e40ca3915db3f046d31624c6dd5e40711fe1c5
LEGACY_DATE=2023-05-19
EARLYBIRD_DATE=2023-05-25
CAMPAIGNSTART_DATE=2023-05-19
```

Docker:
```shell
docker compose -f docker-compose-quboo.yml up
```
