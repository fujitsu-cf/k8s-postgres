# PostgreSQL on kubernetes
PostgreSQL is an advanced object-relational database management system that supports an extended subset of the SQL standard, including transactions, foreign keys, subqueries, triggers, user-defined types and functions.

## Prerequisites

* Kubernetes 1.4+ with Beta APIs enabled
* PV provisioner support in the underlying infrastructure (Only when persisting data)

## Configuration
### Persistent Volume
Setup host path directory for Persistent Volume in file:
postgres-persistence.yml
```
hostPath:
  path: /home/azureuser/data/pg
```
### Postgres
In `postgres-pod.yaml` you can find environment variables for Postgres configuration:

```
env:
  - name: DB_PASS
    value: password
  - name: PGDATA
    value: /var/lib/postgresql/data/pgdata
  - name: PGDATABASE
```

* DB_PASS - Password for the new user.
* PGDATABASE - Name for new database to create.
* PGUSER - Username of new user to create. Default `postgres`

The whole list with environment variables is here: https://www.postgresql.org/docs/9.3/static/libpq-envars.html

## Deployment

First create PV:

```
kubectl create -f postgres-persistence.yml
```

Second create PVC:

```
kubectl create -f postgres-claim.yml
```

Finally deploy service and Pod

```
kubectl create -f postgres-service.yml
kubectl create -f postgres-pod.yml

```
