# Session-4

## Configuration

### Postgres

1. Pull and run a lightweight image of Postgres:
```
docker run -d \
    --name my-postgres \
    -p 8081:8080 \
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

```
