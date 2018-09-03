# docker-registry-https
How to setup a public docker registry with authentication and letsencrypt


## generate basic auth password

```
openssl passwd -apr1 passw0rd
$apr1$9X801lxd$FiE7SMCAkJulRr.gI5I9z0
```

You need to escape any $ with $$ in the docker-compose file
```plaintext
$$apr1$$9X801lxd$$FiE7SMCAkJulRr.gI5I9z0
```


docker-compose.yml

    version: '2'
    services:
    
        traefik:
         image: traefik:1.6-alpine
         restart: always
         ports:
          - 80:80
          - 443:443
         networks:
          - web
         volumes:
          - /var/run/docker.sock:/var/run/docker.sock
          - ./traefik/traefik.toml:/etc/traefik/traefik.toml
          - ./traefik/acme.json:/acme.json
          - ./traefik/logs:/logs
         container_name: traefik
    
        registry:
         image: registry:2
         restart: always
         volumes:
          - ./registry:/var/lib/registry
         labels:
           - "traefik.backend=registry"
           - "traefik.frontend.rule=Host:registry.example.com"
           - "traefik.enable=true"
           - "traefik.frontend.auth.basic=test:$$apr1$$haZC4zC.$$3GgIrlER1m7U9iUCnHNiP1"
           - "traefik.port=5000"
         container_name: registry
         depends_on:
           - traefik
         networks:
           - web
    
    
    networks:
      web:
        external: true