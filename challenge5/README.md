## Challenge 5

![image](../images/challenge5.png)

We now need to wire up more services and discover them through consul. This will allow us to add in the serializer microservice, temperature worker, influx database, NATS message server, and SmartThings microservice.

Your challenge is to update the `sensor/lib/index.js` file so that the `writeData` function is able to retrieve the address from Consul using the Piloted module. After this is done, you will need to update the `containerpilot.json5` configuration file for the sensor to have a watch for the 'serializer' and an onchange job, similar to the existing one for NATS.


## Run

`docker-compose up -d`

`open http://localhost:8500/`


```
$ docker ps
CONTAINER ID        IMAGE                                COMMAND                  CREATED             STATUS              PORTS                                                                            NAMES
7894ab5a100a        challenge5_gateway                   "/bin/containerpilot"    4 minutes ago       Up 4 minutes        0.0.0.0:8080->8080/tcp                                                           challenge5_gateway_1
0d2338a61711        challenge5_serializer                "/bin/containerpilot"    4 minutes ago       Up 4 minutes        10000/tcp                                                                        challenge5_serializer_1
696b9dbb6030        challenge5_temperature               "/bin/containerpilot"    4 minutes ago       Up 4 minutes                                                                                         challenge5_temperature_1
5efe082baa55        autopilotpattern/nats:0.9.6-r1.0.0   "containerpilot ma..."   4 minutes ago       Up 4 minutes        4222/tcp, 6222/tcp, 0.0.0.0:32768->8222/tcp                                      challenge5_nats_1
a627d18f64c3        challenge5_frontend                  "/bin/containerpilot"    4 minutes ago       Up 4 minutes        8080/tcp                                                                         challenge5_frontend_1
d984bcdec061        autopilotpattern/influxdb:1.1.1      "containerpilot in..."   4 minutes ago       Up 4 minutes        0.0.0.0:8083->8083/tcp, 0.0.0.0:8086->8086/tcp                                   challenge5_influxdb_1
f211257c572a        challenge5_smartthings               "/bin/containerpilot"    4 minutes ago       Up 4 minutes        8080/tcp                                                                         challenge5_smartthings_1
8b19187dcb9e        autopilotpattern/consul:0.7.3r39     "/usr/local/bin/co..."   4 minutes ago       Up 4 minutes        53/tcp, 53/udp, 8300-8302/tcp, 8400/tcp, 8301-8302/udp, 0.0.0.0:8500->8500/tcp   challenge5_consul_1
56a7479d2134        redis                                "docker-entrypoint..."   4 minutes ago       Up 4 minutes        6379/tcp                                                                         challenge5_redis_1
```

```
$ docker logs --tail 10 696b9dbb6030
2017/07/25 21:33:59   isServer: true,
2017/07/25 21:33:59   data: null,
2017/07/25 21:33:59   output:
2017/07/25 21:33:59    { statusCode: 502,
2017/07/25 21:33:59      payload:
2017/07/25 21:33:59       { message: 'Client request error: getaddrinfo ENOTFOUND http http:80',
2017/07/25 21:33:59         statusCode: 502,
2017/07/25 21:33:59         error: 'Bad Gateway' },
2017/07/25 21:33:59      headers: {} },
2017/07/25 21:33:59   reformat: [Function] }
```

```
$ docker-compose restart
Restarting challenge5_gateway_1 ... done
Restarting challenge5_serializer_1 ... done
Restarting challenge5_temperature_1 ... done
Restarting challenge5_nats_1 ... done
Restarting challenge5_frontend_1 ... done
Restarting challenge5_influxdb_1 ... done
Restarting challenge5_smartthings_1 ... done
Restarting challenge5_consul_1 ... done
Restarting challenge5_redis_1 ... done
```


`docker-compose down`

```
$ docker ps
CONTAINER ID        IMAGE                                COMMAND                  CREATED              STATUS              PORTS                                                                            NAMES
63898c297594        challenge5_gateway                   "/bin/containerpilot"    About a minute ago   Up About a minute   0.0.0.0:8080->8080/tcp                                                           challenge5_gateway_1
f674e12c5cc8        challenge5_smartthings               "/bin/containerpilot"    About a minute ago   Up About a minute   8080/tcp                                                                         challenge5_smartthings_1
d76044186c16        challenge5_frontend                  "/bin/containerpilot"    About a minute ago   Up About a minute   8080/tcp                                                                         challenge5_frontend_1
9ec507b95206        challenge5_temperature               "/bin/containerpilot"    About a minute ago   Up About a minute                                                                                    challenge5_temperature_1
d06257a57562        autopilotpattern/influxdb:1.1.1      "containerpilot in..."   About a minute ago   Up About a minute   0.0.0.0:8083->8083/tcp, 0.0.0.0:8086->8086/tcp                                   challenge5_influxdb_1
cedb70d9a3b1        challenge5_serializer                "/bin/containerpilot"    About a minute ago   Up About a minute   10000/tcp                                                                        challenge5_serializer_1
37fdbf1ef178        autopilotpattern/nats:0.9.6-r1.0.0   "containerpilot ma..."   About a minute ago   Up About a minute   4222/tcp, 6222/tcp, 0.0.0.0:32775->8222/tcp                                      challenge5_nats_1
215a5944cebb        autopilotpattern/consul:0.7.3r39     "/usr/local/bin/co..."   About a minute ago   Up About a minute   53/tcp, 53/udp, 8300-8302/tcp, 8400/tcp, 8301-8302/udp, 0.0.0.0:8500->8500/tcp   challenge5_consul_1
c9c3d2a18ecd        redis                                "docker-entrypoint..."   About a minute ago   Up About a minute   6379/tcp                                                                         challenge5_redis_1
```



```
gchiodo-mbp-2386:challenge5 gchiodo:master$ docker logs --tail 15 63898c297594
2017/07/25 22:13:22     2017/07/25 22:13:22 [ERR] http: Request GET /v1/health/service/frontend?passing=1, error: No known Consul servers from=127.0.0.1:57026
2017/07/25 22:13:22 failed to query frontend: Unexpected response code: 500 (No known Consul servers) [<nil>]
2017/07/25 22:13:22     2017/07/25 22:13:22 [ERR] agent: failed to sync changes: No known Consul servers
2017/07/25 22:13:24     2017/07/25 22:13:24 [ERR] http: Request GET /v1/health/service/frontend?passing=1, error: No known Consul servers from=127.0.0.1:57026
2017/07/25 22:13:24 failed to query frontend: Unexpected response code: 500 (No known Consul servers) [<nil>]
2017/07/25 22:13:24 ==> Failed to check for updates: Get https://checkpoint-api.hashicorp.com/v1/check/consul?arch=amd64&os=linux&signature=54165e60-e8e6-73cd-4dfb-1e48dea7d817&version=0.7.0: dial tcp: i/o timeout
2017/07/25 22:13:25     2017/07/25 22:13:25 [ERR] agent: failed to sync changes: No known Consul servers
2017/07/25 22:13:26     2017/07/25 22:13:26 [ERR] agent: failed to sync remote state: No known Consul servers
2017/07/25 22:13:26     2017/07/25 22:13:26 [ERR] http: Request GET /v1/health/service/frontend?passing=1, error: No known Consul servers from=127.0.0.1:57026
2017/07/25 22:13:26 failed to query frontend: Unexpected response code: 500 (No known Consul servers) [<nil>]
2017/07/25 22:13:28     2017/07/25 22:13:28 [ERR] http: Request GET /v1/health/service/frontend?passing=1, error: No known Consul servers from=127.0.0.1:57026
2017/07/25 22:13:28 failed to query frontend: Unexpected response code: 500 (No known Consul servers) [<nil>]
2017/07/25 22:13:28     2017/07/25 22:13:28 [ERR] agent: failed to sync changes: No known Consul servers
2017/07/25 22:13:30     2017/07/25 22:13:30 [ERR] http: Request GET /v1/health/service/frontend?passing=1, error: No known Consul servers from=127.0.0.1:57026
2017/07/25 22:13:30 failed to query frontend: Unexpected response code: 500 (No known Consul servers) [<nil>]
```


```
$ docker logs --tail 15 9ec507b95206
2017/07/25 22:15:49   port: 80,
2017/07/25 22:15:49   trace:
2017/07/25 22:15:49    [ { method: 'POST',
2017/07/25 22:15:49        url: 'http://http://serializer:10000/write/temperature' } ],
2017/07/25 22:15:49   isBoom: true,
2017/07/25 22:15:49   isServer: true,
2017/07/25 22:15:49   data: null,
2017/07/25 22:15:49   output:
2017/07/25 22:15:49    { statusCode: 502,
2017/07/25 22:15:49      payload:
2017/07/25 22:15:49       { message: 'Client request error: getaddrinfo ENOTFOUND http http:80',
2017/07/25 22:15:49         statusCode: 502,
2017/07/25 22:15:49         error: 'Bad Gateway' },
2017/07/25 22:15:49      headers: {} },
2017/07/25 22:15:49   reformat: [Function] }
```

__hint__ ContainerPilot documentation can be found at https://www.joyent.com/containerpilot/docs

__hint__ Piloted documentation can be found at https://www.npmjs.com/package/piloted

### [Solution](./SOLUTION.md)

## Next Up: [Challenge 6](../challenge6/README.md)
