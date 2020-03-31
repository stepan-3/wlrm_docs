# Installing on Linux

    #### Warning:: INSTRUCTIONS DEPRECATED
    These instructions are for the installation of legacy and unsupported packages.
    
    For the initial product installation, follow the [instruction for installing as a dynamic module for NGINX](installation-nginx-en.md).

## Installation Options

--8<-- "en/include-en/installation-options-en.md"

To install the filter node, you must:

1. Add the Wallarm repositories, from which you will download packages.
2. Install the Wallarm packages.
3. Configure postanalytics.
4. Set up the filter node for using a proxy server.
5. Connect the filter node to the Wallarm cloud.
6. Configure the server addresses of postanalytics.
7. Configure the filtration mode.
8. Restart the Wallarm service.

----------

--8<-- "en/include-en/elevated-priveleges.md"

<!-- -->     

## 1. Add the Wallarm Repositories

The installation and updating of the filter node is done from the Wallarm
repositories.

Depending on your operating system, run one of the commands:

--8<-- "en/include-en/add-repo-legacy-en.md"

--8<-- "en/include-en/access-repo-en.md"

<br>

## 2. Install the Wallarm Packages

!!! info "Note"
    
    If these packages are installed, a monolithic version of the filter node will result. 
    The Wallarm filter module will be integrated with and inseparable from NGINX.

To install the filter node and postanalytics on the same server, run the command:

--8<-- "en/include-en/install-package-en.md"

## 3. Configure Postanalytics

    #### Info::
    Skip this step if you installed postanalytics on a separate server as you already have your postanalytics configured.

Postanalytics uses the in-memory storage Tarantool.

You must set the amount of server RAM allocated to Tarantool.

--8<-- "en/include-en/configure-postanalytics-legacy-en.md"

## 4. Set up the Filter Node for Using a Proxy Server

--8<-- "en/include-en/setup-proxy.md"

## 5. Connect the Filter Node to the Wallarm Cloud

--8<-- "en/include-en/connect-cloud-en.md"

## 6. Configure the Server Addresses of Postanalytics

    #### Info::
        * Skip this step if you installed postanalytics and the filter node on the same server.
      * Do this step if you installed postanalytics and the filter node on separate servers.

--8<-- "en/include-en/configure-postanalytics-address-en.md"

<br>

## 7. Configure the Filtration Mode

--8<-- "en/include-en/setup-filter-en.md"


## 8. Restart the Wallarm Service

--8<-- "en/include-en/restart-nginx-wallarm-legacy-en.md"


<!-- -->

    
----------
    

## The Installation Is Complete

--8<-- "en/include-en/check-setup-installation-en.md"
