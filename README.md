# devproxy

Proxy for local development based on Traefik 2.x

## Why

To avoid port clashes when working on different (Web-)projects we containerize everything using Docker and make the services available under the `.localhost` domain.

## Setup

Start https://traefik.io with

```
docker-compose up
```
