# SonarQube with Jenkins CI

## Compose file

Execute this [docker-compose.yml](docker-compose.yml) configuration file to start SonarQube, a PostgreSQL database and Jenkins CI.
```shell
docker compose up
```

Restart the containers (after plugin upgrade or install for example).

```shell
docker compose restart sonarqube
```

Analyse a project with Maven:
```shell
mvn sonar:sonar \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=<sonar-analysis-token>
```

Command to stop and remove compose environment:
```shell
docker compose down --rmi local -v --remove-orphans
```

### Jenkinsfile

Sample project with Jenkinsfile:
* https://github.com/SonarqubeGeekshubs/atomist-spring-boot

```groovy
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

## To be improved

 + Plugins
 + ...
