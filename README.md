# elpmaxe-fluffy-garbanzo

Example:

* Build and run Sonatype Nexus OSS 3 with Docker.

## Build

```
docker build ./nexus3 \
    --tag nexus3:elpmaxe \
    --force-rm \
    --no-cache \
    --pull
```

## Persistent Data Storage

```
mkdir -p /opt/containers/nexus3/nexus-data
chown -R 200 /opt/containers/nexus3/nexus-data
chmod 777 /opt/containers/nexus3/nexus-data
```

## Run

```
docker run \
    --publish 8081:8081 \
    --rm \
    --volume /opt/containers/nexus3/nexus-data:/nexus-data \
    --env MAX_HEAP=768m \
    nexus3:elpmaxe
```

The `NEXUS_CONTEXT` [environment variable][1] can be used to control the Nexus Context Path
, passed as `-Dnexus-context-path`. Defaults to `/`.

This [environment variable][2] is used to define the URL which Nexus is accessed.

## Permissions

- Use `docker exec` to run a command in a running container.

```
docker exec \
    --interactive \
    --tty \
    $(docker ps \
      --quiet \
      --filter=ancestor=nexus3:elpmaxe) \
    /bin/bash
```

- Use `id` inside the container to check user permissions.

```
[nexus@ef88954bd4e9 nexus]$ id
uid=200(nexus) gid=200(nexus) groups=200(nexus)
```

- Check user inside the container has sufficient permissions to mounted volume.

[1]: https://books.sonatype.com/nexus-book/3.0/reference/install.html#config-context-path
[2]: https://github.com/sonatype/docker-nexus3
