# Installing as a Dynamic Module for NGINX

!!! warning "Commercial NGINX Plus and Open Source NGINX"
    The instructions in this section address the filter node installation as a dynamic module for the free open-source NGINX.

    If you are running the commercial NGINX Plus, you need a different set of instructions. See [Installing with NGINX Plus](installation-nginxplus-en.md).

If you have a running NGINX installed in your network infrastructure, you can install
Wallarm as a dynamic module for NGINX.

## Use with Official vs. Custom Builds of NGINX

Wallarm is compatible with NGINX installed from [official NGINX repositories](https://nginx.org/en/linux_packages.html).

If you are planning to install a custom build of NGINX, the dynamic module
from the Wallarm repository might be incompatible and not load. To rebuild
the dynamic module, contact [Wallarm Support](../cloud-include/contacting-support.md).

With your support request, provide the following information provided by the output of the given commands:

  * Linux kernel version: `uname -a`
  * Linux distributive: `cat /etc/*release`
  * NGINX version:

    * [NGINX official build](https://nginx.org/en/linux_packages.html): `/usr/sbin/nginx -V`
    * NGINX custom build: `<path to nginx>/nginx -V`

  * Compatibility signature:

    * [NGINX official build](https://nginx.org/en/linux_packages.html): `egrep -ao '.,.,.,[01]{33}' /usr/sbin/nginx`
    * NGINX custom build: `egrep -ao '.,.,.,[01]{33}' <path to nginx>/nginx`

  * The user (and the user's group) who is running the NGINX worker processes: `grep -w 'user' <path to the NGINX configuration files/nginx.conf>`

## Installation Options

--8<-- "en/include-en/installation-options-nginx-en.md"

!!! warning "Installation of postanalytics on a separate server"
    If you are planning to install postanalytics on a separate server,
    you must install postanalytics first. See details in [Separate postanalytics installation](installation-postanalytics-en.md).

To install as a dynamic module for NGINX, you must:

1. Install NGINX.
2. Add the Wallarm repositories, from which you will download packages.
3. Install the Wallarm packages.
4. Configure postanalytics.
5. Connect the Wallarm module.
6. Set up the filter node for using a proxy server.
7. Connect the filter node to the Wallarm cloud.
8. Configure the server addresses of postanalytics.
9. Configure the filtration mode.
10. Configure logging.
11. Restart NGINX.

--8<-- "en/include-en/elevated-priveleges.md"
    
## 1. Install NGINX

You can:

* Use [the official build](https://nginx.org/en/linux_packages.html).
* Prepare a custom build with the similar compilation options.

[See the official NGINX installation instructions](https://www.nginx.com/resources/admin-guide/installing-nginx-open-source/).

!!! info "Installing on Amazon Linux 2"
    To install NGINX on Amazon Linux 2, use the CentOS 7 instruction.

## 2. Add the Wallarm Repositories

The installation and updating of the filter node is done from the Wallarm
repositories.

Depending on your operating system, run one of the commands:

--8<-- "en/include-en/add-repo-en.md"

--8<-- "en/include-en/access-repo-en.md"

## 3. Install the Wallarm Packages

### Install the Requests Processing and Postanalytics on the Same Server

To run postanalytics and process the requests on the same server, you need to
install the following packages:

* Wallarm module
* In-memory storage Tarantool.
* Postanalytics.

Run the following command to install the required packages:

--8<-- "en/include-en/install-nginx-postanalytics-en.md"

### Install Only the Requests Processing on the Server

To only process the requests on the server, you need to install the following
package:

* Wallarm module

Run the following command to install the required package:

--8<-- "en/include-en/install-nginx-en.md"

<!-- -->

## 4. Configure Postanalytics

    #### Info::
    Skip this step if you installed postanalytics on a separate server as you already have your postanalytics configured.

Postanalytics uses the in-memory storage Tarantool.

You must set the amount of server RAM allocated to Tarantool.

--8<-- "en/include-en/configure-postanalytics-en.md"

<!-- -->

## 5. Connect the Wallarm Module

Open the `/etc/nginx/nginx.conf` file.

Ensure that you have the `include /etc/nginx/conf.d/*` line in the file. If you do not, add it.

Add the following directive right after the `worker_processes` directive:

```
load_module modules/ngx_http_wallarm_module.so;
```

Configuration example with the added directive:

```
user  nginx;
worker_processes  auto;
load_module modules/ngx_http_wallarm_module.so;

error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;
```

Copy the configuration files for the system setup:

```term
$ cp /usr/share/doc/nginx-module-wallarm/examples/*.conf /etc/nginx/conf.d/
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

--8<-- "en/include-en/filter-node-defaults.md"

<!-- -->

--8<-- "en/include-en/installation-extra-steps.md"