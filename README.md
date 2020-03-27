## Optional local dev outside of docker
```bash
npm install
```

create db
check out db-service init.sql
---

## Duild with Docker
Create the docker image of the application
***-t*** adds tag to build
```bash
docker build -t node-prod .
```

Once built we can run the server
```bash
docker run -d -p 8080:8080 node-prod // run detached
docker run -it -p 8080:8080 node-prod // run interactive
```

Live reloading option for development
```bash
docker build -f Dockerfile.dev -t node-dev-tag .
docker run --name node-dev-name --rm -it -p 8080:8080 -v ${PWD}:/app node-dev-tag
```

## Note -

Make sure you have the port that is being exposed set to the same exposed port in server.js


## Running development with mysql running as well in Docker - local only

run docker compose to start the development nodejs server + start mysql up and init with items in /db-service

```bash
docker-compose build --no-cache # --no-cache if first time. or if experiencing errors

# run in detached mode
docker-compose up -d
```

Some useful commands for this build
```bash
# get running containers should be 2 
# - node_comp_dev
# - mysql_comp_dev
docker container ls

# get associated volumes.
docker volume ls

# see networks + volume info of container
docker container inspect node_comp_dev


# watch logs
docker contaienr node-comp_dev --follow
```

# Push to docker hub
Docker hub pro tip - to release several tags for the same build
```bash
docker tag local-image:tagname new-repo:tagname
docker push new-repo:tagname
```
1. First create repo in docker hub {{docker-js-stack-api}}
2. build
```bash
docker build -t mjwrazor/docker-js-stack-api:v0.1.0 .
```

3. push
```bash
docker push mjwrazor/docker-js-stack-api:v0.1.0  
```

# Setting up CircleCI
Make sure you have circleci account and you have connected it with github. 

Make a circleCi directory and the config file
```bash
mkdir .circleci
cd .circleci
touch config.yml
cd ..
```

Set up config.yml with the basic docker hub build context for latest and tags. Commit to git and then in circleCi select project and "Set Up Project"

Then go to settings and add environment variables DOCKERHUB_USERNAME & DOCKERHUB_PASS which should then map correctly into the config.yml for circleci.

Once this is done git push and circleci will build the image and push to docker hub.

To push a specific tag. Create a release with a tag. Or tag a commit.

# NOTES
For the server. if you have CORS set up with origin then credentials needs to be true and if the IP is localhost and the PORT is 80 then you don't need to specify the port. `http://localhost` 
