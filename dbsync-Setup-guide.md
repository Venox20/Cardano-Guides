### Secure SSH Access (taken from newb guide)
[cardano-node-guide/securing-ssh-access.md at master · Chris-Graffagnino/cardano-node-guide · GitHub](https://github.com/Chris-Graffagnino/cardano-node-guide/blob/master/docs/securing-ssh-access.md)

### Make non-root user with sudo access (nix install can't be installed as root)
`useradd -c “<SOME COMMENT HERE>” -m -d /home/<YOUR_ADMIN_USER> -s /bin/bash -G sudo,ssh-users <YOUR_ADMIN_USER>`

### Switch to non-root user
`su - <NON-ROOT USER>`


### Install NIX Package Manager

`curl -L https://nixos.org/nix/install | sh`


### Setup IOHK Binary Cache

This will save many hours of build time.

```sudo mkdir -p /etc/nix```


```
cat <<EOF | sudo tee /etc/nix/nix.conf
substituters = https://cache.nixos.org https://hydra.iohk.io
trusted-public-keys = iohk.cachix.org-1:DpRUyj7h7V830dp/i6Nti+NEO2/nhblbov/8MW7Rqoo= hydra.iohk.io:f/Ea+s+dFdN+3Y/G+FDgSq+a5NEWhJGzdjvKNGv0/EQ= cache.nixos.org-1:6NCHdD59X431o0gWypbMrAURkbJ16ZPMQFGspcDShjY=
EOF
```

### Install git and postgresql
`sudo apt install git postgresql postgresql-contrib -y`

### Install Cardano-node with NIX Package Manager
`cd` to home directory

```git clone https://github.com/input-output-hk/cardano-node```

```cd cardano-node```

```nix-build -A scripts.mainnet.node -o mainnet-node-local```


### Install dbsync with NIX Package Manager
`cd` to home directory

```git clone https://github.com/input-output-hk/cardano-db-sync```

```cd cardano-db-sync```

```nix-build -A cardano-db-sync -o db-sync-node```


### Set up PostgreSQL user

```whoami```

```sudo -u postgres psql```

You will now be in the psql shell.  

Take note of the two types of commands
1. psql - such as \c, \l, \dt.
2. SQL - such as CREATE DATABASE, CREATE TABLE and SELECT

Make sure to end all SQL commands with a semi-colon.

```CREATE USER <enter output of whoami>;```

```ALTER USER <enter output of whoami> WITH SUPERUSER;```


```ALTER USER <enter output of whoami> WITH CREATEDB;```

```\du```

You should see a table with the user you just created and the list of attributes that include Superuser, Create DB.


To quit

```\q```

Change file permission on 

```cd cardano-db-sync/config```

```chmod 600 pgpass-mainnet```


### Start Cardano-node

```cd cardano-node```

```./mainnet-node-local```


### Setup dbsync
`cd ~/cardano-db-sync`

```PGPASSFILE=config/pgpass-mainnet scripts/postgresql-setup.sh --createdb```

### Start dbsync

```
PGPASSFILE=config/pgpass-mainnet db-sync-node/bin/cardano-db-sync \
    --config config/mainnet-config.yaml \
    --socket-path ../cardano-node/state-node-mainnet/node.socket \
    --state-dir ledger-state/mainnet \
    --schema-dir schema/
```

### You Can Rebuild the DB if needed by

```
PGPASSFILE=config/pgpass-mainnet scripts/postgresql-setup.sh --recreatedb
```

```
PGPASSFILE=config/pgpass-mainnet db-sync-node/bin/cardano-db-sync \
    --config config/mainnet-config.yaml \
    --socket-path ../cardano-node/state-node-mainnet/node.socket \
    --state-dir ledger-state/mainnet \
    --schema-dir schema/
  ```

### Cardano-cli Modification

DBsync does not need Cardano-cli but if you would like to have it you can compile or get it the way you normally do.

Change your Cardano Node Socket in .bashrc

```export CARDANO_NODE_SOCKET_PATH=/home/<user name>/cardano-node/state-node-mainnet/node.socket```
