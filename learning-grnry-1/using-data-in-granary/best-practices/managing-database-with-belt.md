---
description: >-
  This page is about how we can connect to the database using the Belt callback
  code locally with the Belt Extractor Python package.
---

# Managing Database with Belt

## Configuration steps to deal the database via belt

Include the following package :

```python
from grnry_belt.repository.dbfactory import Database
```

Create a object of Database class like below :

```python
database=Database()
```

Now we can add methods `_get_credentials` to get database credentials from mounted secret. Here in example the mounted secrets for username, password and host are mounted at below locations :

```text
/etc/secrets/grnry-bipro-pg/username
/etc/secrets/grnry-bipro-pg/password
/etc/secrets/grnry-bipro-pg/host
```

To mount the credentials, We need to add `Volumes` and `VolumeMounts` in the belt configuration with the above mention path.

```javascript
"volumes": "{\"volumes\": [{\"name\": \"pg-creds-volume\", \"secret\": {\"secretName\": \"grnry-bipro-pg-secret\"}}]}",
"volumeMounts": "{\"volumeMounts\": [{\"name\": \"pg-creds-volume\", \"subPath\": \"\", \"readOnly\": true, \"mountPath\": \"/etc/secrets/grnry-bipro-pg\"}]}"
```

Once the configuration is done, The connection reads the username, password and host from the mounted location and Further `_init_db_connection` to setup database connection kept as long as belt runs.

Example of `_get_credentials`

```python
def _get_credentials() -> (str, str, str) :
  
    with open('/etc/secrets/grnry-bipro-pg/username','r') as file:
        usr = file.read()
    with open('/etc/secrets/grnry-bipro-pg/password','r')  as file:
        pwd = file.read()
    with open('/etc/secrets/grnry-bipro-pg/host','r') as file:
        host = file.read()
    return usr, pwd, host
```

Example of `_init_db_connection`  where we call PG credentials secret has to be volume mounted in belt config.

```python
def _init_db_connection():

    if database.cursor is None:
        usr, pwd, host = _get_credentials()
        database.db_name = 'gevoservice'
        database.db_user = usr
        database.db_pwd = pwd
        database.db_host = host

        database.create_connection_pool()
        database.connect()
```

Calling a `_init_db_connection`  to connect to the Database when trigger the belt and start execute function.

```python
def execute(event_headers, event_payload, profile=None): 
    ## Setup DB connection once
    _init_db_connection()
```

Once the database connection is done we can perform any query on the connected database tables. For example:

```python
INSERT_DOCUMENT_SHIPMENT = "INSERT INTO grnry_bipro.document_shipment (transfer_id, dateiquelle, dateiadresse, complete_transfer) VALUES %s RETURNING id;"
SELECT_TRANSFER_QUERY = "SELECT backend_id FROM bipro.bipro_transfer where id = %s"
```

