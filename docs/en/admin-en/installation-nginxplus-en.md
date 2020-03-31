# Installing with NGINX Plus

## Installation Options

--8<-- "en/include-en/installation-options-en.md"

    #### Warning:: Installation of postanalytics on a separate server
    If you are planning to install postanalytics on a separate server,
    you must install postanalytics first. See details in [Separate postanalytics installation](installation-postanalytics-en.md).

To install the filter node with NGINX Plus, you must:

1. Install NGINX Plus.
2. Add the Wallarm repositories, from which you will download packages.
3. Install the Wallarm packages.
4. Configure postanalytics.
5. Connect the Wallarm module.
6. Set up the filter node for using a proxy server.
7. Connect the filter node to the Wallarm cloud.
8. Configure the server addresses of postanalytics.
9. Configure the filtration mode.
10. Configure logging.
11. Restart NGINX Plus.

<!-- -->
    
--8<-- "en/include-en/elevated-priveleges.md"

<!-- --> 

## 1. Install NGINX Plus

[See the official NGINX installation instructions](https://www.nginx.com/resources/admin-guide/installing-nginx-plus/).

!!! info "Installing on Amazon Linux 2"
    To install NGINX on Amazon Linux 2, use the CentOS 7 instruction.

<!-- -->

## 2. Add the Wallarm Repositories

The installation and updating of NGINX Plus with the Wallarm module is done from the Wallarm
repositories.

Depending on your operating system, run one of the commands:

--8<-- "en/include-en/add-repo-en.md"

<!-- -->

--8<-- "en/include-en/access-repo-en.md"

<!-- -->

## 3. Install the Wallarm Packages

To run postanalytics and process the requests on the same server, you must
install the following packages:

* NGINX Plus with the Wallarm module.
* Postanalytics.

To only process the requests on the server, you must install the following
package:

* nginx-plus-module-wallarm.

### Install the Requests Processing and Postanalytics on the Same Server

--8<-- "en/include-en/install-nginx-plus-postanalytics-en.md"

<!-- -->

### Install Only the Requests Processing

--8<-- "en/include-en/install-nginx-plus-en.md"

<!-- -->

## 4. Configure Postanalytics

    #### Info::
    Skip this step if you installed postanalytics on a separate server as you already have your postanalytics configured.

<!-- -->

Postanalytics uses the in-memory storage Tarantool.

You must set the amount of server RAM allocated to Tarantool.

--8<-- "en/include-en/configure-postanalytics-en.md"

<!-- -->

## 5. Connect the Wallarm Module

In the file `/etc/nginx/nginx.conf`, add the following directive right after the `worker_processes` directive:

```
load_module modules/ngx_http_wallarm_module.so;
```

Confguration example with the added directive:

```
user  nginx;
worker_processes  auto;
load_module modules/ngx_http_wallarm_module.so;

error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;
```

Copy the configuration files for the system setup:

```term
$ cp /usr/share/doc/nginx-plus-module-wallarm/examples/*.conf /etc/nginx/conf.d/
```

<!-- -->

## 6. Set up the Filter Node for Using a Proxy Server

--8<-- "en/include-en/setup-proxy.md"

<!-- -->

## 7. Connect the Filter Node to the Wallarm Cloud

--8<-- "en/include-en/connect-cloud-en.md"

<!-- -->

## 8. Configure the Server Addresses of Postanalytics

    #### Info::
      * Skip this step if you installed postanalytics and the filter node on the same server.
      * Do this step if you installed postanalytics and the filter node on separate servers.

<!-- -->

--8<-- "en/include-en/configure-postanalytics-address-nginx-en.md"

<!-- -->

## 9. Configure the Filtration Mode

--8<-- "en/include-en/setup-filter-nginx-en.md"

<!-- -->

## 10. Configure Logging

<!-- -->
--8<-- "en/include-en/installation-step-logging.md"
<!-- -->

## 11. Restart NGINX

--8<-- "en/include-en/root_perm_info.md"

<!-- -->

<!-- -->

--8<-- "en/include-en/restart-nginx-en.md"


<!-- -->

## The Installation Is Complete

--8<-- "en/include-en/check-setup-installation-en.md"

<!-- -->

--8<-- "en/include-en/filter-node-defaults.md"

<!-- -->

--8<-- "en/include-en/installation-extra-steps.md"