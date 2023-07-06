# git indexer applcaiton

This application consists of 2 modules

1. the web UI application
2. an indexer cronjob

## manually create database

```test

# shell into the running postgres container

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
