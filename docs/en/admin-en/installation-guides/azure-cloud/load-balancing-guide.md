[link-create-scale-set]:            create-scale-set.md
[link-portal-azure-lb]:             https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Network%2FLoadBalancers  
[link-ip-types]:                    https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-ip-addresses-overview-arm#allocation-method
[link-lb-differences]:              https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-overview#skus
[link-floating-ip]:                 https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-multivip-overview#rule-type-2-backend-port-reuse-by-using-floating-ip
[link-docs-check-operation]:        ../../installation-check-operation-en.md

[anchor-create-lb]:             #creating-a-load-balancer
[anchor-configure-lb]:          #configuring-the-load-balancer

[img-create-lb]:            ../../../../images/installation-azure/auto-scaling/en/load-balancing-guide/add-load-balancer.png
[img-overview-lb]:          ../../../../images/installation-azure/auto-scaling/en/load-balancing-guide/load-balancer-overview.png
[img-add-backend-pool]:     ../../../../images/installation-azure/auto-scaling/en/load-balancing-guide/add-backend-pool.png
[img-health-probe]:         ../../../../images/installation-azure/auto-scaling/en/load-balancing-guide/add-health-probe.png
[img-add-lb-rule]:          ../../../../images/installation-azure/auto-scaling/en/load-balancing-guide/add-balancing-rule.png
[img-update-vms]:           ../../../../images/installation-azure/auto-scaling/en/load-balancing-guide/update-vms.png
[img-check-operation]:      ../../../../images/en/test-attack.png

#   Setting Up Incoming Requests Balancing

##  Creating a Load Balancer

Now that you have a [configured][link-create-scale-set] filter node scale set, you need to create and configure a load balancer, which distributes incoming traffic between several filter nodes in a scale set.

To create a load balancer, perform the following actions:

1.  Go to the “Load balancers” page and click the “Add” button.

2.  Select a resource group to which the load balancer should be added from the “Resource group” drop-down list.

3.  Enter the desired load balancer name into the “Name” field.

4.  Select the “Public” option in the “Type” setting for the load balancer to receive requests from external IP addresses.

5.  Select the necessary balancer SKU in the “SKU” setting.

    1.  “Basic”: provides the basic functions of a load balancer and supports up to 100 deployed virtual machines in a scale set.
    
    2.  “Standard”: provides advanced functions of a load balancer and supports up to 1000 deployed virtual machines in a scale set located in various availability zones.

    >   #### Info:: Selecting a load balancer
    >   If you specified availability zones for the virtual machines to be deployed in when creating the scale set, you need to select the “Standard” load balancer SKU. This is necessary because the load balancer and the scale set have to be located in a single availability zone to operate correctly. Working with availability zones is only supported by the standard load balancer.
    >   
    >   In this document, the “Basic” SKU is used for demonstration because no availability zones are specified in the filter nodes scale set creation guide.
    >   
    >   To see detailed information about the differences between the load balancer SKUs on the Microsoft Azure platform, proceed to this [link][link-lb-differences].

6.  Configure the load balancer’s external IP address parameters in the “Public IP address” section.

    1.  Select “Create” in the “Public IP address” setting to create a new IP address for the load balancer.

    2.  Enter the desired IP address name into the “Public IP address name” field.

    3.  Select the IP address type in the “Assignment” setting.

        1.  “Dynamic”: assign a new IP address for the load balancer when the first virtual machine is added to the scale set. When the load balancer is deleted or stopped, its IP address is released and can be used by other resources.

        2.  “Static”: assign the IP address for the load balancer upon its creation. This IP address is reserved for use by this load balancer and it is guaranteed not to be released if the load balancer stops.

        >   #### Info::
        >   To see detailed information about IP address types on the Microsoft Azure platform, proceed to this [link][link-ip-types].

    4.  Leave the “Add public IPv6 address” setting off (the “No” option selected).

7.  Press the “Review + Create” button. Make sure all the entered data is correct and click the “Create” button.

![Creating a load balancer][img-create-lb]

After creating a load balancer, you need to [set it up][anchor-configure-lb] to work with a scale set created [earlier][link-create-scale-set]. 

##  Configuring the Load Balancer

To configure the load balancer created [earlier][anchor-create-lb], perform the following actions:

1.  Proceed to the “[Load balancers][link-portal-azure-lb]” page and select the necessary load balancer from the list to open its settings window. The load balancer settings menu is on the left of its description.

    ![Load balancer settings][img-overview-lb]

2.  Configure the load balancer to use the scale set created [earlier][link-create-scale-set] by placing it into the balancer backend pool in the “Backend pools” section.

    1.  Proceed to the “Backend pools” section in the load balancer settings menu and click the “Add” button to create a backend pool for the load balancer to use.
    
    2.  Enter the desired backend pool name into the “Name” field.

    3.  Select “Virtual machine scale set” from the “Associated to” drop-down list.

    4.  Select the scale set from the “Virtual Machine Scale Set” drop-down list to set it as a load balancer backend pool.
    
    5.  Click the “OK” button.

    ![Creating a backend pool][img-add-backend-pool]

3.  Configure scale set virtual machine availability check-up rules in the “Health Probes” section.

    1.  Proceed to the “Health Probes” section in the load balancer settings menu and click the “Add” button to create a new rule.
    
    2.  Enter the desired name of the rule into the “Name” field.
    
    3.  Select the protocol that should be used to check the availability of the virtual machines from the “Protocol” drop-down list. 
    
        In this document, the TCP protocol is used to check virtual machine availability.
    
    4.  Enter the port to which the balancer should send the health check requests into the “Port” field.
    
    5.  Enter the necessary interval in seconds for health check queries into the “Interval” field.
    
    6.  Enter the number of failed health check requests required to recognize the virtual machine as an unavailable one into the “Unhealthy threshold” field.

    7.  Нажмите на кнопку «OK»;

    ![Creating a health probe][img-health-probe]
    
4.  Configure the traffic distribution rules for the load balancer in the “Load balancing rules” section.

    1.  Proceed to the “Load balancing rules” section in the load balancer settings menu and click the “Add” button to create a new rule.
    
    2.  Enter the desired name of the rule into the “Name” field.

    3.  Select the IP address version that the rule should be applied to in the “IP version” setting.

    4.  Select the protocol that the rule should be applied to in the “Protocol” setting.

    5.  Enter the port to receive the requests that should be processed according to the new rule into the “Port” field.

    6.  Enter the port to which the load balancer should redirect the incoming requests into the “Backend port” field.

        >   #### Info:: Port values
        >   Ports entered into the “Port” and “Backend port” fields may differ.

    7.  Select the backend pool configured on the basis of the scale set from the “Backend pool” drop-down list.
    
    8.  Select the virtual machine health check rule configured earlier from the “Health probe” drop-down list.
    
    9.  Specify the time period in minutes during which the connection with the client should be maintained despite the absence of new incoming requests using the “Idle timeout (minutes)” slider of the field next to it.
    
    10. In this document, a floating IP is not required, therefore the “Floating IP” function is off.
    
        If necessary, you can enable floating IP by selecting “Enabled” in the “Floating IP (direct server return)” setting. To see more information about this function, proceed to this [link][link-floating-ip].
    
    11. Click the “OK” button.

    ![Creating a load balancing rule][img-add-lb-rule]

You need to upgrade virtual machines running on the scale set to apply the changes that were made. Perform the following actions:

1.  Proceed to the “Virtual machine scale sets” page and click the required scale set.

2.  Proceed to the “Instances” section of the settings.

3.  Select all of the virtual machines by selecting the checkboxes next to each of the entries in the list and clicking the “Upgrade” button.
    
    ![Upgrading scale set virtual machines][img-update-vms]
    
4.  Confirm the virtual machines’ upgrade by clicking “Yes.”

        #### Info:: Public IP load balancer address assignment
      If you selected a dynamic IP address when creating a load balancer, it is assigned to the balancer after the virtual machine upgrade.

Now the dynamically auto-scaling set of the Wallarm filter nodes will process the incoming traffic to your application.

To check the deployed filter nodes’ operation, perform the following steps:

1.  Make sure that your application is accessible through the load balancer and the Wallarm filter nodes by referring to the balancer IP address or domain name using your browser.

2.  Make sure that the Wallarm services protect your application by [performing a test attack][link-docs-check-operation].

![The «Events» tab on the Wallarm web interface][img-check-operation]