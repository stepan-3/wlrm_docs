# Blocking with NGINX

!!! info "Supported version"
    This feature is supported starting Wallarm Node 2.8

By default, blocking by IP address is turned off. To activate it, proceed to the following steps:

1.  Go to the folder that contains the NGINX configuration files. To do this, select and run the command that corresponds with your filter node installation method from the tabs below.

    {% termtabs name="Dynamic NGINX module" -%}
$ cd /etc/nginx/conf.d
    {%- tab name="Dynamic NGINX module from OS repositories" -%}
$ cd /etc/nginx/conf.d
    {%- tab name="NGINX Plus module" -%}
$ cd /etc/nginx/conf.d
    {%- tab name="NGINX-Wallarm module (DEPRECATED)" -%}
$ cd /etc/nginx-wallarm/conf.d
    {%- endtermtabs %}

2.  In the current folder, create a file named `/etc/nginx/conf.d/wallarm-acl.conf` with the following content:

    ```
    wallarm_acl_db default {
        wallarm_acl_path <path-to-wallarm-acl-folder>;
        wallarm_acl_mapsize 64m;
    }
    
    server {
        listen 127.0.0.9:80;
    
        server_name localhost;
    
        allow 127.0.0.0/8;
        deny all;
    
        access_log off;
    
        location /wallarm-acl {
            wallarm_acl default;
            wallarm_acl_api on;
        }
    }
    ```
    
    In the configuration above, `<path-to-wallarm-acl-folder>` is a path to an empty directory, in which the `nginx` user can create subdirectories and files for storing access control lists.
    
    !!! info "Checking the `nginx` user rights"
    > Run the following command to check whether the `nginx` user has rights to perform actions on the directory:
    > ```
    > sudo -u nginx [ -r <path-to-directory> ] && [ -w <path-to-directory> ] && echo "ОК"
    > ```
    > In this command, `<path-to-directory>` is the path to the directory, access to which you need to check.
    >  
    > Due to the `sudo -u nginx` prefix, this command is executed using the `nginx` user.
    > 
    > The command first checks whether the user has permission to read the directory (the `[ -r <path-to-directory> ]` part). Next, it checks whether the user has permission to write into the directory (the `[ -w <path-to-directory> ]` part).   
    > 
    > If the `nginx` user has permission to read and write into the directory specified in the command, the terminal outputs the `OK` message. The specified directory can be used as a `wallarm_acl_path` value.
    > 
    > If the `nginx` user does not have the required permission, the terminal output is empty.
    
    **Example:**
    
    Directories that are valid for access control list storage depend on the filter node installation method you used and your OS. The valid directories to specify in the `wallarm_acl_path` directive are as follows:
    *   Dynamic NGINX module:
    
        {% termtabs name="Debian 8.x (jessie)" -%}
    /var/cache/nginx/wallarm_acl_default
        {%- tab name="Debian 9.x (stretch)" -%}
    /var/cache/nginx/wallarm_acl_default
        {%- tab name="Debian 10.x (buster)" -%}
    /var/cache/nginx/wallarm_acl_default
        {%- tab name="Ubuntu 14.04 LTS (trusty)" -%}
    /var/cache/nginx/wallarm_acl_default
        {%- tab name="Ubuntu 16.04 LTS (xenial)" -%}
    /var/cache/nginx/wallarm_acl_default
        {%- tab name="Ubuntu 18.04 LTS (bionic)" -%}
    /var/cache/nginx/wallarm_acl_default
        {%- tab name="CentOS 6.x" -%}
    /var/cache/nginx/wallarm_acl_default
        {%- tab name="CentOS 7.x" -%}
    /var/lib/nginx/wallarm_acl_default
        {%- tab name="Amazon Linux 2" -%}
    /var/lib/nginx/wallarm_acl_default
        {%- endtermtabs %}
    
    *   Dynamic NGINX module from OS repositories:
    
        {% termtabs name="Debian 8.x (jessie)" -%}
    /var/lib/nginx/wallarm_acl_default
        {%- tab name="Debian 9.x (stretch)" -%}
    /var/lib/nginx/wallarm_acl_default
        {%- tab name="Debian 10.x (buster)" -%}
    /var/lib/nginx/wallarm_acl_default
        {%- tab name="Ubuntu 14.04 LTS (trusty)" -%}
    /var/lib/nginx/wallarm_acl_default
        {%- tab name="Ubuntu 16.04 LTS (xenial)" -%}
    /var/lib/nginx/wallarm_acl_default
        {%- tab name="Ubuntu 18.04 LTS (bionic)" -%}
    /var/lib/nginx/wallarm_acl_default
        {%- tab name="CentOS 6.x" -%}
    /var/lib/nginx/wallarm_acl_default
        {%- tab name="CentOS 7.x" -%}
    /var/lib/nginx/wallarm_acl_default
        {%- tab name="Amazon Linux 2" -%}
    /var/lib/nginx/wallarm_acl_default
        {%- endtermtabs %}
    
    *   NGINX Plus module:
    
        {% termtabs name="Debian 8.x (jessie)" -%}
    /var/lib/nginx/wallarm_acl_default
        {%- tab name="Debian 9.x (stretch)" -%}
    /var/lib/nginx/wallarm_acl_default
        {%- tab name="Debian 10.x (buster)" -%}
    /var/lib/nginx/wallarm_acl_default
        {%- tab name="Ubuntu 14.04 LTS (trusty)" -%}
    /var/lib/nginx/wallarm_acl_default
        {%- tab name="Ubuntu 16.04 LTS (xenial)" -%}
    /var/lib/nginx/wallarm_acl_default
        {%- tab name="Ubuntu 18.04 LTS (bionic)" -%}
    /var/lib/nginx/wallarm_acl_default
        {%- tab name="CentOS 6.x" -%}
    /var/lib/nginx/wallarm_acl_default
        {%- tab name="CentOS 7.x" -%}
    /var/lib/nginx/wallarm_acl_default
        {%- tab name="Amazon Linux 2" -%}
    /var/lib/nginx/wallarm_acl_default
        {%- endtermtabs %}
    
    *   NGINX-Wallarm module (DEPRECATED):
    
        {% termtabs name="Debian 8.x (jessie)" -%}
    /var/lib/nginx-wallarm/wallarm_acl_default
        {%- tab name="Debian 9.x (stretch)" -%}
    /var/lib/nginx-wallarm/wallarm_acl_default
        {%- tab name="Debian 10.x (buster)" -%}
    /var/lib/nginx-wallarm/wallarm_acl_default
        {%- tab name="Ubuntu 14.04 LTS (trusty)" -%}
    /var/lib/nginx-wallarm/wallarm_acl_default
        {%- tab name="Ubuntu 16.04 LTS (xenial)" -%}
    /var/lib/nginx-wallarm/wallarm_acl_default
        {%- tab name="Ubuntu 18.04 LTS (bionic)" -%}
    /var/lib/nginx-wallarm/wallarm_acl_default
        {%- tab name="CentOS 6.x" -%}
    /var/cache/nginx-wallarm/wallarm_acl_default
        {%- tab name="CentOS 7.x" -%}
    /var/cache/nginx-wallarm/wallarm_acl_default
        {%- tab name="Amazon Linux 2" -%}
    /var/cache/nginx-wallarm/wallarm_acl_default
        {%- endtermtabs %}
         
3.  Turn on blocking for the particular vhosts and/or locations by adding the following lines to their configuration files:

    ```
    server {
        ...
        wallarm_acl default;
        ...
    }
    ```

4.  Add the following lines to the `/etc/wallarm/node.yaml` file:

    ```
    sync_blacklist:
        nginx_url: http://127.0.0.9/wallarm-acl
    ```

5.  Activate the blacklist synchronization. 

    One way to do this is to uncomment the line containing `sync-blacklist` as a substring in the `/etc/cron.d/wallarm-node-nginx` file by removing the `#` symbol at the beginning of the line. 
    
    You can also do this by running the following command:
    
    ```term
    # sed -i -Ee 's/^#(.*sync-blacklist.*)/\1/' /etc/cron.d/wallarm-node-nginx
    ```
    
    !!! info "Using the `sed` command"
    > Sed is a stream editor.
    > 
    > By default, sed writes to standard output. The `-i` option means that the file will be edited in-place.
    > 
    > The `-eE` option comprises two options:
    > * The `-e` option means the following:
    >   * The first non-option parameter will be used as a script to run on the input.
    >   * The second non-option parameter will be used as an input file. 
    > * The `-E` option means that the script following this option uses the [extended regular expression syntax](https://www.gnu.org/software/sed/manual/sed.html#ERE-syntax).
    > <br></br>
    > 
    > The script that follows the options replaces the lines that satisfy the `^#(.*sync-blacklist.*)` regular expression with the string that satisfies the subexpression in parenthesis in the `/etc/cron.d/wallarm-node-nginx` file. The `\1` back-reference of the sed command means that the subexpression in the first parenthesis should be used as a replacement.
    >
    > The line that satisfies the `^#(.*sync-blacklist.*)` regular expression
    > * starts with the `#` symbol.
    > * contains `sync-blacklist` as a substring.
    > <br></br>
    > 
    > The replacement for the described line is the substring of this line without the `#` symbol at the beginning of the line. 
    > 
    > This command uncomments the line that enables blacklist synchronization. Thus, the blacklist synchronization will be activated.
    > 
    > You can learn more about sed by proceeding with the [link](https://www.gnu.org/software/sed/manual/sed.html).
    
6.  You can add IP addresses to the whitelist to skip checking of the blacklist upon receiving a request from them. For example, the following lines in the vhost or location configuration file add the `1.2.3.4/32` IP address pool to its whitelist:

    ```
    server {
        ...
        wallarm_acl default;
        allow 1.2.3.4/32;
        satisfy any;
        ...
    }
    ```
