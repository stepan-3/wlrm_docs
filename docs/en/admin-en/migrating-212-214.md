[doc-install-nginx-distr]:      installation-nginx-distr-en.md#1-add-the-repositories
[check-operation]:              installation-check-operation-en.md

#   Migrating the Filter Node 2.12 to 2.14 on Linux

To migrate an existing filter node version 2.12 to version 2.14, do the following:
1.  Add the Wallarm repository for the new filter node version.
2.  Update the Wallarm packages.
3.  Restart the services.


##  Adding the Wallarm Repository for a New Filter Node Version

If your current filter node is installed on Debian (including installations from the [backports repositories][doc-install-nginx-distr]) or Ubuntu, then comment the existing repository URLs and add the new ones in the `/etc/apt/sources.list.d/wallarm.list` file:

--8<-- "en/include-en/migration-212-214/add-repos-deb.md"

If your current filter node is installed on CentOS or Amazon Linux 2, then comment the existing `baseurl` parameter and add the new one in the `/etc/yum.repos.d/wallarm-node.repo` file:

--8<-- "en/include-en/migration-212-214/add-repos-rpm.md"


##  Updating the Wallarm Packages

      #### Warning:: Update Sequence
      
      If the Wallarm modules are installed separately, first update the postanalytics module.

Update the Wallarm postanalytics module and the Wallarm NGINX/NGINX Plus/Kong module.

If these modules are installed on the same host, then execute the following command:

--8<-- "en/include-en/migration-212-214/install-modules-all.md"

If these modules are installed on the different hosts, then
*   execute the following command on the appropriate host to update the postanalytics module:<br>
--8<-- "en/include-en/migration-212-214/install-module-postanalytics.md"

*   execute the following command on the appropriate host to update the Wallarm NGINX/NGINX Plus/Kong module:<br>
--8<-- "en/include-en/migration-212-214/install-module-addon.md"

<!-- -->
   

##  Restarting the Services

After successful update of the Wallarm packages, restart the updated services by issuing the following command:

--8<-- "en/include-en/migration-212-214/restart-services.md"


##  The Migration is Complete

Now you should have successfully migrated your filter node to version 2.14.

      #### Info:: See also:
      
      [Checking the Filter Node Operation][check-operation]
