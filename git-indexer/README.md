# git indexer applcaiton

This application consists of 2 modules

1. the web UI application
2. an indexer cronjob

It also requires a PostgreSQL database to operate.

## manually create database

```test

# shell into the running postgres container

su - postgres

# run the following as postgres user
PGPASSWORD=postgres psql postgres

CREATE USER git WITH PASSWORD 'git';
CREATE DATABASE gitindexer;
GRANT ALL PRIVILEGES ON DATABASE gitindexer TO git;

# changind username, password or database name would require changing K8S Deployment and Cronjob configuration to update environment variable SQLALCHEMY_DATABASE_URI accordingly.

```

## create secret for ssh credentials

the indexer job currently requires ssh setup in order to access the remote Git repositories.

```shell

cd ~/.ssh

kubectl create secret generic git-secret \
     --from-file=id_ed25519=id_ed25519 \
     --from-file=id_ed25519.pub=id_ed25519.pub \
     --from-file=known_hosts=known_hosts \
     -n <your namespace>
```
