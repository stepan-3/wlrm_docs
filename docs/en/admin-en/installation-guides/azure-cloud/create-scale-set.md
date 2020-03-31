[link-azure-portal-scale-set]:          https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Compute%2FvirtualMachineScaleSets
[link-create-image]:                    create-image.md
[link-create-lb]:                       load-balancing-guide.md
[link-availability-zones]:              https://docs.microsoft.com/en-us/azure/availability-zones/az-overview
[link-create-ssh-keys]:                 ../../installation-azure-en.md#1--create-an-ssh-key-pair
[link-configure-balancing]:             load-balancing-guide.md

[img-create-scale-set]:                 ../../../../images/installation-azure/auto-scaling/en/scale-set-guide/create-scale-set.png
[img-configure-autoscaling]:            ../../../../images/installation-azure/auto-scaling/en/scale-set-guide/configure-auto-scaling.png
[img-networking-parameters]:            ../../../../images/installation-azure/auto-scaling/en/scale-set-guide/configure-networking.png
[img-check-operation]:                  ../../../../images/en/cloud-node-status.png

#   Creating and Configuring a Virtual Machine Scale Set

To create and configure a virtual machine scale set for Wallarm filter node auto-scaling, proceed with the following steps:


1.  Proceed to the “[Virtual machine scale sets][link-azure-portal-scale-set]” page and click the “Add” button to create a virtual machine scale set.

2.  Perform general virtual machine scale set configuration by entering the required data into the “Basics” section of the form.

    1.  Enter the name of the scale set into the “Virtual machine scale set name” field.
    
    2.  Click the “Browse all images” link in the “Operating system disk image” setting to specify the image to be used as the basis for the deployed virtual machines.
    
    3.  In the window that appears, proceed to the “My Items” tab and select the [previously created image][link-create-image] from the list.
    
    4.  Select the Azure subscription from the “Subscription” drop-down list.
    
    5.  Select the resource group that the deployed virtual machines should be placed into from the “Resource group” drop-down list.
    
    6.  If necessary, select the zones that the filter nodes should be deployed in from the “Availability zone” drop-down list. In this document, the availability zones for the scale set are not specified for the purpose of the demonstration.
        
        >   #### Info:: Load balancer requirements
        >   If you specify the availability zones for the virtual machine scaling set, you need to use the “Standard” load balancer SKU instead of the “Basic” SKU. 
        >   
        >   To see detailed information about creating a load balancer, proceed with this [link][link-create-lb].
        
        <!-- -->
        
        >   #### Info:: Availability zones
        >   To see detailed information about availability zones on the Azure platform, proceed to this [link][link-availability-zones].
        
    7.  Enter the desired username for access to the virtual machines into the “Username” field.
    
    8.  Select “SSH public key” as the desired “Authentication type” to be able to connect to the virtual machines in the scale set via SSH keys.
        
        >   #### Info:: Authentication via password
        >   You may also enable authentication with a username and a password; however, connecting to the virtual machines via SSH keys is preferred.
        
    9.  Generate a pair of SSH keys and paste a public key with the OpenSSH format into the “SSH public key” field. Save the private key to use for connecting to virtual machines in the future.
        
        >   #### Info:: Generating an SSH key pair
        >   To see detailed information about the SSH key pair generation process, proceed to this [link][link-create-ssh-keys].
    
    ![Creating a scale set][img-create-scale-set]

3.  Configure virtual machine scale set auto-scaling by entering the required data in the “Autoscale” section.

    1.  Select “Enabled” in the “Autoscale” parameter to enable virtual machine set auto-scaling.
    
    2.  Specify the minimum size of the set in the “Minimum number of VMs” field (for example, two virtual machines).
    
    3.  Specify the maximum size of the set in the “Maximum number of VMs” field (for example, ten virtual machines).
    
    4.  Configure the rule for scale set size increase in the “Scale out” section.
    
        1.  Enter the average CPU load percentage (for example, 60 percent) into the “CPU threshold (%)” field. Upon reaching this CPU load, the scale set size increases.
        
        2.  Enter the quantity of virtual machines to be added to the scale set upon reaching the “CPU threshold (%)” field value into the “Number of VMs to increase by” field. 
        
    5.  Configure the rule for scale set size decrease in the “Scale in” section.
    
        1.  Enter the average CPU load percentage (for example, 30 percent) into the “CPU threshold (%)” field. Upon reaching this CPU load, the scale set size decreases.
        
        2.  Enter the quantity of virtual machines to be removed from the scale set upon reaching the “CPU threshold (%)” field value into the “Number of VMs to decrease by” field.

    ![Configuring the scale set auto-scaling][img-configure-autoscaling]

4.  Configure the network parameters for the scale set by entering the required data in the “Networking” section.

    1.  Select “None” in the “Choose load balancing options” parameter to configure a load balancer for the scale set [later][link-configure-balancing].
    
    2.  Select the necessary virtual network and subnet where the virtual machines for the scale set should be deployed.
    
    3.  Select “Off” in the “Public IP address per instance” parameter. Because further traffic routing is conducted by the load balancer, there is no need to assign a public IP address for each filter node instance in the set.

    ![Configuring scale set networking][img-networking-parameters]

5.  Make sure all of the parameters of the scale set are configured correctly and click the “Create” button.

The specified number of virtual machines will automatically launch upon the successful creation of the scale set.

You can check that the scale set was created correctly by viewing the number of launched machines in the set and comparing this data point with the number of filter nodes connected to the Wallarm cloud.

You can do this using the Wallarm website. For example, if two virtual machines with filter nodes are currently launched, the Wallarm website will display the “2/2 nodes are active” label for the corresponding cloud node on the *Nodes* tab.

![Checking the scale set status][img-check-operation]

You can now proceed to [creating and configuring a load balancer][link-configure-balancing].