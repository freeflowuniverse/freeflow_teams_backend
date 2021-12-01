# Freeflow deployment guide
## Prerequisites
- install golang
```
https://go.dev/doc/install
```
- add this to bashrc
```
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
ulimit -n 8096
```

- Install required packages 
```
apt-get install make npm zip unzip docker.io docker-compose -y 
```
## Cloning the repos 
- clone frontend repo 
```
git clone https://github.com/freeflowuniverse/freeflow_teams_frontend.git
```
- clone the backend
```
git clone https://github.com/freeflowuniverse/freeflow_teams_backend.git
```
- clone the mattermost-docker repo
```
git clone https://github.com/freeflowuniverse/mattermost-docker-mysql.git
```
## Building frontend and backend packages
```
mkdir -p freeflow_teams_frontend/dist
cd freeflow_teams_backend
ln -nfs ../freeflow_teams_frontend/dist client
cd ../freeflow_teams_frontend
make build
cd ../freeflow_teams_backend
make build 
make package
```
## Docker build prerequesits
```
cd ../mattermost-docker-mysql
cp ../freeflow_teams_backend/dist/mattermost-team-linux-amd64.tar.gz app/
mkdir -p ./volumes/app/mattermost/{data,logs,config,plugins}
chown -R 2000:2000 ./volumes/app/mattermost/
```
## Building mattermost image
```
docker-compose build 
```
## Deploying freeflow teams (frontend, backend, mysqldb, nginx)
```
docker-compose up 
```
## Backing up the deployment
Simply you need to backup volumes in `mattermost-docker-mysql/volumes`