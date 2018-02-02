# docker-registry-https
How to setup a secure public docker registry with authentication


## generate basic auth password

```plaintext
openssl passwd -apr1 passw0rd
$apr1$9X801lxd$FiE7SMCAkJulRr.gI5I9z0
```

You need to escape any $ with $$ in the docker-compose file
```plaintext
$$apr1$$9X801lxd$$FiE7SMCAkJulRr.gI5I9z0
```


