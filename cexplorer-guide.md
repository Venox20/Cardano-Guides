### Login to psql as PostgreSQL Superuser "postgres"
```sudo -u postgres psql```

### List all data bases 
``` \l```

or 

```\l+``` for more detail  ```q``` to quit

You should see a database name with cexplorer with the Owner being your username.

```\q``` to quit and exit back to terminal.

### Login to cexplorer
```PGPASSFILE=config/pgpass-mainnet psql cexplorer```


### Run sample queries from IOHK Github
[IOHK Interesting Queries](https://github.com/input-output-hk/cardano-db-sync/blob/master/doc/interesting-queries.md)


### List all tables in database
```\dt+```

### Display details of a particular table
```\d+ <tablename>```
