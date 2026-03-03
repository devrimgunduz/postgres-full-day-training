Fixing docker issues:

If you break your config and the `postgres-training` container will not start (for instance, setting `shared_preload_libraries = pgaudit` without the extension installed), you can do the following:

```
$ docker run --rm -it -v postgres-full-day-training_postgres_data:/repair alpine sh

$ vi /repair/18/docker/postgresql.auto.conf

# fix/save

$ docker compose --profile dba up -d
```


