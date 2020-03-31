# Installing as a VMware vApp

The filter node can be installed as a VMware vApp.

The functionality of the filter node installed as a VMware vApp
is completely identical to the functionality of the other deployment options.

## Prerequisites

The virtualization system must meet the following requirements:

* vApp support
* vCenter version 5.5 or higher.
* ESXi version 5.5 or higher.

Privileges:

* User has access to the vCenter console with the permission to deploy OVF-templates.
* Access to the Wallarm cloud with the permission to create a node.

## Get the Link to the OVF Template

1. Proceed to the link that corresponds to the cloud you are using: 
    *   If you are using the EU cloud, proceed to the <https://my.wallarm.com/> link.
    *   If you are using the US cloud, proceed to the <https://us1.my.wallarm.com> link.
2. Click *Nodes*.
3. Click *OVF-template for VMware*.

The path to the OVF template will be copied to the clipboard. The link will be valid for 24 hours.

      #### Info::
    The node's access credentials are replaced automatically. If the filter node is still in operation, it will lose the ability to interact with the Wallarm API.

## Deploy the OVF Template VMware

1. Go to the vCenter through the vSphere Client or the vSphere Web Service.
2. Click *File*.
3. Click *Deploy OVF template*.
4. Paste the link to the OVF template from the clipboard.
5. On the *Name and Location* step, specify the name for the virtual machine to be displayed in the vSphere Client interface.
6. On the *Deployment configuration* step, choose configuration:

    * *up to 10mbps* is designed for test installations. It uses one virtual processor and 512 MB of memory.
    * *10-50mbps* is designed for up to 50 Mbit of traffic. It uses 4 virtual processors and 16 GB of memory.
    * *50-100mbps* is designed for up to 100 Mbit of traffic. It uses 8 virtual processors and 32 GB of memory.
    
    !!! info "Disk space"
    Make sure that the allocated disk space is at least 4x the amount of RAM. For example, if you have 16 GB of RAM, you will need 64 GB of disk space.

7. On the *Properties* step, configure the filter node parameters. These parameters are used only for the configuration on the first start.
   
    * *Hostname* is server name.
    * *HTTP hosts*: a list of domains to be processed. The requests whose Host header value is not included in the list will not be transferred to the backend and will be blocked.
    * *HTTP backends*: a list of IP addresses, to which the requests will be forwarded.

8. On the first virtual host boot, the server console will prompt to set the root password.

   !!! info "Username"
   When prompted to provide a username, enter `installer`.

    After the first boot, you need to do one of the following:

    *   Wait for 15 minutes until the filter node-specific files are downloaded.
    *   Manually run `/usr/share/wallarm-common/syncnode`.
<br>

9. Configure logging.<br><br>
--8<-- "en/include-en/installation-step-logging.md"
<!-- -->

--8<-- "en/include-en/filter-node-defaults.md"

<!-- -->

--8<-- "en/include-en/installation-extra-steps.md"