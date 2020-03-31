[link-create-image]:            create-image.md
[link-create-scale-set]:        create-scale-set.md
[link-setup-balancing]:         load-balancing-guide.md
[link-create-lb]:               load-balancing-guide.md#creating-a-load-balancer
[link-configure-lb]:            load-balancing-guide.md#configuring-the-load-balancer

#   Filter Node Auto-Scaling: Overview

You can set up Wallarm filter node auto-scaling on the Microsoft Azure platform to make sure that filter nodes are capable of handling traffic fluctuations (if there are any). Enabling auto-scaling allows the processing of incoming requests to the application using the filter nodes even when traffic soars significantly.

      #### Warning:: Prerequisites
      Setting up auto-scaling requires an image of the virtual machine with the Wallarm filter node.
      
        For detailed information about creating an image of the virtual machine with the Wallarm filter node on the Azure platform, proceed to this [link][link-create-image].

To auto-scale filter nodes on the Microsoft Azure platform, perform the following steps:
1.  [Create and configure a virtual machine scale set][link-create-scale-set]
2.  [Set up incoming requests balancing][link-setup-balancing]
    1.  [Create a load balancer][link-create-lb]
    2.  [Configure the load balancer][link-configure-lb]
