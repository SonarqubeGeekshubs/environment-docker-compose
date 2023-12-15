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
  -Dsonar.token=sqa_4ca76618fb3ab502f0c427a5a1c82c247d8f9608
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
