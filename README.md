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
sudo apt install mariadb-client-core-10.3 
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
### Now enter this to connect using Mariadb
```
mariadb -umaxuser -pmaxpwd -h 127.0.0.1 -P 4000
```
#### Should like this 
```
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 8
Server version: 10.5.9-MariaDB-1:10.5.9+maria~focal-log mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> 
```
### Now enter this command 
```
show databases;
```
```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| zipcodes_one       |
| zipcodes_two       |
+--------------------+
5 rows in set (0.000 sec)
```
### Then enter this command 
```
+---------+-------------+----------------+-------+--------------+-----------+------------+-------------------------+---------------+-----------------+---------------------+------------+
| Zipcode | ZipCodeType | City           | State | LocationType | Coord_Lat | Coord_Long | Location                | Decommisioned | TaxReturnsFiled | EstimatedPopulation | TotalWages |
+---------+-------------+----------------+-------+--------------+-----------+------------+-------------------------+---------------+-----------------+---------------------+------------+
|     705 | STANDARD    | AIBONITO       | PR    | PRIMARY      | 18.14     | -66.26     | NA-US-PR-AIBONITO       | FALSE         |                 |                     |            |
|     610 | STANDARD    | ANASCO         | PR    | PRIMARY      | 18.28     | -67.14     | NA-US-PR-ANASCO         | FALSE         |                 |                     |            |
|     611 | PO BOX      | ANGELES        | PR    | PRIMARY      | 18.28     | -66.79     | NA-US-PR-ANGELES        | FALSE         |                 |                     |            |
|     612 | STANDARD    | ARECIBO        | PR    | PRIMARY      | 18.45     | -66.73     | NA-US-PR-ARECIBO        | FALSE         |                 |                     |            |
|     601 | STANDARD    | ADJUNTAS       | PR    | PRIMARY      | 18.16     | -66.72     | NA-US-PR-ADJUNTAS       | FALSE         |                 |                     |            |
|     631 | PO BOX      | CASTANER       | PR    | PRIMARY      | 18.19     | -66.82     | NA-US-PR-CASTANER       | FALSE         |                 |                     |            |
|     602 | STANDARD    | AGUADA         | PR    | PRIMARY      | 18.38     | -67.18     | NA-US-PR-AGUADA         | FALSE         |                 |                     |            |
|     603 | STANDARD    | AGUADILLA      | PR    | PRIMARY      | 18.43     | -67.15     | NA-US-PR-AGUADILLA      | FALSE         |                 |                     |            |
|     604 | PO BOX      | AGUADILLA      | PR    | PRIMARY      | 18.43     | -67.15     | NA-US-PR-AGUADILLA      | FALSE         |                 |                     |            |
|     605 | PO BOX      | AGUADILLA      | PR    | PRIMARY      | 18.43     | -67.15     | NA-US-PR-AGUADILLA      | FALSE         |                 |                     |            |
|     703 | STANDARD    | AGUAS BUENAS   | PR    | PRIMARY      | 18.25     | -66.1      | NA-US-PR-AGUAS BUENAS   | FALSE         |                 |                     |            |
|     704 | STANDARD    | AGUIRRE        | PR    | PRIMARY      | 17.96     | -66.22     | NA-US-PR-AGUIRRE        | FALSE         |                 |                     |            |
|    7675 | STANDARD    | WESTWOOD       | NJ    | PRIMARY      | 40.98     | -74.03     | NA-US-NJ-WESTWOOD       | FALSE         | 13245           | 24083               | 1089095041 |
|    7677 | STANDARD    | WOODCLIFF LAKE | NJ    | PRIMARY      | 41.02     | -74.05     | NA-US-NJ-WOODCLIFF LAKE | FALSE         | 2945            | 5471                | 325436960  |
|    7885 | STANDARD    | WHARTON        | NJ    | PRIMARY      | 40.89     | -74.58     | NA-US-NJ-WHARTON        | FALSE         | 5273            | 8999                | 240827990  |
+---------+-------------+----------------+-------+--------------+-----------+------------+-------------------------+---------------+-----------------+---------------------+------------+
15 rows in set (0.001 sec)

MariaDB [(none)]> 

```


