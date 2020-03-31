# Installing the Filter Node

!!! info "Request processing"
    Request processing by a filter node consists of the following phases:
    * Initial processing by the NGINX-Module-Wallarm
    * Postanalytics and the statistical analysis of the processed requests.

This instruction describes the installation of the Wallarm filter node as a dynamic module for NGINX on the same server with postanalytics.

To install the filter node, do the following:

1. Install NGINX.
2. Add the Wallarm repositories, from which you will download packages.
3. Install the Wallarm packages.
4. Configure postanalytics.
5. Connect the Wallarm module.
6. Connect the filter node to the Wallarm cloud.

--8<-- "en/include-en/elevated-priveleges.md"

<!-- -->   

## 1. Install NGINX

Install NGINX from the official NGINX repositoriy by following the instruction that corresponds with your operating system from the list below.

*   [Ubuntu](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/#installing-a-prebuilt-ubuntu-package-from-the-official-nginx-repository)
*   [Debian](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/#installing-a-prebuilt-debian-package-from-the-official-nginx-repository)
*   [CentOS](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/#installing-a-prebuilt-centos-rhel-package-from-the-official-nginx-repository)
*   [Amazon Linux 2](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/#installing-a-prebuilt-centos-rhel-package-from-the-official-nginx-repository): use the CentOS 7 instruction 

        #### Warning:: Stable NGINX version
    Make sure you are installing the *stable* version of NGINX. The `mainline` part of the path must be omitted from the NGINX repository link.

## 2. Add the Wallarm Repositories

The filter node is installed and updated from the Wallarm repositories.

Depending on your operating system, run one of the commands:

--8<-- "en/include-en/add-repo-en.md"

--8<-- "en/include-en/access-repo-en.md"

<br>

## 3. Install the Wallarm Packages

Depending on your operating system, run one of the commands:

--8<-- "en/include-en/install-nginx-postanalytics-en.md"

## 4. Configure Postanalytics

Postanalytics uses the Tarantool in-memory storage.

You must set the amount of server RAM allocated to Tarantool.

--8<-- "en/include-en/configure-postanalytics-en.md"

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

<br>

## 6. Connect the Filter Node to the Wallarm Cloud

--8<-- "en/include-en/connect-cloud-en.md"

<br>

## Installation Completed

The installation is completed.

Now you need to configure the filter node to filter traffic. See [Configure the Proxying and Filtering Rules](qs-setup-proxy-en.md).
