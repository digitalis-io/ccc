# CCC (Containerized Cassandra Cluster)
Clean and simple containerized Cassandra cluster for local testing. A modern alternative to [ccm](https://github.com/digitalis-io/dcc).

This approach is based on [official docker image](https://hub.docker.com/_/cassandra/), but still lets you control all the config files without a need to build a custom image. Which is described in more details in this [blog post](https://digitalis.io/blog/)

## Quick start
```
./setup-config.sh
docker-compose up -d

# Check the cluster status
docker exec cass1  nodetool status
```
   - That will bring up a 3 node Cassandra cluster. You can configure the number of Cassandra containers in [docker-compose.yml](docker-compose.yml)
   - Cassandra data is stored under `./data/` on the host and kept even if the cluster is destroyed
   - Cassandra configuration files are under `./etc/`.
   - You can access the cluster from your app on `localhost:9042` 
   - Or can add a container with your app to [docker-compose.yml](docker-compose.yml)

If you want to start fresh again (no data, vanilla config):
```
sudo rm -r ./data/* ./etc/*
```

## Destroying the cluster
```
docker-compose down
```

## nodetool
```
docker exec cass1  nodetool ...
```

## cqlsh
```
docker exec cass1 cqlsh -e "DESCRIBE keyspaces"

# Or in interactive shell mode
docker exec -it cass1 cqlsh
```

## Cassandra logs
```
docker logs -f cass1
```

## Basic configuration
Edit [docker-compose.yml](docker-compose.yml) for your needs, keeping in mind the following:

   - An [official docker image](https://hub.docker.com/_/cassandra/) for cassandra must be used
   - You should specify an explicit version tag
   - Each cassandra container needs to have two volumes configured, one for data and one for config files
   - Some basic Cassandra configuration can be done through `CASSANDRA_` environment  variables
   - Check the syntax with `docker-compose config` before running `./setup-config.sh`

## Cassandra configuration
If you need more advanced configuration, you can edit config files (eg. cassandra.yaml) for each node under `etc/<node>/` and simply run

```
docker-compose restart
```

Some practical examples can be found in this [blog post](https://digitalis.io/blog/)
