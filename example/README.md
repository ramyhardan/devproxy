# Devproxy Usage Example

This is a simple example setup using two services:

- `web`: Some webapp Traefik should route traffic to.
- `db`: Accessible by `web` but not by Traefik (on its own network).

We split the `docker-compose.yml` into

- [docker-compose.base.yml](docker-compose.base.yml): Services without any Traefik specifics
- [docker-compose.dev.yml](docker-compose.dev.yml): Service with Traefik configuration

[docker-compose.dev.yml](docker-compose.dev.yml) contains the interesting parts:

```
networks:
  proxy:
    external:
      name: devproxy_default
```

Thsi includes Traefik's network

```
services:
  web:
    networks:
      - proxy
      - default
```

Next we make Traefik's network and the default network available to the `web` service.

```
services:
  web:
    networks:
      - proxy
      - default
    labels:
      - "traefik.enable=true"
      - "traefik.docker.network=devproxy_default"
      - "traefik.http.services.devproxy-example-web.loadbalancer.server.port=80"
```

Now we configure Traefik with labels. Make sure that the Traefik service name (`devproxy-example-web`) is unique across all services of all your projects that you proxy with Traefik.

## Starting The Services

`docker-compose -f docker-compose.base.yml -f docker-compose.dev.yml up` Starts the services and registers `web` with Traefik.
Then it's available at http://web-example.localhost/
