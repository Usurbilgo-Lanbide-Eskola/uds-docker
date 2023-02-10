# Create SSL certificates

Before running docker compose create ssl certificates in a directory called certs
```
openssl req -nodes -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -sha256 -days 1825
```


and

```
openssl dhparam -out dhparam.pem 4096
```

# Run docker compose file

Go to the cloned directory and run 
```
docker-compose up -d
```
