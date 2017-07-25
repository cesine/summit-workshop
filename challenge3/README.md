## Challenge 3

![image](../images/challenge3.png)

We want to put a gateway in front of the frontend service so that we can scale the frontend to more than one instance. To do this we will use docker-compose with a couple of frontend containers (frontend1 and frontend2). Your challenge is to update the _gateway/gateway.config.yml_ with the `urls` pointing to frontend1 and frontend2, then add `links` in the docker-compose for the gateway to reference the frontend1 and frontend2 instances. Verify the results by running `docker-compose up -d` and navigating to the gateway port (localhost:8080).

## Build

docker-compose up -d



## Debugging

```
$ docker ps
CONTAINER ID        IMAGE                  COMMAND             CREATED             STATUS              PORTS                    NAMES
dc153d64422e        challenge3_gateway     "node server.js"    13 minutes ago      Up 49 seconds       0.0.0.0:8080->8080/tcp   challenge3_gateway_1
0364273b7d40        challenge3_frontend1   "node ."            13 minutes ago      Up 13 minutes       8080/tcp                 challenge3_frontend1_1
98a2da83e8ee        challenge3_frontend2   "node ."            13 minutes ago      Up 13 minutes       8080/tcp                 challenge3_frontend2_1
```


```
$ docker logs --tail 10 dc153d64422e
    at GetAddrInfoReqWrap.onlookup [as oncomplete] (dns.js:92:26)
error: [EG:db] Error in Redis:  Error: Redis connection to redis:6379 failed - getaddrinfo ENOTFOUND redis redis:6379
    at errnoException (dns.js:50:10)
    at GetAddrInfoReqWrap.onlookup [as oncomplete] (dns.js:92:26)
error: [EG:db] Error in Redis:  Error: Redis connection to redis:6379 failed - getaddrinfo ENOTFOUND redis redis:6379
    at errnoException (dns.js:50:10)
    at GetAddrInfoReqWrap.onlookup [as oncomplete] (dns.js:92:26)
error: [EG:db] Error in Redis:  Error: Redis connection to redis:6379 failed - getaddrinfo ENOTFOUND redis redis:6379
    at errnoException (dns.js:50:10)
    at GetAddrInfoReqWrap.onlookup [as oncomplete] (dns.js:92:26)
```

__hint__ information on express-gateway can be found at http://express-gateway.io

__hint__ docker-compose documentation can be found at https://docs.docker.com/compose/compose-file/compose-file-v2/

### [Solution](./SOLUTION.md)

## Next Up: [Challenge 4](../challenge4/README.md)
