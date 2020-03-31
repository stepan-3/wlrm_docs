# Updating the Docker Container

To update the Wallarm modules installed inside the Docker
container, you must:

1. Download the updated image.
2. Stop the running container.
3. Start the container on the new image.

## 1. Download the Updated Image

Run the command:

```
# docker pull wallarm/node
```

## 2. Stop the Running Container

Run the command:

```
# docker stop <container name>
```

## 3. Start the Container on the New Image.

Run the command:

```
# docker run -d -v /path/to/license.key:/etc/wallarm/license.key -v /path/to/node.yaml:/etc/wallarm/node.yaml -e NGINX_BACKEND=93.184.216.34 wallarm/node
```

!!! info "See also"
      [Deploying with Docker](installation-docker-en.md)

<br>
