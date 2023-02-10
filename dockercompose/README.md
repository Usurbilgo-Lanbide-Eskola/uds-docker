# Set guacamole tunnel token 

In order to comunicate with uds broker the guacamole tunnel must have a valid token.
This token can be any hash under 48 characters.  Generate this chain with any tool you want.
(in python you can use hashlib.sha256().hexdigest[:48])
Then edit guacamoletunnel/guacamole.properties file and replace GENERATED_TOKEN whit the 
created token.

# Run docker compose file

Go to the cloned directory and run 
```
docker compose up -d
```

# Create / update database tables

Open a shell in the udsbroker service and run
```
python manage.py migrate
```

# Register the tunnel token in the db

Open a shell in the udsbroker service and run
```
python manage.py migrate

>>> import datetime
>>> from uds.models.tunnel_token import TunnelToken
>>> token=YOUR_GENERATED_TOKEN
>>> tunnel_id=YOUR_TUNNEL_SERVER_IP
>>> TunnelToken.objects.create(ip=tunnel, token=token,stamp=datetime.datetime.now())
```