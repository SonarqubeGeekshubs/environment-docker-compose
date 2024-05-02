# Run SonarQube with a PostgreSQL database

## Compose file

Execute this [docker-compose.yml](docker-compose.yml) configuration file to start SonarQube and a PostgreSQL database.
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

## To be improved

 + Backup
 + Clustering
 + Upgrade
 + Admin password
 + Plugins
 + ...
