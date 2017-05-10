# docker-tunnel

For when you want to "wrap" a service running on different docker network for local access.  This is useful for example when you don't want to expose ports in the original service (because you might for instance not know what ports will be free or where the stack will run etc.).

# usage

With `docker-compose` you can create a tunnel sending all the local trafic to some service in a specified docker network

``` yaml
version: '2'

services:
  tunnel:
    image: Fuco1/docker-tunnel
    ports:
      # enable ssh tunelling from localhost only
      - "127.0.0.1:2222:22"
    networks:
      - app-backend-network

networks:
  app-backend-network:
    external:
      name: app_backend-network

```

Then start it up with

    docker-compose up -d

and set up a tunnel with ssh

    # suppose there is a service `mysql` in `backend-network` for docker-compose stack `app`
    ssh -T root@127.0.0.1 -p 2222 -L 127.0.0.1:33306:mysql:3306
