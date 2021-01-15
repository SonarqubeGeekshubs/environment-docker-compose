# Sonarqube Compose recipes

## Requirements

 * Docker CE 1.18

## Steps

### Docker

```
docker run -d --name sonarqube_geekshubs -p 9000:9000 -p 9092:9092 sonarqube
```

http://localhost:9000

docker logs -f sonarqube_geekshubs

f3e62fe8ec1814aba0f0c2604e156ab6f1a06a61


sonar-scanner \
  -Dsonar.projectKey=sonar-test \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=f3e62fe8ec1814aba0f0c2604e156ab6f1a06a61

### Compose

https://github.com/SonarqubeGeekshubs/environment-docker-compose/blob/master/sonarqube-postgres/docker-compose.yml

docker-compose up -d

remove all

```
docker-compose down --rmi local -v --remove-orphans
```

## Analysis

### Maven

3.6.3-jdk-11, 3.6.3, 3.6-jdk-11, 3.6, 3-jdk-11, 3, latest

Docker:
```
docker run -it --rm -v ~/.m2.docker:/root/.m2 \
-v $PWD:/usr/src/ -w /usr/src/ \
maven:3.6.3-jdk-11 \
mvn clean verify sonar:sonar -Dsonar.login=c573711c07806a4fa17bae1fde4349ce471606db \
-Dsonar.host.url=http://10.1.0.141:9000
```

Maven:
```
mvn sonar:sonar \
  -Dsonar.projectKey=maven-key \
  -Dsonar.host.url=http://ip172-18-0-26-bov7nr833cq000ced6ng-9000.direct.labs.play-with-docker.com \
  -Dsonar.login=ed5d9216572b96828a4f75e7f3f16a3c19baf91e
```

Docker local:
```
docker run -it --rm -v ~/.m2.docker:/root/.m2 -v $PWD:/usr/src/ -w /usr/src/ maven:3.6.3-jdk-11 mvn clean verify sonar:sonar -Dsonar.login=c573711c07806a4fa17bae1fde4349ce471606db -Dsonar.host.url=http://10.1.0.141:9000
```


### Jenkins

https://github.com/SonarqubeGeekshubs/environment-docker-compose

```
$ git clone git@github.com:SonarqubeGeekshubs/environment-docker-compose.git
$ cd environment-docker-compose/sonarqube-jenkins
$ docker-compose up -d
```

Github token
bd62bb3364962b752d8d0fd22c63fed3cdf0e301

Jenkinstoken: 212d1b0930f3f99fe1fa10393b7043ff204557ac

### Jenkinsfile

https://github.com/SonarqubeGeekshubs/atomist-spring-boot

file:

```
node {
    // Mark the code checkout 'stage'....
    stage 'Checkout'
    checkout scm

    stage 'Configure'
    env.PATH = "${tool 'Maven 3'}/bin:${env.PATH}"

    // Mark the code build 'stage'....
    stage('Build') {
      // Run the maven build
      sh “mvn clean verify -Dmaven.test.failure.ignore=true”
    }

    stage('SonarQube analysis') {
      withSonarQubeEnv('Sonarqube') {
        sh 'mvn sonar:sonar'
      }
    }
}
```

### Quality Game

Checkout Repository: https://github.com/mechero/code-quality-game

Update .env file:
````
BACKEND_VERSION=1.2.0
UI_VERSION=1.0.4
SONAR_SERVER=http://host.docker.internal:9000
SONAR_TOKEN=4e992e6874ef0f26f0bbc1b247d1dedda2819849
LEGACY_DATE=2020-12-31
EARLYBIRD_DATE=2021-01-14
CAMPAIGNSTART_DATE=2021-01-14
````

Docker:
```
$ docker-compose -f code-quality-game-docker-compose.yml up
```
