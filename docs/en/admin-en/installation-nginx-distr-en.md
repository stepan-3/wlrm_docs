# Installing as a Dynamic Module with NGINX from Debian/CentOS Repositories

You can install NGINX from the Debian/CentOS repositories.

To install NGINX from the repositories, you must:

1. Add the Debian/CentOS repositories.
2. Install NGINX with the Wallarm module.
3. Configure postanalytics.
4. Connect the Wallarm module.
5. Set up the filter node for using a proxy server.
6. Connect the filter node to the Wallarm cloud.
7. Configure the server addresses of postanalytics.
8. Configure the filtration mode.
9. Configure logging
10. Restart NGINX.

<!-- -->

--8<-- "en/include-en/elevated-priveleges.md"

<!-- -->     

## 1. Add the Repositories

Depending on your operating system, run one of the commands:

--8<-- "en/include-en/add-repo-distr-en.md"

<!-- -->

--8<-- "en/include-en/access-repo-en.md"

<!-- -->

## 2. Install NGINX with the Wallarm Module

      #### Warning:: Important information for Debian 8 “Jessie” users
      
      Note that using the NGINX installed from the `jessie` repository will result in non-functioning Wallarm module for NGINX.
      
      You need to add the `jessie-backports` backports repository and install NGINX from this repository.
      
      If you follow the instructions from the previous step to add the repositories to your system, then the backports repository is already set up. You can execute the command for Debian 8.x (see below) to get all necessary components installed.

<!-- -->

### Install the Requests Processing and Postanalytics on the Same Server

To run postanalytics and process the requests on the same server, you need to
install the following packages:

* Wallarm module
* In-memory storage Tarantool.
* Postanalytics.

Run the following command to install the required packages:

--8<-- "en/include-en/install-nginx-postanalytics-distr-en.md"

<!-- -->

### Install Only the Requests Processing on the Server

To only process the requests on the server, you need to install the following
package:

* Wallarm module

Run the following command to install the required package:

--8<-- "en/include-en/install-nginx-distr-en.md"

<!-- -->

## 3. Configure Postanalytics

Postanalytics uses the in-memory storage Tarantool.

You must set the amount of server RAM allocated to Tarantool.

--8<-- "en/include-en/configure-postanalytics-distr-en.md"

<!-- -->

## 4. Connect the Wallarm Module

Copy the configuration files for the system setup:

{% termtabs name="Debian" -%}
$ cp /usr/share/doc/libnginx-mod-http-wallarm/examples/*conf /etc/nginx/conf.d/
{%- tab name="CentOS" -%}
$ cp /usr/share/doc/nginx-mod-http-wallarm/examples/*conf /etc/nginx/conf.d/
{%- endtermtabs %}

<!-- -->

## 5. Set up the Filter Node for Using a Proxy Server

--8<-- "en/include-en/setup-proxy.md"

<!-- -->

## 6. Connect the Filter Node to the Wallarm Cloud

--8<-- "en/include-en/connect-cloud-en.md"

<!-- -->

## 7. Configure the Server Addresses of Postanalytics

    #### Info::
    * Skip this step if you installed postanalytics and the filter node on the same server.
      * Do this step if you installed postanalytics and the filter node on separate servers.

<!-- -->

--8<-- "en/include-en/configure-postanalytics-address-nginx-en.md"

<!-- -->

## 8. Configure the Filtration Mode

--8<-- "en/include-en/setup-filter-nginx-en.md"

<!-- -->

## 9. Configure Logging

<!-- -->
--8<-- "en/include-en/installation-step-logging.md"
<!-- -->

## 10. Restart NGINX

--8<-- "en/include-en/root_perm_info.md"

<!-- -->

<!-- -->

--8<-- "en/include-en/restart-nginx-distr-en.md"

<!-- -->

## The Installation Is Complete

--8<-- "en/include-en/check-setup-installation-en.md"

<!-- -->

--8<-- "en/include-en/filter-node-defaults.md"

<!-- -->

--8<-- "en/include-en/installation-extra-steps.md"