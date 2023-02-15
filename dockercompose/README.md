# Set tunnel token 

In order to comunicate with uds broker the guacamole and uds tunnel server must have a valid token.
This token can be any hash under 48 characters.  Generate this chain with any tool you want.
(in python you can use hashlib.sha256().hexdigest[:48])

## Run docker compose file

Go to the cloned directory and run 
```
docker compose up -d
```

## Create / update database tables

Open a shell in the udsbroker service and run
```
python manage.py migrate
```

## Register the tunnel token in the db

Open a shell in the udsbroker service and run
```
python manage.py migrate

>>> import datetime
>>> from uds.models.tunnel_token import TunnelToken
>>> token=YOUR_GENERATED_TOKEN
>>> tunnel_id=YOUR_TUNNEL_SERVER_IP
>>> TunnelToken.objects.create(ip=tunnel, token=token,stamp=datetime.datetime.now())
```

## Configure token in guacamole and uds tunnel server

Edit guacamoletunnel/guacamole.properties and uds-tunnel/udstunnel.conf files to replace GENERATED_TOKEN
literal with the created token.

## Restart docker compose environment

# Web access

This docker compose environment uses Caddy to manage HTTP/HTTPS access and SSL certificates. In order to setup
a valid certificate chain, you should change caddy/Caddyfile file and change `openuds35` with a full qualified domain 
name pointing to the public ip running the environment.

If Caddy detects a fqdn it will try to create a valid SSL certificate, if it detects a local/internal ip it will create
self signeg certicates (https://caddyserver.com/docs/automatic-https)

