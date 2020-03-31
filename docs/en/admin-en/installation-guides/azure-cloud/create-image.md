[link-docs-azure-autoscaling]:              autoscaling-overview.md
[link-docs-azure-node-setup]:               ../../installation-azure-en.md
[link-docs-azure-create-ssh-key]:           ../../installation-azure-en.md#1--create-an-ssh-key-pair
[link-cloud-connect-guide]:                 ../../installation-azure-en.md#5--connect-the-filter-node-to-the-wallarm-cloud
[link-docs-reverse-proxy-setup]:            ../../../quickstart-en/qs-setup-proxy-en.md
[link-docs-check-operation]:        ../../installation-check-operation-en.md
[link-deprovision]:                 https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/agent-linux#commands
[link-azure-portal-vm]:             https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Compute%2FVirtualMachines
[link-availability-zones]:          https://docs.microsoft.com/en-us/azure/availability-zones/az-overview
[link-azure-portal-images]:         https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Compute%2Fimages

[anchor-node]:          #1--creating-and-configuring-a-virtual-machine-containing-the-filter-node-on-microsoft-azure
[anchor-image]:         #2--creating-a-virtual-machine-image

[img-stop-vm]:          ../../../../images/installation-azure/auto-scaling/en/create-image/stop-vm.png
[img-create-image]:     ../../../../images/installation-azure/auto-scaling/en/create-image/capture-image.png
[img-images-list]:      ../../../../images/installation-azure/auto-scaling/en/create-image/images-overview.png


#   Creating an Image with the Wallarm Filter Node on the Microsoft Azure Platform

To set up auto-scaling of the Wallarm filter nodes deployed on the Microsoft Azure platform, you first need virtual machine images. This document describes the procedure for preparing an image of the virtual machine with the Wallarm filter node installed. For detailed information about setting up auto-scaling, proceed to this [link][link-docs-azure-autoscaling].

To create an image with the Wallarm filter node on the Microsoft Azure platform, perform the following procedures:
1.  [Creating and configuring a virtual machine with the filter node on the Microsoft Azure platform][anchor-node];
2.  [Creating an image on the basis of the configured virtual machine][anchor-image].


##  1.  Creating and Configuring a Virtual Machine Containing the Filter Node on Microsoft Azure

Before creating an image, you need to perform an initial configuration of a single Wallarm filter node. To configure a filter node, do the following:

1.  [Create and configure][link-docs-azure-node-setup] a virtual machine with the filter node on the Microsoft Azure platform.

    >   #### Warning:: Provide the filter node with an internet connection
    >   The filter node requires access to a Wallarm API server for proper operation. The choice of Wallarm API server depends on the Wallarm Cloud you are using:
    >   *   If you are using the EU cloud, your node needs to be granted access to `https://api.wallarm.com:444`.
    >   *   If you are using the US cloud, your node needs to be granted access to `https://us1.api.wallarm.com:444`.
    
    <!-- -->
    
    >   #### Info:: Connecting to the virtual machine via a custom private key
    >   Make sure you have access to the [private key][link-docs-azure-create-ssh-key] from the key pair that is used to connect to your filter node via SSH.

2.  [Connect][link-cloud-connect-guide] the filter node to the Wallarm cloud.

    >   #### Warning:: Use a token to connect to the Wallarm cloud
    >   Please note that you need to connect the filter node to the Wallarm cloud using the `addcloudnode` script. Multiple filter nodes are allowed to connect to the Wallarm cloud using the same token. 
    >   
    >   Thus, you will not need to manually connect each of the filter nodes to the Wallarm cloud when the size of the scale set increases.

3.  [Configure][link-docs-reverse-proxy-setup] the filter node to act as a reverse proxy for your web application.

4.  [Make sure][link-docs-check-operation] that the filter node is configured correctly and protects your web application against malicious requests.

5.  It is recommended to perform a virtual machine deprovision before creating an image so that you create a deprovisioned image of the virtual machine. Virtual machines created on the basis of such an image can be deployed in the scale set with new user accounts without any dependence on the user who created the image.
    1.  Connect to the virtual machine via the SSH protocol using the private key [created][link-docs-azure-create-ssh-key] during filter node deployment on the Azure platform.
    
    2.  Run the following deprovision command:
    
        ```term
        # waagent -deprovision+user
        ```

    3.  Enter `y` to confirm deprovisioning.
    
    4.  Wait until the deprovision process is finished.

    >   #### Warning:: The deprovision process
    >   During the deprovision process, some of the files and the existing user account that was used to connect to the machine via SSH are deleted from the virtual machine.
    >   
    >   To see detailed information about the deprovision procedure, proceed to this [link][link-deprovision].
    >   
    >   Note that the filter node configuration **is not affected** by the deprovision procedure.

After you have finished configuring the virtual machine, turn it off by completing the following actions:
1.  Navigate to the “[Virtual Machines][link-azure-portal-vm]” page.
2.  Open the drop-down menu by clicking the menu button on the right of the “Subscription” column.
3.  Select “Stop” in the drop-down menu.

![Stopping a virtual machine][img-stop-vm]

##  2.  Creating a Virtual Machine Image

To create an image and use it successfully in the future, virtual machine deprovision is required.

        #### Info:: Detailed information
        To see detailed information about the virtual machine deprovision process, proceed to this [link][anchor-node].

You can now create a virtual machine image based on the configured filter node instance. To create an image, perform the following steps:

1.  Proceed to the “[Virtual Machines][link-azure-portal-vm]” page and click the name, from the list, of the [previously created virtual machine][anchor-node].

2.  Click the “Capture” button in the virtual machine overview window that appears.

3.  Enter the desired image name into the “Name” field.

4.  Select the resource group that the image should be placed into from the “Resource group” drop-down list. If necessary, you can create a new resource group by clicking the “Create” button under the drop-down list.

5.  During the image creation process, the generalization of the base virtual machine is performed. After this action, the machine becomes unavailable for further use and cannot be launched. If you want to delete the base virtual machine after image capturing, select the “Automatically delete this virtual machine after creating the image” checkbox.

6.  If necessary, turn on zone resiliency by clicking the “On” button.
    
    This function replicates the image onto all of the availability zones of the current region. This ensures that the image is still accessible from other zones in case of an accident in one of the availability zones where the image is stored.
    
    >   #### Info:: Detailed information
    >   To see detailed information about availability zones on the Azure platform, proceed with this [link][link-availability-zones].

    ![Creating an image][img-create-image]

7.  Click the “Create” button to launch the virtual machine image creation process.

Once the creation process is finished, make sure that the created image is present in the list on the “[Images][link-azure-portal-images]” page. You can find it by searching by name.

![Images list][img-images-list]

Now you can [set up the auto-scaling][link-docs-azure-autoscaling] of Wallarm filter nodes on the Microsoft Azure platform using the prepared image.