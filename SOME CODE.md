### Option A — Using SQL*Plus (recommended)

Open Command Prompt and type:

```bash
sqlplus / as sysdba
```


### Disable it

```sql
EXEC DBMS_XDB.SETHTTPPORT(0);
```


# Step 4: Re-enable later

Whenever you want Oracle to use port 8080 again:

```sql
EXEC DBMS_XDB.SETHTTPPORT(8080);
```