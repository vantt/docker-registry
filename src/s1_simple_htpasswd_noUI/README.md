# Docker Registry Htpasswd

This provides a ready to use setup for a docker registry running with htpasswd
authentication.

## Add user to httpasswd file

```bash

$ htpasswd -B config/htpasswd admin

New password:
Re-type new password: 
Adding password for user admin

```

By default these users are configured:

```

john:doe
foo:bar
healthchecker:healthchecker

```

# Logging in

You have to call `docker login 0.0.0.0:443` with whatever URL you have configure your registry to run on.

If you don't want to interactively enter a username and password, you can
store it in `~/.docker/config.json` like so:



```json
{
  "auths": {
    "0.0.0.0:443": {
      "auth": "Zm9vOmJhcg==",
        "email": "foo@bar.com"
    }
  }
}
```

### Create your auth string

```bash
  
  $ echo "username:password" | base64

  Zm9vOmJhcg==

```

Now you can paste your auth string into config.json


# Test Docker v2 HTTP API

To manually test the Docker v2 HTTP API you can run this command:

```
curl -k -v -H "Authorization: Basic Zm9vOmJhcg==" https://0.0.0.0:443/v2/
```

It should give you an `HTTP 200 OK`.

Have fun!

#####

You only need to replace the `certs/registry.crt` and `certs/registry.key`
with your own certificate and key file and generate your own `bcrypt` username
and password combinations and put them in `registry/conf/htpasswd` using:


# Registry Management
https://hub.docker.com/r/anoxis/registry-cli

### list all images
docker run --rm anoxis/registry-cli -r https://registry.miczone.asia -l admin:123456

### delete all tags off all images, keep 10 tags
docker run --rm anoxis/registry-cli -r https://registry.miczone.asia -l admin:123456 --delete --num 10

### delete a specific tags
docker run --rm anoxis/registry-cli -r https://registry.miczone.asia -l admin:123456 -i devops/nginx --delete --tags-like "specific_tags"

### delete tags like
docker run --rm anoxis/registry-cli -r https://registry.miczone.asia -l admin:123456 -i devops/nginx --delete --tags-like "specific_tags"

--delete --tags-like "snapshot-" --keep-tags "stable" "latest"
