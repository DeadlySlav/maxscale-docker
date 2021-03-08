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
SELECT * FROM zipcodes_one.zipcodes_one LIMIT 10;
```
```
+---------+-------------+-----------+-------+--------------+-----------+------------+--------------------+---------------+-----------------+---------------------+------------+
| Zipcode | ZipCodeType | City      | State | LocationType | Coord_Lat | Coord_Long | Location           | Decommisioned | TaxReturnsFiled | EstimatedPopulation | TotalWages |
+---------+-------------+-----------+-------+--------------+-----------+------------+--------------------+---------------+-----------------+---------------------+------------+
|     705 | STANDARD    | AIBONITO  | PR    | PRIMARY      | 18.14     | -66.26     | NA-US-PR-AIBONITO  | FALSE         |                 |                     |            |
|     610 | STANDARD    | ANASCO    | PR    | PRIMARY      | 18.28     | -67.14     | NA-US-PR-ANASCO    | FALSE         |                 |                     |            |
|     611 | PO BOX      | ANGELES   | PR    | PRIMARY      | 18.28     | -66.79     | NA-US-PR-ANGELES   | FALSE         |                 |                     |            |
|     612 | STANDARD    | ARECIBO   | PR    | PRIMARY      | 18.45     | -66.73     | NA-US-PR-ARECIBO   | FALSE         |                 |                     |            |
|     601 | STANDARD    | ADJUNTAS  | PR    | PRIMARY      | 18.16     | -66.72     | NA-US-PR-ADJUNTAS  | FALSE         |                 |                     |            |
|     631 | PO BOX      | CASTANER  | PR    | PRIMARY      | 18.19     | -66.82     | NA-US-PR-CASTANER  | FALSE         |                 |                     |            |
|     602 | STANDARD    | AGUADA    | PR    | PRIMARY      | 18.38     | -67.18     | NA-US-PR-AGUADA    | FALSE         |                 |                     |            |
|     603 | STANDARD    | AGUADILLA | PR    | PRIMARY      | 18.43     | -67.15     | NA-US-PR-AGUADILLA | FALSE         |                 |                     |            |
|     604 | PO BOX      | AGUADILLA | PR    | PRIMARY      | 18.43     | -67.15     | NA-US-PR-AGUADILLA | FALSE         |                 |                     |            |
|     605 | PO BOX      | AGUADILLA | PR    | PRIMARY      | 18.43     | -67.15     | NA-US-PR-AGUADILLA | FALSE         |                 |                     |            |
+---------+-------------+-----------+-------+--------------+-----------+------------+--------------------+---------------+-----------------+---------------------+------------+
10 rows in set (0.000 sec)
```

### Now check from zipcodes_two 
```
SELECT * FROM zipcodes_two.zipcodes_two LIMIT 10;
```
```
+---------+-------------+-------------+-------+--------------+-----------+------------+----------------------+---------------+-----------------+---------------------+------------+
| Zipcode | ZipCodeType | City        | State | LocationType | Coord_Lat | Coord_Long | Location             | Decommisioned | TaxReturnsFiled | EstimatedPopulation | TotalWages |
+---------+-------------+-------------+-------+--------------+-----------+------------+----------------------+---------------+-----------------+---------------------+------------+
|   42040 | STANDARD    | FARMINGTON  | KY    | PRIMARY      | 36.67     | -88.53     | NA-US-KY-FARMINGTON  | FALSE         | 465             | 896                 | 11562973   |
|   41524 | STANDARD    | FEDSCREEK   | KY    | PRIMARY      | 37.4      | -82.24     | NA-US-KY-FEDSCREEK   | FALSE         |                 |                     |            |
|   42533 | STANDARD    | FERGUSON    | KY    | PRIMARY      | 37.06     | -84.59     | NA-US-KY-FERGUSON    | FALSE         | 429             | 761                 | 9555412    |
|   40022 | STANDARD    | FINCHVILLE  | KY    | PRIMARY      | 38.15     | -85.31     | NA-US-KY-FINCHVILLE  | FALSE         | 437             | 839                 | 19909942   |
|   40023 | STANDARD    | FISHERVILLE | KY    | PRIMARY      | 38.16     | -85.42     | NA-US-KY-FISHERVILLE | FALSE         | 1884            | 3733                | 113020684  |
|   41743 | PO BOX      | FISTY       | KY    | PRIMARY      | 37.33     | -83.1      | NA-US-KY-FISTY       | FALSE         |                 |                     |            |
|   41219 | STANDARD    | FLATGAP     | KY    | PRIMARY      | 37.93     | -82.88     | NA-US-KY-FLATGAP     | FALSE         | 708             | 1397                | 20395667   |
|   40935 | STANDARD    | FLAT LICK   | KY    | PRIMARY      | 36.82     | -83.76     | NA-US-KY-FLAT LICK   | FALSE         | 752             | 1477                | 14267237   |
|   40997 | STANDARD    | WALKER      | KY    | PRIMARY      | 36.88     | -83.71     | NA-US-KY-WALKER      | FALSE         |                 |                     |            |
|   41139 | STANDARD    | FLATWOODS   | KY    | PRIMARY      | 38.51     | -82.72     | NA-US-KY-FLATWOODS   | FALSE         | 3692            | 6748                | 121902277  |
+---------+-------------+-------------+-------+--------------+-----------+------------+----------------------+---------------+-----------------+---------------------+------------+
10 rows in set (0.001 sec)
```

### Now use this command to see the largest zipcode in zipcodes_one
```
SELECT Zipcode FROM zipcodes_one.zipcodes_one ORDER BY Zipcode DESC LIMIT 1;
```
```
+---------+
| Zipcode |
+---------+
|   47750 |
+---------+
1 row in set (0.008 sec)
```
### Now use this command to see the smalled zipcode in zipcodes_two
```
SELECT Zipcode FROM zipcodes_two.zipcodes_two ORDER BY Zipcode ASC LIMIT 1;
```
```
+---------+
| Zipcode |
+---------+
|   38257 |
+---------+
1 row in set (0.005 sec)
```
