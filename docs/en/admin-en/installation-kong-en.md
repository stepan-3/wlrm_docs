[kong-install]:         https://konghq.com/install/
[kong-docs]:            https://getkong.org/docs/
[kong-admin-api]:       https://getkong.org/docs/0.10.x/admin-api/

[doc-wallarmblockpage]: ../admin-en/configure-parameters-en.html#wallarmblockpage
[doc-postanalytics]:    installation-postanalytics-en.md
[doc-supported-os]:     supported-platforms.md

# Installing with Kong

!!! info "Prerequisites"
>
    The Kong platform shall meet the following requirements:
    * Kong version 1.4.3 or lower
    * Kong installed on the platform [supported by Wallarm][doc-supported-os] according to Kong [official instructions][kong-install]
    
    One of the following points is required for proper Kong operation:
    * prepared configuration files,
    * configured database.
    
    Please make sure that installed Kong meets the prerequisites before proceeding with Wallarm installation.
>
    The official Kong documentation is available by the [link][kong-docs].

<!-- -->

    #### Warning:: Known Limitations
    
      * The [`wallarm_block_page`][doc-wallarmblockpage] directive is not supported.
    * Wallarm configuration via [Kong Admin API][kong-admin-api] is not supported.

## Installation

    #### Warning:: Installation of postanalytics on a separate server
    If you are planning to install postanalytics on a separate server,
    you must install postanalytics first. See details in [Separate postanalytics installation][doc-postanalytics].

To install the Wallarm module with Kong, you need to:

1. Add Wallarm repositories.
2. Install Wallarm packages.
3. Configure postanalytics.
4. Set up the filter node for using a proxy server.
5. Connect the filter node to the Wallarm cloud.
6. Configure the postanalytics server addresses.
7. Configure the filtration mode.
8. Configure logging.
    
<!-- -->

--8<-- "en/include-en/elevated-priveleges.md"

<!-- -->     

## 1. Add Wallarm Repositories

The filter node installs and updates from the Wallarm repositories.

Depending on your operating system, run one of the following commands:

--8<-- "en/include-en/add-repo-kong-en.md"

<!-- -->

--8<-- "en/include-en/access-repo-en.md"

<!-- -->

## 2. Install Wallarm Packages

To install the filter node and postanalytics on the same server, run the command:

--8<-- "en/include-en/install-package-kong-en.md"

To install the filter node alone, run the command:

--8<-- "en/include-en/install-package-primary-kong-en.md"

<!-- -->

## 3. Configure Postanalytics 

    #### Info::
    Skip this step if you installed postanalytics on a separate server as you already have your postanalytics configured.

<!-- -->

--8<-- "en/include-en/configure-postanalytics-kong-en.md"

<!-- -->

## 4. Set up the Filter Node for Using a Proxy Server

--8<-- "en/include-en/setup-proxy.md"

<!-- -->

## 5. Connect the Filter Node to the Wallarm Cloud

--8<-- "en/include-en/connect-cloud-en.md"

<!-- -->

## 6. Configure the Postanalytics Server Addresses

      #### Info::
      * Skip this step if you installed postanalytics and the filter node on the same server.
      * Do this step if you installed postanalytics and the filter node on separate servers.

<!-- -->

--8<-- "en/include-en/configure-postanalytics-address-kong-en.md"

<!-- -->

## 7. Set up the Filtration Mode

--8<-- "en/include-en/setup-filter-kong-en.md"

<!-- -->

## 8. Configure Logging

<!-- -->
--8<-- "en/include-en/installation-step-logging.md"
<!-- -->

## Start Kong

To start Kong with the installed Wallarm module, run the command:

```term
# kong start --nginx-conf /etc/kong/nginx-wallarm.template
```
    
<!-- -->

## The Installation Is Complete

--8<-- "en/include-en/check-setup-installation-en.md"

<!-- -->

--8<-- "en/include-en/filter-node-defaults.md"

<!-- -->

--8<-- "en/include-en/installation-extra-steps.md"