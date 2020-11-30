# docker-autoheal-log

Based on https://github.com/willfarrell/docker-autoheal

Monitor container logs plus healthcheck status and restart docker containers.
Some containers report healthy even though some core functionality isn't working. This lets you monitor the container logs for error string in addition to healthcheck and restart if error condition occurs.

## Supported tags and Dockerfile links
- [`latest` (*Dockerfile*)](https://github.com/ahmedsajid/docker-autoheal-log-log/blob/master/Dockerfile)

[![](https://images.microbadger.com/badges/version/ahmedsajid/autoheal-log.svg)](http://microbadger.com/images/ahmedsajid/autoheal-log "Get your own version badge on microbadger.com")  [![](https://images.microbadger.com/badges/image/ahmedsajid/autoheal-log.svg)](http://microbadger.com/images/ahmedsajid/autoheal-log "Get your own image badge on microbadger.com")

## How to use
```bash
docker run -d \
    --name autoheal \
    --restart=always \
    -e AUTOHEAL_CONTAINER_LABEL=all \
    -v /var/run/docker.sock:/var/run/docker.sock \
    ahmedsajid/autoheal-log
```
a) Apply the label `autoheal=true` to your container to have it watched.

b) Set ENV `AUTOHEAL_CONTAINER_LABEL=all` to watch all running containers.

c) Set ENV `AUTOHEAL_CONTAINER_LABEL` to existing label name that has the value `true`.

Note: You must specify `AUTOHEAL_LOG_STRING` for log monitoring.

## ENV Defaults
```
AUTOHEAL_CONTAINER_LABEL=all
AUTOHEAL_INTERVAL=5   # check every 5 seconds
AUTOHEAL_START_PERIOD=0   # wait 0 seconds before first health check
AUTOHEAL_DEFAULT_STOP_TIMEOUT=10   # Docker waits max 10 seconds (the Docker default) for a container to stop before killing during restarts (container overridable via label, see below)
AUTOHEAL_LOG_SINCE=10    # only inspect logs in last 10 seconds
DOCKER_SOCK=/var/run/docker.sock   # Unix socket for curl requests to Docker API
CURL_TIMEOUT=30     # --max-time seconds for curl requests to Docker API
```

## Optional ENV
```
AUTOHEAL_LOG_TAIL=100    # only inspect last 100 lines
```
Only 1 AUTOHEAL_LOG_* is supported.

### Optional Container Labels
```
autoheal.stop.timeout=20        # Per containers override for stop timeout seconds during restart
```

## Testing
```bash
docker build -t autoheal-log .

docker run -d \
    -e AUTOHEAL_CONTAINER_LABEL=all \
    -v /var/run/docker.sock:/var/run/docker.sock \
    autoheal-log
```
