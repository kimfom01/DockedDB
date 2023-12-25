# DockedDB
## Create virtual machine

* login to root
* create user with shell and home directory 
```
useradd -m -s /bin/bash {username}
``` 
* add passsword to new user 
```
passwd {username}
```
* make the user a sudoer 
```
usermode -aG sudo {username}
```
* login to user
* setup docker in virtual machine (look for documentation on website)
* make sure docker is a sudoer

## DB backup and restore

* get database connection details for pg_dump {user} {host} {database_name} {password}
* check db encoding to make sure target db has the same encoding as the source db (dump file)
* create dump of database
```
pg_dump -h {host} -U {user} -d {database} -Fc --encoding=UTF8 -f /directory/to/backup/file/{file_name}.dump (go to postgres dir incase)
```
* update the docker compose file
* add volume(pg_dump file) to the DB section
```
db:
    image: postgres
    volumes:
      - ./backup_file.dump:/etc/backup/backup_file.dump
```
* build your docker compose image (https://github.com/kimfom01/DockedDB)
* setup nginx for proxy redirection or whatever they call it
* make sure you add dump file as a volume in postgres image script

* cd into directory with docker-compose.yml
```
docker compose up -d
```
* to access the shell in the postgres container

```
docker exec -it  {container name } /bin/bash 
```

* then from the bash terminal we can run 
```
pg_restore -U {username} -d {database name} -Fc {path to volume}
```
* to retore the database

* to connect you can use psql
```
psql -U {username} -d {databse name} -h {host|optional} -p {port |optional}
```


