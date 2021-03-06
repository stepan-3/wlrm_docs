[doc-nginx-install]:    installation-nginx-distr-en.md
[doc-nginx-install]:        installation-nginx-distr-en.md
[doc-eu-scanner-ip-addresses]: scanner-address-en.md
[doc-us-scanner-ip-addresses]: scanner-address-us-en.md

# Configuration Options for the NGINX-Based Filter Node

!!! info "NGINX official documentation"
    The Wallarm configuration is very similar to the NGINX configuration. [See the official NGINX documentation](https://www.nginx.com/resources/admin-guide/). Along with the Wallarm specific configuration options, you have the full capabilities of NGINX configuration.

## Wallarm Directives

### wallarm_acl

Allows you to restrict access to resources when a request IP address is in the specified ACL (Access Control List). 

The specified ACL must be declared with the directive `wallarm_acl_db`.

You can use the `satisfy` directive to set constraints from both the ACL and other NGINX modules, such as ngx_http_access_module.

Setting directive to **off** disables the ACL check.

**Example:**

```
satisfy any;

wallarm_acl wapi;

allow 1.2.3.4/0;
deny all;
```

    #### Info::
>This parameter can be set inside the http, server and/or location blocks.

### wallarm_acl_api

If this directive is applied within the location block, than this location can be used to manage ACL content.

**Example:**
```
location / wallarm-acl {
  allow 127.0.0.1;
  deny all;

  wallarm_acl wapi;
  wallarm_acl_api on;
}
```

    #### Info::
>This parameter can be set inside the http, server and/or location blocks.

### wallarm_acl_db

Allows you to declare and configure an ACL database to restrict access by IP addresses.

**Example**

```
wallarm_acl_db wapi {
  wallarm_acl_path /var/cache/nginx / wallarm/acl/wapi;
}
```


    #### Info::
    The parameter can only be configured at the main level.


### wallarm_acl_mapsize

Allows you to set the initial memory size to be allocated for the corresponding ACL.

When the limit is reached, the memory will be automatically reallocated, but the API request that attempted to change the ACL and caused the overflow, will produce an error and should be repeated.

    #### Info::
    The parameter can only be configured inside the `wallarm_acl_db` block.


### wallarm_acl_path

Specifies the directory that will be used to save the state of the ACL.

    #### Info::
    The parameter can only be configured inside the `wallarm_acl_db` block.

### wallarm_acl_block_page

Lets you set up the response code and the page returned to the client when the request was sent from the [blocked](../user-guides/cloud-ui/blacklist/blacklist.md) IP address.

The directive can take any value including:
* the path to the blocking page file on the server,
* the name of `location`,
* the path to the file with the blocking page template on the machine that runs NGINX,
* error code.

**To return a particular page in the response to the blocked request:**
1. Create the blocking page and save this file on the server. For example, the `blacklist_block.html` file.
2. Add the `wallarm_acl_block_page` directive and specify the path to the blocking page file:

    ```
    wallarm_acl_block_page /path/blacklist_block.html
    ```
    The client with the blacklist IP address will be redirected to the `blacklist_block.html` page.

**To apply an existing `location` configuration to the blocked request:**
1. Enter the blocking message into the `location` block. For example:
    
    ```
    location @block {'The page is blocked';}
    ```
2. Add the `wallarm_acl_block_page` directive and specify the `location` block name:
   
    ```
    wallarm_acl_block_page @block;
    location @block {'The page is blocked';}
    ```

    The request from the blacklist IP address will be blocked with the `The page is blocked` message.

**To return a specific error code along with the blocking page in the response to the blocked request:**
1. Create the blocking page and save this file on the server. For example, the `445.htm` file.
2. Add the `location` block with the blocking page settings:

    ```
    location @blocked {
        root /var/www/errors;           # The directory with the 445.htm file
        rewrite ^(.*)$ /445.htm break;  # The redirect to the /445.htm
    }
    ```
3. Add the `error_page` object and specify the error code and the name of the blocking page `location`:

    ```
    error_page 445 @blocked;
    ```
4. Add the `location` block with the error code settings:

    ```
    location = /err445 {
        internal;                   # The internal location that is not available from the outside
        return 445;                 # The 445 response code is returned
    }
    ```
5. Add the `wallarm_acl_block_page` directive and specify the name of the error code `location`:

    ```
    wallarm_acl_block_page /err445;
    ```

    After all settings are applied, the configuration file will look like follows:

    ```
    wallarm_block_page /err445;     # /err445 – location to redirect the request to
    error_page 445 @blocked;        # The request that triggered the 445 error is passed to the @blocked location 
    location @blocked {
        root /var/www/errors;           # The directory with the 445.htm file
        rewrite ^(.*)$ /445.htm break;  # The redirect to the /445.htm
    }
    location = /err445 {
        internal;                   # The internal location that is not available from the outside
        return 445;                 # The 445 response code is returned
        }
    ```

    The client with the blacklist IP address will be redirected to the `445.htm` page and will receive the `445` error code in the response to the request.

**To return the dynamic blocking page in the response to the blocked request:**
1. Create a template for the dynamic blocking page and save this file on the machine that runs NGINX, for example, the `blacklist_block.html` page. You can also use the template provided by Wallarm which is named `wallarm_blocked.html`.

!!! info "Including NGINX variables in the blocking page template"
    You can include NGINX variables in your blocking page template to display a dynamic value by specifying the name of the NGINX variable starting with the `$` symbol.

2. Add the `wallarm_acl_block_page` directive and specify the path to the template on the machine starting with the `&` symbol:

    ```
    wallarm_acl_block_page &C:/templates/blacklist_block.html
    ```

    > #### Warning:: Important Information for Debian and CentOS Users
    > If you use an NGINX version lower than 1.11 installed from [CentOS/Debian][doc-nginx-install] repositories, you should remove the `request_id` variable from the page code in order to display the dynamic blocking page correctly:
    > ```
    > UUID ${request_id}  # This part of the code should be removed
    > ```
    > This applies to both `wallarm_blocked.html` and to your own dynamic page template.

    #### Info::
    This parameter can be set inside the http, server and location blocks.

### wallarm_api_conf

A path to the `node.yaml` file, which contains access requirements for the Wallarm API.

**Example**: 
```
wallarm_api_conf /etc/wallarm/node.yaml
```

Used to upload serialized requests from the filtering node directly to the Wallarm API (cloud) instead of uploading into the postanalytics module (Tarantool).
**Only requests with attacks are sent to the API.** Requests without attacks are not saved.

**Example of the node.yaml file content:**
```
# API connection credentials

hostname: <some name>
uuid: <some uuid>
secret: <some secret>

# API connection parameters (the parameters below are used by default)

api:
  host: api.wallarm.com
  port: 444
  use_ssl: true
  ca_verify: true
```


### wallarm_block_page

Lets you set up the response code and page returned to the client when an [invalid request](../glossary-en.md#invalid-request) has been blocked. 

The directive can take any value including:
* the path to the blocking page file on the server,
* the name of `location`,
* the path to the file with the blocking page template on the machine that runs NGINX,
* error code.

**To return a particular page in the response to the blocked request:**
1. Create the blocking page and save this file on the server. For example, the `block.html` file.
2. Add the `wallarm_block_page` directive and specify the path to the blocking page file:

    ```
    wallarm_block_page /path/block.html
    ```
    The client sent the request will be redirected to the `block.html` page.

**To apply an existing `location` configuration to the blocked request:**
1. Enter the blocking message into the `location` block. For example:
    
    ```
    location @block {'The page is blocked';}
    ```
2. Add the `wallarm_block_page` directive and specify the `location` block name:
   
    ```
    wallarm_block_page @block;
    location @block {'The page is blocked';}
    ```

    The request will be blocked with the `The page is blocked` message.

**To return a specific error code along with the blocking page in the response to the blocked request:**
1. Create the blocking page and save this file on the server. For example, the `445.htm` file.
2. Add the `location` block with the blocking page settings:

    ```
    location @blocked {
        root /var/www/errors;           # The directory with the 445.htm file
        rewrite ^(.*)$ /445.htm break;  # The redirect to the /445.htm
    }
    ```
3. Add the `error_page` object and specify the error code and the name of the blocking page `location`:

    ```
    error_page 445 @blocked;
    ```
4. Add the `location` block with the error code settings:

    ```
    location = /err445 {
        internal;                   # The internal location that is not available from the outside
        return 445;                 # The 445 response code is returned
    }
    ```
5. Add the `wallarm_block_page` directive and specify the name of the error code `location`:

    ```
    wallarm_block_page /err445;
    ```

    After all settings are applied, the configuration file will look like follows:

    ```
    wallarm_block_page /err445;     # /err445 – location to redirect the request to
    error_page 445 @blocked;        # The request that triggered the 445 error is passed to the @blocked location 
    location @blocked {
        root /var/www/errors;           # The directory with the 445.htm file
        rewrite ^(.*)$ /445.htm break;  # The redirect to the /445.htm
    }
    location = /err445 {
        internal;                   # The internal location that is not available from the outside
        return 445;                 # The 445 response code is returned
        }
    ```

    The client sent the request will be redirected to the `445.htm` page and will receive the `445` error code in the response to the request.

**To return the dynamic blocking page in the response to the blocked request:**
1. Create a template for the dynamic blocking page and save this file on the machine that runs NGINX, for example, the `block.html` page. You can also use the template provided by Wallarm which is named `wallarm_blocked.html`.

!!! info "Including NGINX variables in the blocking page template"
    You can include NGINX variables in your blocking page template to display a dynamic value by specifying the name of the NGINX variable starting with the `$` symbol.

2. Add the `wallarm_block_page` directive and specify the path to the template on the machine starting with the `&` symbol:

    ```
    wallarm_block_page &C:/templates/block.html
    ```

    > #### Warning:: Important Information for Debian and CentOS Users
    > If you use an NGINX version lower than 1.11 installed from [CentOS/Debian][doc-nginx-install] repositories, you should remove the `request_id` variable from the page code in order to display the dynamic blocking page correctly:
    > ```
    > UUID ${request_id}  # This part of the code should be removed
    > ```
    > This applies to both `wallarm_blocked.html` and to your own dynamic page template.

    #### Info::
    This parameter can be set inside the http, server and location blocks.

### wallarm_cache_path

A directory in which the backup catalog for the proton.db and [LOM](../glossary-en.md#lom) copy storage is created when the server starts. This directory must be writable for the client that runs NGINX.

    #### Info::
    This parameter is configured inside the http block only.


### wallarm_fallback

With the value set to **on**, NGINX has the ability to enter an emergency mode: if proton.db or LOM cannot be downloaded, this setting disables the Wallarm module for the http, server, and location blocks, for which the data fails to download. NGINX keeps functioning.

    #### Info::
    This parameter can be set inside the http, server and location blocks.


### wallarm_force

Sets the requests analysis and LOM rules generation based on the NGINX mirrored traffic. See [Analyzing mirrored traffic with NGINX](mirror-traffic-en.md).

### wallarm_global_trainingset_path

A path to the proton.db file that has the global settings for requests filtering, which do not depend on the application structure.

    #### Info::
    This parameter can be set inside the http, server and location blocks.
>
    **Default value**: `/etc/wallarm/proton.db`

### wallarm_file_check_interval

Defines an interval between checking new data in proton.db and [LOM](../glossary-en.md#lom). The unit of measure is specified in the suffix as follows:
* not specified for minutes,
* `s` for seconds,
* `ms` for milliseconds.

    #### Info::
    This parameter is configured inside the http block only.
>
    **Default value**: `1` (1 minute)

### wallarm_instance

An application identifier. The directive is used to visually separate the data of different applications on *Dashboard*. Numeric values only.

The application identifiers are used solely for the convenience of data presentation. To correctly separate the data of different applications, the same application identifiers must be set in the Wallarm interface.

Any filter node will filter traffic for any number of applications without additional configuration.

    #### Info::
    This parameter can be set inside the http, server and location blocks.


### wallarm_key_path

A path to the Wallarm license key.

    #### Info::
    **Default value**: `/etc/wallarm/license.key`


### wallarm_local_trainingset_path

A path to the [LOM](../glossary-en.md#lom) file that contains information on the protected application and the filter node settings.

    #### Info::
    This parameter can be set inside the http, server and location blocks.
>
    **Default value**: `/etc/wallarm/lom`


### wallarm_mode

Traffic processing mode:

- **off**: requests are not processed.
- **monitoring**: all requests are processed, but none of them is blocked even if an attack is detected.
- **block**: all requests where an attack was detected are blocked.
- **aggressive**: all non-standard requests are blocked. For example, mapping a string in the field usually used for passing a number. Use this mode with extreme caution.

The value can include variables that are available after receiving a request string and headers. This can be used for applying various policies for various clients:

```
map $remote_addr $wallarm_mode_real {
    default block;
    1.1.1.1/24 monitoring;
    2.2.2.2 off;
}
...

wallarm_mode $wallarm_mode_real;
```

You can see the detailed example of filtration mode configuration by proceeding to the [link](block-part-en.md).

    #### Info::
    This parameter can be set inside the http, server and location blocks.
>
    **Default value**: `off`

--8<-- "en/include-en/scanner-whitelist-warning.md"

Usage of `wallarm_mode` can be restricted by the `wallarm_mode_allow_override` directive.


### wallarm_mode_allow_override

Manages the ability to override the `wallarm_mode` values via filtering rules downloaded from the Wallarm cloud (LOM):

- **off**: the rules set in LOM are ignored.
- **strict**: LOM can only strengthen the operation mode.
- **on**: it is possible to both strengthen and soften the operation mode.

For example, with `wallarm_mode monitoring` and `wallarm_mode_allow_override strict` set, the Wallarm cloud can be used to enable blocking of some requests, but the attack analysis cannot be fully disabled.

    #### Info::
    This parameter can be set inside the http, server and location blocks.
>
    **Default value**: `on`


### wallarm_parse_response

The mode of processing web server responses; by default, only the client's request to the web server can be processed.

Possible values:

- **on**: analysis of web server responses by a passive [vulnerability](../glossary-en.md#vulnerability) scanner without sending requests from the Wallarm cloud).
- **off**: responses are not analyzed.

    #### Info::
    This parameter can be set inside http, server and location blocks.
>
    **Default value**: `on`

<br>

    #### Warning:: Improve performance
    You are recommended to disable processing of static files through `location` to improve performance.

<br>

### wallarm_parse_websocket

Wallarm has full WebSockets support. By default, the WebSockets messages are not analyzed for attacks. To force the feature, use the `wallarm_parse_websocket` directive.

Possible values:
- **on**: message analyses is enabled
- **off**: message analyses is disabled.

    #### Info::
    This parameter can be set inside the http, server and location blocks.
>
    **Default value**: `off`

### wallarm_parser_disable

Allows to disable parsers.

The following parsers are currently supported:

- cookie
- zlib
- htmljs
- json
- multipart
- base64
- percent
- urlenc
- xml

**Example**

```
wallarm_parser_disable base64;
wallarm_parser_disable xml;
location /ab {
    wallarm_parser_disable json;
    wallarm_parser_disable base64;
    proxy_pass http://example.com;
}
location /zy {
    wallarm_parser_disable json;
    proxy_pass http://example.com;
}
```

    #### Info::
    This parameter can be set inside the http, server and location blocks.

### wallarm_parse_html_response

Lets you enable and disable an HTML parser for responses to requests. Can be:
* `on`
* `off`

    #### Info::
    This parameter can be set inside the http, server and location blocks.
>
    **Default value**: `on`

### wallarm_stalled_worker_timeout

Sets the time limit of a single request processing for NGINX worker in seconds.

If the time exceeds the limit, data about NGINX workers is written to the `stalled_workers_count` and `stalled_workers` [statistic](configure-statistics-service.md##working-with-the-statistics-service) parameters.

    #### Info::
    This parameter can be set inside the http, server and location blocks.
>
    **Default value**: `5` (5 seconds)

### wallarm_process_time_limit

Sets the time limit of a single request processing in milliseconds. If the time exceeds the limit, an error is recorded into the log and the request is marked as an `overlimit_res` attack. The requests are blocked in the *blocking* mode (`wallarm_mode block;`) and ignored in the *monitoring* mode (`wallarm_mode monitoring;`).

Default value is 1000ms (one second).

    #### Info::
    This parameter can be set inside the http, server and location blocks.

### wallarm_process_time_limit_block

The ability to manage the blocking of requests, which exceed the time limit set in the `wallarm_process_time_limit` directive:

- **on**: the requests are always blocked
- **off**: the requests are always ignored
- **attack**: depends on the attack blocking mode set in the `wallarm-mode` directive:

  - **monitoring**: the requests are ignored
  - **block** and **aggressive**: the requests are blocked

    #### Info::
    This parameter can be set inside the http, server and location blocks.
    **Default value**: `wallarm_process_time_limit_block attack`

### wallarm_request_memory_limit

Set a limit for the maximum amount of memory that can be used for processing of a single request.

If the limit is exceeded the request processing will be interrupted and a user will get 500 error.

The following suffixes can be used in this parameter:
* `k` or `K` for kilobytes
* `m` or `M` for megabytes
* `g` or `G` for gigabytes

Value of 0 turns the limit off.

By default, limits are off. 

    #### Info::
>This parameter can be set inside the http, server and/or location blocks.

### wallarm_proton_log_mask_master

Settings of the debug logging for the master process NGINX. 

      #### Warning:: Using the directive
>
      You need to configure the directive only if you are told to do so by the Wallarm support team member. They will provide you with the value to use with the directive.

<!-- -->

    #### Info::
    The parameter can only be configured at the main level.


### wallarm_proton_log_mask_worker

Settings of the debug logging for a worker process NGINX. 

        #### Warning:: Using the directive
>
        You need to configure the directive only if you are told to do so by the Wallarm support team member. They will provide you with the value to use with the directive.

<!-- -->

      #### Info::
    The parameter can only be configured at the main level.

### wallarm_request_chunk_size

Limits the size of the part of the request that is processed during one iteration. You can set up a custom value of the `wallarm_request_chunk_size` directive in bytes by assigning an integer to it. The directive also supports the following postfixes:
* `k` or `K` for kilobytes
* `m` or `M` for megabytes
* `g` or `G` for gigabytes

    #### Info::
    This parameter can be set inside the http, server and location blocks.
>
    **Default value**: `8k` (8 kilobytes).


### wallarm_set_tag

Defines the key value pair for label each request. A value can contain variables.

Specified tags will be available in postanalytics.

Usage:
```
wallarm_set_tag somename $var;
```

    #### Info::
    This parameter can be set inside the server and/or location blocks.


### wallarm_tarantool_connect_attempts

      #### Warning:: Deprecated
    
      Use the [`wallarm_upstream_connect_attempts`](#wallarmupstreamconnectattempts) directive instead.


### wallarm_tarantool_connect_interval

      #### Warning:: Deprecated
    
      Use the [`wallarm_upstream_reconnect_interval`](#wallarmupstreamreconnectinterval) directive instead.


### wallarm_tarantool_upstream

With the `wallarm_tarantool_upstream` you can balance the requests between several postanalytics servers.

**Example:**

```
     upstream wallarm_tarantool {
        server 127.0.0.1:3313 max_fails=0 fail_timeout=0 max_conns=1;
        keepalive 1;
    }

    ...

    wallarm_tarantool_upstream wallarm_tarantool;
```

See also [Module ngx_http_upstream_module](https://nginx.org/en/docs/http/ngx_http_upstream_module.html).

    #### Warning:: Required conditions
    It is required that the following conditions are satisfied for the `max_conns` and the `keepalive` parameters:
    * The value of the `keepalive` parameter must not be lower than the number of the tarantool servers.
    * The value of the `max_conns` parameter must be specified for each of the upstream Tarantool servers to prevent the creation of excessive connections.

<!-- -->

    #### Info::
    The parameter is configured inside the http block only.

### wallarm_timeslice

A limit on the time that a filter node spends on one iteration of processing a request before it switches to the next request. Upon reaching the time limit, the filter node proceeds to process the next request in the queue. After performing one iteration on each of the requests in the queue, the node performs the second iteration of processing on the first request in the queue.

You can use time intervals suffixes that are described in the [nginx documentation](https://nginx.org/en/docs/syntax.html) to assign different time unit values to the directive.

      #### Info::
    This parameter can be set inside the http, server and location blocks.
>
    **Default value**: `0` (time limit for single iteration is disabled).

-----

    #### Warning:: 
    Due to nginx server limitations, it is necessary to disable the request buffering by assigning the `off` value to the `proxy_request_buffering` nginx directive for the `wallarm_timeslice` directive to work.

### wallarm_ts_request_memory_limit

Set a limit for the maximum amount of memory that can be used by one instance of proton.db and LOM.

If the memory limit is exceeded while processing of some request, user will get 500 error.

The following suffixes can be used in this parameter:
* `k` or `K` for kilobytes
* `m` or `M` for megabytes
* `g` or `G` for gigabyte

Value of **0** turns the limit off.

      #### Info::
    This parameter can be set inside the http, server and/or location blocks.
>
    **Default value**: `1` GB

### wallarm_unpack_response

If the backend sends compressed data, the value `on` decompresses the data before processing. The value `off` turns off the decompressing.

    #### Info::
    **Default value**: `on`.


### wallarm_upstream_backend

A method for sending serialized requests. Requests can be sent either to the Tarantool or to the API.

Possible values of the directive:
*   `tarantool`
*   `api`

Depending on the other directives, the default value will be assigned as follows:
*   `tarantool`—if there is no `wallarm_api_conf` directive in the configuration.
*   `api`—if there is a `wallarm_api_conf` directive, but there is no `wallarm_tarantool_upstream` directive in the configuration.

    > #### Warning:: Note
    > If the `wallarm_api_conf` and `wallarm_tarantool_upstream` directives are present simultaneously in the configuration, a configuration error of the *directive ambiguous wallarm upstream backend* form will occur.

    #### Info::
      This parameter can be set inside the http block only.


### wallarm_upstream_connect_attempts

Defines the number of immediate reconnects to the Tarantool or Wallarm API.
If a connection to the Tarantool or API is terminated, then the attempt to reconnect will not occur, except when there are no more connections and the serialized request queue is not empty.

!!! info "Note"
    Reconnection may occur through another server because the “upstream” subsystem is responsible for choosing the server.
    This parameter can be set inside the http block only.

### wallarm_upstream_reconnect_interval

Defines the interval between attempts to reconnect to the Tarantool or Wallarm API after the number of unsuccessful attempts has exceeded the `wallarm_upstream_connect_attempts` threshold.

    #### Info::
    This parameter can be set inside the http block only.


### wallarm_upstream_connect_timeout

Defines a timeout for connecting to the Tarantool or Wallarm API.

    #### Info::
    This parameter can be set inside the http block only.


### wallarm_upstream_queue_limit

Defines a limit to the number of serialized requests.
Simultaneously setting the `wallarm_upstream_queue_limit` parameter and not setting the `wallarm_upstream_queue_memory_limit` parameter means that there will be no limit on the latter.

    #### Info::
      This parameter can be set inside the http block only.


### wallarm_upstream_queue_memory_limit

Defines a limit to the total volume of serialized requests.
Simultaneously setting the `wallarm_upstream_queue_memory_limit` parameter and not setting the `wallarm_upstream_queue_limit` parameter means that there will be no limit on the latter.

      #### Info::
      **Default value:** `100m`.
>
    This parameter can be set inside the http block only.


### wallarm_worker_rlimit_vmem

      #### Warning:: Deprecated
>
      It is now an alias for [wallarm_ts_request_memory_limit](#wallarmtsrequestmemorylimit) directive.
