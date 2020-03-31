[img-ssh-key-generation]:       ../../images/installation-azure/common/ssh-key-generation.png
[img-resource-search]:          ../../images/installation-azure/en/resource-search.png
[img-wl-prod-description]:      ../../images/installation-azure/en/wallarm-product-description.png
[img-az-marketplace]:           ../../images/installation-azure/en/azure-marketplace.png    
[img-vm-wizard-basics]:         ../../images/installation-azure/en/vm-wizard-basics.png
[img-vm-wizard-auth]:           ../../images/installation-azure/en/vm-wizard-auth.png
[img-vm-wizard-review]:         ../../images/installation-azure/en/vm-wizard-review.png
[img-vm-deployment]:            ../../images/installation-azure/en/vm-deployment.png
[img-connect-to-vm]:            ../../images/installation-azure/en/connect-to-vm.png

[link-az-portal]:               https://portal.azure.com
[link-az-ssh-doc]:              https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows
[link-az-marketplace-wl]:       https://azuremarketplace.microsoft.com/en-us/marketplace/apps/wallarm.wallarm-ng-waf-offer-1?tab=Overview
[link-qsg-linux-vm]:            https://docs.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-portal

[link-qsg-wl-proxy]:            https://docs.wallarm.com/en/quickstart-en/qs-setup-proxy-en.html
[link-tarantool]:               https://tarantool.io/en/ 
[link-check-node-operation]:    https://docs.wallarm.com/en/admin-en/installation-check-operation-en.html


#   Deploying on Microsoft Azure

Azure Marketplace provides [an deployment-ready Linux image][link-az-marketplace-wl] with pre-installed filter node software.

To deploy a filter node on Microsoft Azure cloud, do the following steps:

1.  Create an SSH key pair.
2.  Log in to Microsoft Azure portal.
3.  Create and run a virtual machine with a filter node software.
4.  Connect to the virtual machine via SSH protocol.
5.  Set up the filter node for using a proxy server.
6.  Connect the filter node to the Wallarm cloud.
7.  Set up the proxying and filtering rules for the filter node.
8.  Tune the memory allocation policy for the filter node.
9.  Configure logging.
10. Restart NGINX.
     
<!-- -->
        
##  1.  Create an SSH Key Pair

During the deployment process you will connect to a virtual machine using the SSH protocol.

The Azure cloud defines two means of getting authenticated while using the SSH protocol: either by login and password or by SSH key pair. Authentication with SSH key pair is considered to be more secure compared to login and password authentication method. Azure uses SSH key pair for authentication by default.

Create an SSH RSA key pair. For example, you could use `ssh-keygen` or `PuTTYgen` tools.

![SSH keys generating with PuTTYgen][img-ssh-key-generation]

See [How to use SSH keys with Windows on Azure][link-az-ssh-doc] for more information.

<!-- -->
    
##  2.  Log in to Microsoft Azure Portal

Log in to the Azure [portal][link-az-portal].

<!-- -->
    
##  3.  Create and Run a Virtual Machine with a Filter Node Software

To create a virtual machine with a filter node software, do the following:

1.  In the upper left corner of the Azure portal homepage select **Create a resource**. 

2.  Search for “wallarm” in the search bar.

    ![Resource search][img-resource-search]

3.  Select “Wallarm - Next-Gen Web Application Firewall”. 

    The Wallarm product description page will open.

    ![Wallarm product description][img-wl-prod-description]

    Alternatively, you could reach the same page using Azure Marketplace. To do that, go to the [link][link-az-marketplace-wl] and select **Get it now**.

    ![Wallarm on Azure Marketplace][img-az-marketplace]

4.  Select **Create** to open a “Create a virtual machine” wizard.

5.  In the “Basics” tab select the correct subscription (from your Azure account), set the name and the size of a virtual machine. 

    ![Virtual machine wizard: basics][img-vm-wizard-basics]

6.  Choose an authentication method to be used with the VM. 

    If you choose SSH key pair as an authentication method, provide a user name and the public SSH key you have created [earlier](#1--create-an-ssh-key-pair).

    ![Virtual machine wizard: authentication][img-vm-wizard-auth]

7.  Set up other necessary  virtual machine parameters.

8.  Select **Review + Create**. Make sure everything is set up correctly.

    ![Virtual machine wizard: review][img-vm-wizard-review]

9.  Select **Create** to start the virtual machine deployment.

10. After the completion of deployment, select **Go to resource**.

    ![Virtual machine deployment process][img-vm-deployment]

See [Quickstart: Create a Linux virtual machine in the Azure portal][link-qsg-linux-vm] for more information.

<!-- -->
    
##  4.  Connect to the Virtual Machine via SSH Protocol

Select **Connect** on the virtual machine overview screen to view the IP address and SSH port values. If necessary, change them to appropriate values.

![Setting up connection parameters][img-connect-to-vm]

Connect to the virtual machine via SSH protocol using the private SSH key you have created [earlier](#1--create-an-ssh-key-pair). 

See [How to use SSH keys with Windows on Azure][link-az-ssh-doc] for more information.

<!-- -->
        
##  5.  Set up the Filter Node for Using a Proxy Server

--8<-- "en/include-en/setup-proxy.md"
    
<!-- -->
    
##  6.  Connect the Filter Node to the Wallarm Cloud


--8<-- "en/include-en/connect-cloud-node-cloud-en.md"
    
    

##  7.  Set up the Proxying and Filtering Rules for the Filter Node

--8<-- "en/include-en/setup-filter-nginx-en.md"

<!-- -->
    
##  8.  Tune the Memory Allocation Policy for the Filter Node

The filter node uses [Tarantool][link-tarantool] to store data in memory. By default the amount of RAM allocated to the Tarantool is set to 75% of the total virtual machine memory. 

You could change this value, if needed. To do so, perform the following steps:

1.  Open the configuration file `/etc/default/wallarm-tarantool`:

    ```term
    # nano /etc/default/wallarm-tarantool
    ```

2.  Set the amount of allocated RAM in GB with `SLAB_ALLOC_ARENA` parameter. 

    For example, if it is necessary to provide 24 GB of memory to the Tarantool, the parameter should be set like:

    ```
    SLAB_ALLOC_ARENA=24
    ```

3.  Save your changes and exit from the editor.

4.  Restart the Tarantool daemon:

    ```term
    # systemctl restart wallarm-tarantool
    ```

<!-- -->

# 9. Configure Logging

<!-- -->
--8<-- "en/include-en/installation-step-logging.md"
<!-- -->

# 10. Restart NGINX

Restart NGINX by running the following command:

```term
# systemctl restart nginx
```

<!-- -->

##  The Deployment Is Completed

You have completed the deployment process successfully. 

Check if the filter node is operating normally and proxying the traffic through itself. 

See [Checking the filter node operation][link-check-node-operation] for more information.

--8<-- "en/include-en/filter-node-defaults.md"

<!-- -->

--8<-- "en/include-en/installation-extra-steps.md"