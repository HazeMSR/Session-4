# Session-4

## Configuration

### Postgres

1. Pull and run a lightweight image of Postgres:
```
docker run -d \
    --name my-postgres \
    -p 8081:5432 \
    -e POSTGRES_PASSWORD=mysecretpassword \
    -e POSTGRES_USER=myself \
    -v /home/ec2-user/Session-4/db:/home/data \
    postgres:14-alpine
```
2. Create the role and the db:

```
# Login
docker exec -it my-postgres psql -U myself -W

# Create role
CREATE ROLE postgres LOGIN SUPERUSER PASSWORD 'mysecretpassword';

# Create db
CREATE DATABASE dvdrental;

exit;

# Import the database
docker exec -it my-postgres pg_restore -U myself -d dvdrental /home/data/dvdrental.tar -W

exit
```

### Spark

1. Copy the postgres driver to the spark jars library:

```
# Create your jupyter container
docker run -d \
    --name=my-spark \
    -v ~/Session-4/db:/home/jovyan/db \
    -p 8080:8888 \
    --shm-size=5g \
    jupyter/pyspark-notebook
    
# Login in your spark container as a root user

docker exec -u 0 -it my-spark /bin/bash

# Go to the following route:
cd /usr/local/spark/jars

# Copy you postgres driver to the current directory:
cp $HOME/work/postgresql-driver.jar ./

exit
```
