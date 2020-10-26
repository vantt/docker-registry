# Docker Registry Htpasswd

This provides a ready to use setup for a docker registry running behind a tls-termination front proxy as traefik or haproxy forwards http traffic to port *5000*.

The authentication method uses default basic htpasswd authentication.

## To add more users to httpasswd file

Generate your own `bcrypt` username and password combinations and put them in `registry/config/htpasswd` using:

```bash

$ htpasswd -B config/htpasswd vantt

New password:
Re-type new password: 
Adding password for user vantt

```

By default these users are configured as sample below:

```

vantt:$adjgasdfljashldffadf
john:doe
foo:bar
healthchecker:healthchecker

```

## To run Registry

```

docker-compose up -d

```


# Logging in

You have to call `docker login https://yourregistry.domain.com` with whatever URL you have configure your registry to run on.

If you don't want to interactively enter a username and password, you can
store it in `~/.docker/config.json` like so:


```json
{
  "auths": {
    "yourregistry.domain.com": {
      "auth": "Zm9vOmJhcg==",
        "email": "foo@bar.com"
    }
  }
}
```

### With the Auth-String created by

```bash
  
  $ echo "username:password" | base64

  Zm9vOmJhcg==

```

Now you can paste your auth string into config.json


# Test Docker v2 HTTP API

To manually test the Docker v2 HTTP API you can run this command:

```

# Authorization token created by:  echo "username:password" | base64

curl -k -v -H "Authorization: Basic Zm9vOmJhcg==" https://yourregistry.domain.com/v2/


```

It should give you an `HTTP 200 OK`.

Have fun!


# Push images to Registry

```

docker build -t tagName .

docker tag tagName yourregistry.domain.com:remoteTag

docker push yourregistry.domain.com:remoteTag


```

# Run your own TLS Http server without HAProxy or Traefik

You only need to replace the `certs/registry.crt` and `certs/registry.key`
with your own certificate and key file.


# Images Management
https://hub.docker.com/r/anoxis/registry-cli

### list all images

```
docker run --rm anoxis/registry-cli -r https://registry.miczone.asia -l username:password
```

### delete all tags off all images, keep 10 tags

```
docker run --rm anoxis/registry-cli -r https://registry.miczone.asia -l username:password --delete --num 10

```

### delete a specific tags

```
docker run --rm anoxis/registry-cli -r https://registry.miczone.asia -l username:password -i devops/nginx --delete --tags-like "specific_tags"
```

### delete specificc tag using --tags-like

```
docker run --rm anoxis/registry-cli -r https://registry.miczone.asia -l username:password -i devops/nginx --delete --tags-like "specific_tags"
```

### delete many tags using --tags-like

```
--delete --tags-like "snapshot-" --keep-tags "stable" "latest"

```