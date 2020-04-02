[doc-ip-blocking]:            configure-ip-blocking-en.md
[doc-wallarm-mode]:           configure-parameters-en.md#wallarmmode
[doc-config-params]:          configure-parameters-en.md
[doc-monitoring]:             monitoring/intro.md

# Installing with Docker (Using the NGINX-Based Docker Image)

The filter node can be deployed as a Docker container. The Docker container
is a fat one and contains all subsystems of the filter node.

The functionality of the filter node installed inside the Docker container
is completely identical to the functionality of the other deployment options.

To deploy the filter node as a Docker container, you must:

1. Deploy the filter node.
2. Connect the filter node to the Wallarm cloud.
3. Configure NGINX-Wallarm.
4. Configure logging.
5. Configure monitoring.

!!! warning "Known limitations"
    * [IP blocking][doc-ip-blocking] is not supported.
    * Most [Wallarm directives][doc-config-params] cannot be changed through environment variables; these directives must be written in configuration files inside the container.

## 1. Deploy the Filter Node

Run one of the `docker run` commands depending on the 

--8<-- "en/cloud-include/wallarm-clouds.md"

cloud in use: 

--8<-- "en/cloud-include/docker-run-command.md"

where:

* `example.com` — the protected resource.
* `deploy@example.com` — login to 
--8<-- "en/cloud-include/wallarm-portal.md"
.

* `very_secret` — password for 
--8<-- "en/cloud-include/wallarm-portal.md"
.
* `memvalue` – amount of memory allocated to Tarantool.

After running the command, you will have:

* The protected resource on port 80.
* The filter node registered in 
--8<-- "en/cloud-include/wallarm-portal.md"
; the filter node displayed in the Wallarm interface.

You can also fine-tune the deployment by putting additional configuration files
inside the container.

## 2. Connect the Filter Node to the Wallarm Cloud

The filter node interacts with the Wallarm cloud located on a remote server.

To connect the filter node to the Wallarm cloud, you have the following
options:

* Automatic registration.
* Using credentials.
* Using a prepared configuration file.

### Automatic Registration

Transfer the environment variables `DEPLOY_USER`,
`DEPLOY_PASSWORD` with the access credentials to 
--8<-- "en/cloud-include/wallarm-portal.md"
.

The filter node will try to automatically register itself in the Wallarm cloud on the first start.

If a filter node with the same name as the node's container identifier is already registered in the cloud, then the registration process will fail.

To avoid this, pass the `DEPLOY_FORCE=true` environment variable to the container.

```term
# docker run -d -e DEPLOY_USER="deploy@example.com" -e DEPLOY_PASSWORD="very_secret" -e NGINX_BACKEND="IP address or FQDN" wallarm/node
```

If the registration process finishes successfully, then the container's `/etc/wallarm` directory will be populated with the license file (`license.key`), a file with the credentials for the filter node to access the cloud (`node.yaml`), and other files required for proper node operation.

On the next start of the same filter node, registration will not be required. The filter node communicates with the cloud using the following artifacts acquired during the automatic registration:

* The `uuid` and `secret` values (they are placed in the `/etc/wallarm/node.yaml` file).
* The Wallarm license key (it is placed in the `/etc/wallarm/license.key` file).

To connect the already registered filter node to the cloud, pass to its container

* either the `uuid` and `secret` values via the environment variables and the `license.key` file
* or the `node.yaml` and `license.key` files.


### Use of Prepared Credentials

Pass to the filter node's container

* the `uuid` and `secret` values via the corresponding `NODE_UUID` and `NODE_SECRET` environment variables, and
* the `license.key` file via Docker volumes.

Run one of the `docker run` commands depending on the 
--8<-- "en/cloud-include/wallarm-clouds.md"
 cloud in use: 
--8<-- "en/cloud-include/docker-run-command-with-creds.md"

Commands are in the tabs below:

=== "EU Cloud"
   First tab

    docker run -d "NODE_UUID=00000000-0000-0000-0000-000000000000" -e NODE_SECRET="0000000000000000000000000000000000000000000000000000000000000000" -v /path/to/license.key:/etc/wallarm/license.key -e NGINX_BACKEND=192.168.xxx.1 wallarm/node

=== "US Cloud"
   Send tab

    docker run -d -e WALLARM_API_HOST=us1.api.wallarm.com -e "NODE_UUID=00000000-0000-0000-0000-000000000000" -e NODE_SECRET="0000000000000000000000000000000000000000000000000000000000000000" -v /path/to/license.key:/etc/wallarm/license.key -e NGINX_BACKEND=192.168.xxx.1 wallarm/node

### Use of a Prepared Configuration File Containing Credentials

Pass the following files to the filter node's container via Docker volumes:
* the `node.yaml` file, containing the credentials for the filter node to access the Wallarm cloud, and
* the `license.key` file.

``` bash
docker run -d -v /path/to/node.yaml:/etc/wallarm/node.yaml -v /path/to/license.key:/etc/wallarm/license.key -e NGINX_BACKEND=192.168.xxx.1 wallarm/node
```

## 3. Configure NGINX-Wallarm

The filter node configuration is done via the NGINX configuration file.

The use of container lets you go through a simplified configuration process
by using the environment variables. The simplified process is enabled by
transferring the `NGINX_BACKEND` environment variable.

### Simplified Process

* `NGINX_BACKEND` — The backend address to which all incoming requests
  must be transferred. If the address does not have the `http://` or `https://`,
  prefix, then `http://` is used by default. See details in
  [proxy_pass](https://nginx.org/ru/docs/http/ngx_http_proxy_module.html#proxy_pass).

  Do not use the `NGINX_BACKEND` variable if you do need the simplified configuration process and if you use your own configuration files.

  Note that without the `NGINX_BACKEND` variable, Wallarm will not start automatically. To start Wallarm, configure `wallarm_mode monitoring`. See details in the
   `wallarm_mode` directive [description][doc-wallarm-mode].
   
* `WALLARM_MODE`: The NGINX-Wallarm module operation mode. See details in the
   `wallarm_mode` directive [description][doc-wallarm-mode].

### Configuration Files

The directories used by NGINX:

* `/etc/nginx-wallarm/conf.d` — common settings.
* `/etc/nginx-wallarm/sites-enabled` — virtual host settings.
* `/var/www/html` — static files.

## 4. Configure Logging

The logging is enabled by default.

The log directories are:

* `/var/log/nginx-wallarm/` — NGINX logs.
* `/var/log/wallarm/` — Wallarm logs.

### Configure Extended Logging

--8<-- "en/include-en/installation-step-logging.md"

### Configure Log Rotation

By default, the logs rotate once every 24 hours.

Changing the rotation parameters through environment variables is not
possible. To set up the log rotation, change the configuration files in
`/etc/logrotate.d/`.

## 5. Configure Monitoring

To monitor the filter node, there are Nagios-compatible scripts inside
the container. See details in [Monitor the filter node][doc-monitoring].

Example of running the scripts:

``` bash
docker exec -it wallarm-node /usr/lib/nagios-plugins/check_wallarm_tarantool_timeframe -w 1800 -c 900
```

``` bash
docker exec -it wallarm-node /usr/lib/nagios-plugins/check_wallarm_export_delay -w 120 -c 300
```

## The Installation Is Complete

--8<-- "en/include-en/check-setup-installation-en.md"

--8<-- "en/include-en/filter-node-defaults.md"

--8<-- "en/include-en/installation-extra-steps.md"