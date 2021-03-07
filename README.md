# Real World Project: Database Shard
## Prerequisites:
Have Docker, Docker Compose, Mariadb installed

## Docker install
```
sudo apt install docker.io
```
## Docker Compose install
```
sudo apt install docker-compose
```
## Mariadb install
```
sudo apt install mariadb-client
```
## Check that Docker is running
```
sudo systemctl status docker
```


### Clone my github in the terminal
```
git clone https://github.com/DeadlySlav/maxscale-docker
```
### Now we need to access the directroy 
```
cd maxscale-docker/maxscale/
```
### Now we need to bring up the containers within the directory
```
sudo docker-compose up -d
```
#### It should show 3 containers. (Master 1, Master 2, Maxscale) 
```
maxscale_master2_1 is up-to-date
maxscale_master_1 is up-to-date
maxscale_maxscale_1 is up-to-date
phpmyadmin is up-to-date
```
### Now enter this command in the terminal to see that they are up
```
sudo docker-compose exec maxscale maxctrl list servers
```
#### It will look like this after entering the command 
```
┌────────────────┬─────────┬──────┬─────────────┬─────────────────┬───────────┐
│ Server         │ Address │ Port │ Connections │ State           │ GTID      │
├────────────────┼─────────┼──────┼─────────────┼─────────────────┼───────────┤
│ zip_master_one │ master  │ 3306 │ 0           │ Master, Running │ 0-3000-32 │
├────────────────┼─────────┼──────┼─────────────┼─────────────────┼───────────┤
│ zip_master_two │ master2 │ 3306 │ 0           │ Running         │ 0-3000-31 │
└────────────────┴─────────┴──────┴─────────────┴─────────────────┴───────────┘
```





