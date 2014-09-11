# Docker Redis

Dockerfile to build a Redis Server container image which can be linked to other containers.

## Table of Contents
- [Version](#version)
- [Installation](#installation)
- [How To Use](#how-to-use)
- [Configuration](#configuration)
    - [Ports](#ports)
    - [Data Store](#data-store)
- [Upgrading](#upgrading)
- [Credit](#credit)

## Version

The current release (2.8.14) contains scripts to install Redis Server v2.8.14, and uses the Brightcommerce Ubuntu 14.04 base image. Our version numbers will reflect the version of Redis Server being installed.

## Installation

Pull the latest version of the image from the Docker Index. This is the recommended method of installation as it is easier to update the image in the future. These builds are performed by the **Docker Trusted Build** service.

``` bash
docker pull brightcommerce/redis:latest
```

or specify a tagged version:

``` bash
docker pull brightcommerce/redis:2.8.14
```

Alternately you can build the image yourself:

``` bash
git clone https://github.com/brightcommerce/docker-redis.git
cd docker-redis
docker build -t="$USER/redis" .
```

## How To Use

Run the Redis image:

``` bash
docker run --name redis -d brightcommerce/redis:latest
```

To test if the Redis server is configured properly, try connecting to the server.

``` bash
redis-cli -h $(docker inspect --format {{.NetworkSettings.IPAddress}} redis)
```

If the `redis-cli` client isn't installed on the host you can use `docker-enter` to shell into the container and run it from there:

``` bash
sudo docker-enter redis
root@9a34e83ea636:~# redis-cli
127.0.0.1:6379> INFO server
# Server
redis_version:2.8.14
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:b84ccf22550d3c52
redis_mode:standalone
os:Linux 3.16.1+ x86_64
arch_bits:64
multiplexing_api:epoll
gcc_version:4.8.2
process_id:11
run_id:cc85d4e2aefa545699aa4710a56d240688fcd0e8
tcp_port:6379
uptime_in_seconds:330
uptime_in_days:0
hz:10
lru_clock:1058886
config_file:/etc/redis/redis.conf
127.0.0.1:6379> QUIT
root@9a34e83ea636:~# logout
```

## Configuration

### Ports

This installation exposes port `6379`.

### Data Store

For data persistence a volume should be mounted at `/var/lib/redis`.

The updated run command looks like this:

``` bash
mkdir -p /opt/redis
docker run -name redis -d -v /opt/redis:/var/lib/redis brightcommerce/redis:latest
```

This will make sure that the data stored in the database is not lost when the image is stopped and restarted.

## Upgrading

To upgrade to newer releases, simply follow this 3 step upgrade procedure.

- **Step 1**: Stop the currently running image:

``` bash
docker stop redis
```

- **Step 2**: Update the docker image:

``` bash
docker pull brightcommerce/redis:latest
```

- **Step 3**: Start the image:

``` bash
docker run -name redis -d [OPTIONS] brightcommerce/redis:latest
```

## Credit

This repository was based on the work of [docker-redis by Sameer Naik](https://github.com/sameersbn/docker-redis).
