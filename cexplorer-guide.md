### Login to psql as PostgreSQL Superuser "postgres"
```sudo -u postgres psql```

### List all data bases 
``` \l```

or 

```\l+``` for more detail  ```q``` to quit

You should see a database name with cexplorer with the Owner being your username.  If not see [dbsync-Setup-guide](https://github.com/Venox20/Cardano-Guides/blob/main/dbsync-Setup-guide.md)

```\q``` to quit and exit back to terminal.

### Login to cexplorer
```PGPASSFILE=config/pgpass-mainnet psql cexplorer```


### Run sample queries from IOHK Github
[IOHK Interesting Queries](https://github.com/input-output-hk/cardano-db-sync/blob/master/doc/interesting-queries.md)


### List all tables in database
```\dt+```

### Display details of a particular table
```\d+ <tablename>```

### Getting Started with PostgreSQL
[Getting Started PSQL](https://www3.ntu.edu.sg/home/ehchua/programming/sql/PostgreSQL_GetStarted.html)

### Video to understand inner join
[Inner Join Video](https://www.youtube.com/watch?v=a-xlrdG-fpY&t)
