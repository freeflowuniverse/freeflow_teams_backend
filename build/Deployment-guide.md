# Freeflow deployment guid
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
## cloning the repos 
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
git clone https://github.com/ashraffouda/mattermost-docker-mysql.git
```
## building frontend and backend packages
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
## docker build prerequesits
```
cd ../mattermost-docker-mysql
cp ../freeflow_teams_backend/dist/mattermost-team-linux-amd64.tar.gz app/
mkdir -p ./volumes/app/mattermost/{data,logs,config,plugins}
chown -R 2000:2000 ./volumes/app/mattermost/
```
## building mattermost image
```
docker-compose build 
```
## deploying freeflow teams (frontend, backend, mysqldb, nginx)
```
docker-compose up 
```