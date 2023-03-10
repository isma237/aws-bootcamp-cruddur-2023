# Week 1 — App Containerization

## Docker

Durant cette semaine nous avons travaillé sur la conteneurisation de nos applications. Afin de mieux gérer le cycle de vie des différents containers nous avons opté pour l'utilisation de Docker-compose.

Docker-compose

```
version: "3.8"
services:
  backend-flask:
    environment:
      FRONTEND_URL: "https://3000-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
      BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
    build: ./backend-flask
    ports:
      - "4567:4567"
    volumes:
      - ./backend-flask:/backend-flask
  frontend-react-js:
    environment:
      REACT_APP_BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
    build: ./frontend-react-js
    ports:
      - "3000:3000"
    volumes:
      - ./frontend-react-js:/frontend-react-js
  db:
    image: postgres:13-alpine
    restart: always
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
    ports:
      - '5432:5432'
    volumes: 
      - db:/var/lib/postgresql/data
  dynamodb-local:
    # https://stackoverflow.com/questions/67533058/persist-local-dynamodb-data-in-volumes-lack-permission-unable-to-open-databa
    # We needed to add user:root to get this working.
    user: root #This line 
    command: "-jar DynamoDBLocal.jar -sharedDb -dbPath ./data"
    image: "amazon/dynamodb-local:latest"
    container_name: dynamodb-local
    ports:
      - "8000:8000"
    volumes:
      - "./docker/dynamodb:/home/dynamodblocal/data"
    working_dir: /home/dynamodblocal
volumes:
  db:
    driver: local

# the name flag is a hack to change the default prepend folder
# name when outputting the image names
networks: 
  internal-network:
    driver: bridge
    name: cruddur
```

## La sécurité des containers
Quelques principes à utiliser afin de sécuriser les containers. Nous avons quelques outils qui nous permettent d'évaluer la vulnérabilité de ces derniers ainsi que d'améliorer leur gestion. 

- Snyk Open Source Github Walkthrough
- AWS Secret Manager: Permet de décentraliser la gestion des informations sensibles. L'idée est d'éviter d'écrire des mots de passes par exemple directeur dans nos containers. Secret Manager supporte également la rotation des mots de passe.
- AWS Inspector: Is an automated security assessment service that helps improve the security and compliance of applications deployed on AWS


## Documentation des API avec Open API

Permet de documenter les API qui seront ensuites consultable sur Swagger. 
Exercice: Documenter une simple application et l'importer sur AWS API Gateway pour comprendre le fonctionnement. 

## Installation de DynamoDB en local et de Postgres 

- Verification via la création d'une table et d'un document sur DynamoDB. 
- Connexion sur Postgre après installation du client sur Gitpod. **PS** Il faut intégrer les options *-h* et *-U* pour que ca fonctionnement correctement en précisant les credentials présents dans le docker-compose. 

## Modifications des API
Consulter le repos pour consulter les modifications. 

## Question
Comment intégrer SSM dans docker-compose ?


