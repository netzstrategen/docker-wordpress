# Docker wordpress

## Setup
Copy and rename the example docker-compose YAML file to the root of the project so is just docker-compose.yml.
Copy also the docker directory to the root of the repo.

To avoid accidental commits you can add it to the .gitignore or the .git/info/exclude file.

## Run

On the terminal cd to the root of your wordpress project and run
```
docker-compose up -d
```

-d to run in the background.

Be sure the port 80 on your computer is free to use. It maybe be taken by your apache webserver or another
docker project.

Once the containers are running you can log in inside the php container with the following command:
```
docker-compose exec --user=www-data web bash
```

Without the --user-data=www-data the login will be with the root user and that may create issues with
file permissions. 'web' means just the name of service as specified in the docker-compose.yml file.

To connect wordpress with the database containers use the following credentials:

```
const DB_NAME = 'db';
const DB_USER = 'db';
const DB_PASSWORD = 'db';
const DB_HOST = 'db';
const DB_CHARSET = 'utf8';
const DB_COLLATE = '';
```

Also, change `UPLOADS_PROXY_DOMAIN` to be the live or stage website domain.

Matching the ones we configured in the docker-compose.yml file.
