### Install NIX Package Manager

```curl -L https://nixos.org/nix/install > install-nix.sh```

```./install-nix.sh```

### Setup IOHK Binary Cache

This will save many hours of build time.

```sudo mkdir -p /etc/nix```


```
cat <<EOF | sudo tee /etc/nix/nix.conf
substituters = https://cache.nixos.org https://hydra.iohk.io
trusted-public-keys = iohk.cachix.org-1:DpRUyj7h7V830dp/i6Nti+NEO2/nhblbov/8MW7Rqoo= hydra.iohk.io:f/Ea+s+dFdN+3Y/G+FDgSq+a5NEWhJGzdjvKNGv0/EQ= cache.nixos.org-1:6NCHdD59X431o0gWypbMrAURkbJ16ZPMQFGspcDShjY=
EOF
```


### Install Cardano-node with NIX Package Manager


```git clone https://github.com/input-output-hk/cardano-node```

```cd cardano-node```

```nix-build -A scripts.mainnet.node -o mainnet-node-local```

### Install dbsync with NIX Package Manager

```git clone https://github.com/input-output-hk/cardano-db-sync```

```cd cardano-db-sync```

```nix-build -A cardano-db-sync -o db-sync-node```

### Install PostgreSQL

```sudo apt update```

```sudo apt install postgresql postgresql-contrib```

### Set up PostgreSQL user

```whoami```

```sudo -u postgres psql```

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

```PGPASSFILE=config/pgpass-mainnet scripts/postgresql-setup.sh --createdb```

### Start dbsync

```
PGPASSFILE=config/pgpass-mainnet db-sync-node/bin/cardano-db-sync \
    --config config/mainnet-config.yaml \
    --socket-path ../cardano-node/state-node-mainnet/node.socket \
    --state-dir ledger-state/mainnet \
    --schema-dir schema/
```

### Cardano-CLI Modification

DBsync does not need Cardano-cli but if you would like to have it you can compile or get it the way you normally do.

Change your Cardano Node Socket in .bashrc

```export CARDANO_NODE_SOCKET_PATH=/home/<user name>/cardano-node/state-node-mainnet/node.socket```
