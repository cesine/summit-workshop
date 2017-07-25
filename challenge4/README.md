## Challenge 4

![image](../images/challenge4.png)

Using docker links isn't a good method for scaling. Instead we will add consul and use ContainerPilot to register our services with consul for us. Once this is done we can use the `docker-compose scale` command to add more instances of the frontend and have them balanced through the gateway. Your challenge is to update the gateway/etc/containerpilot.json5 file to run the `bin/generate-config` file before starting the gateway and on any change to the frontend that is detected in consul.

## Run

`docker-compose up --scale SERVICE=2`

`docker-compose up -d`



`open http://localhost:8500/`

## Debugging

```
$ docker ps
CONTAINER ID        IMAGE                              COMMAND                  CREATED              STATUS              PORTS                                                                            NAMES
9f22173db1d2        challenge4_frontend                "/bin/containerpilot"    About a minute ago   Up About a minute   8080/tcp                                                                         challenge4_frontend_1
b3ec24224cd7        autopilotpattern/consul:0.7.3r39   "/usr/local/bin/co..."   About a minute ago   Up About a minute   53/tcp, 53/udp, 8300-8302/tcp, 8400/tcp, 8301-8302/udp, 0.0.0.0:8500->8500/tcp   challenge4_consul_1
48dd96939b49        redis                              "docker-entrypoint..."   About a minute ago   Up About a minute   6379/tcp                                                                         challenge4_redis_1
dc153d64422e        challenge3_gateway                 "node server.js"         23 minutes ago       Up 19 seconds       0.0.0.0:8080->8080/tcp                                                           challenge3_gateway_1
0364273b7d40        challenge3_frontend1               "node ."                 23 minutes ago       Up 23 minutes       8080/tcp                                                                         challenge3_frontend1_1
98a2da83e8ee        challenge3_frontend2               "node ."                 23 minutes ago       Up 23 minutes       8080/tcp                                                                         challenge3_frontend2_1
```


```
$ docker-compose up --scale SERVICE=2
gateway_1   | 2017/07/25 21:19:12 error: [EG:config] failed to (re)load gateway config: Error: ENOENT: no such file or directory, open '/opt/app/gateway.config.json'
gateway_1   | 2017/07/25 21:19:12 error: [EG:config]  Error: ENOENT: no such file or directory, open '/opt/app/gateway.config.json'
```


```
docker-compose down
```

__hint__ ContainerPilot documentation can be found at https://www.joyent.com/containerpilot/docs

__hint__ You need to add a job to execute `generate-config`

### [Solution](./SOLUTION.md)

## Next Up: [Challenge 5](../challenge5/README.md)
