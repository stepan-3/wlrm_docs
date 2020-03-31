# Summary

* [Wallarm Overview](README.md)

## Quick Start

* [How Wallarm Works](quickstart-en/qs-intro-en.md)
* [Prerequisites](quickstart-en/qs-prereq-en.md)
* Installation
  * [Creating the Wallarm Account](quickstart-en/qs-license-en.md)
  * [Install the Filter Node (NGINX)](quickstart-en/qs-install-node-en.md)
* [Configure Traffic Proxying](quickstart-en/qs-setup-proxy-en.md)
* [Check the Filter Node Operation](quickstart-en/qs-check-operation-en.md)

## Administrator Guide

* [Introduction](admin-en/admin-intro-en.md)
* Installation
  * [Supported Platforms](admin-en/supported-platforms.md)
  * NGINX
    * [Installation Options Overview](admin-en/installation-nginx-overview.md)
    * [Installing as a Dynamic Module for NGINX](admin-en/installation-nginx-en.md)
    * [Installing as a Dynamic Module with NGINX from Debian/CentOS Repositories](admin-en/installation-nginx-distr-en.md)
    * [Installing with NGINX Plus](admin-en/installation-nginxplus-en.md)
    * [Installing with Docker](admin-en/installation-docker-en.md)
  * Envoy
    * [Installing with Docker](admin-en/installation-guides/envoy/envoy-docker.md)   
  * Kubernetes
    * Installing Wallarm Ingress Controller
      * [Installing NGINX Ingress Controller with Integrated Wallarm Services](admin-en/installation-kubernetes-en.md)
      * Installing NGINX Plus Ingress Controller with Integrated Wallarm Services
        * [Introduction](admin-en/installation-guides/ingress-plus/introduction.md)
        * [Building the Wallarm NGINX Plus Ingress Controller](admin-en/installation-guides/ingress-plus/assembly.md)
        * [Deploying the Wallarm NGINX Plus Ingress Controller](admin-en/installation-guides/ingress-plus/deploy.md)
        * [Creating an Ingress Resource](admin-en/installation-guides/ingress-plus/resource-creation.md)
        * [Checking the Operation of the Wallarm Services](admin-en/installation-guides/ingress-plus/wallarm-services-check.md)
    * Installing Wallarm Sidecar Container
      * [How It Works](admin-en/installation-guides/kubernetes/wallarm-sidecar-container.md)
      * [Kubernetes Deployment Based on Helm Charts](admin-en/installation-guides/kubernetes/wallarm-sidecar-container-helm.md)
      * [Kubernetes Deployment Based on Manifests](admin-en/installation-guides/kubernetes/wallarm-sidecar-container-manifest.md)
  * Cloud Platforms
    * [Installation Options Overview](admin-en/installation-guides/clouds-intro.md)
    * [Installing on Amazon AWS](admin-en/installation-ami-en.md)
      * [Creating and Configuring the Wallarm Filter Node Instance](admin-en/installation-ami-en.md)
      * [Creating an Amazon Machine Image](admin-en/installation-guides/amazon-cloud/create-image.md)
      * [Setting Up Filter Node Auto-Scaling](admin-en/installation-guides/amazon-cloud/autoscaling-overview.md)
        * [Overview](admin-en/installation-guides/amazon-cloud/autoscaling-overview.md)
        * [Setting Up Filter Node Auto-Scaling](admin-en/installation-guides/amazon-cloud/autoscaling-group-guide.md)
        * [Setting Up Incoming Request Balancing](admin-en/installation-guides/amazon-cloud/load-balancing-guide.md)
    * [Installing on the Google Cloud Platform](admin-en/installation-gcp-en.md)
      * [Creating and Configuring the Wallarm Filter Node Instance](admin-en/installation-gcp-en.md)
      * [Creating an Image with the Wallarm Filter Node](admin-en/installation-guides/google-cloud/create-image.md)
      * [Setting Up Filter Node Auto-Scaling](admin-en/installation-guides/google-cloud/autoscaling-overview.md)
        * [Overview](admin-en/installation-guides/google-cloud/autoscaling-overview.md)
        * [Creating a Filter Node Instance Template](admin-en/installation-guides/google-cloud/creating-instance-template.md)
        * [Creating a Managed Instance Group with Enabled Auto-Scaling](admin-en/installation-guides/google-cloud/creating-autoscaling-group.md)
        * [Setting up Incoming Request Balancing](admin-en/installation-guides/google-cloud/load-balancing-guide.md)
    * [Installing on Heroku](admin-en/installation-heroku-en.md)
  * [Installing on the Kong Platform](admin-en/installation-kong-en.md)
  * [Installing on Linux [DEPRECATED]](admin-en/installation-linux-en.md)
  * [Separate Postanalytics Installation](admin-en/installation-postanalytics-en.md)
  * [Checking the Filter Node Operation](admin-en/installation-check-operation-en.md)
* Configuration
  * [Configuration Options (NGINX)](admin-en/configure-parameters-en.md)
  * [Configuration Options (Envoy)](admin-en/configuration-guides/envoy/fine-tuning.md)
  * [Filtering Mode Configuration](admin-en/configure-wallarm-mode.md)
  * [Configuring and Working with the Statistics Service](admin-en/configure-statistics-service.md)
  * [Fine-tuning of Wallarm Ingress Controller](admin-en/configure-kubernetes-en.md)
  * [Analyzing Mirrored Traffic with NGINX](admin-en/mirror-traffic-en.md)
  * [Blocking Part of a Website](admin-en/block-part-en.md)
  * [Settings for Using a Balancer or Proxy](admin-en/using-proxy-or-balancer-en.md)
  * [Masking Sensitive Data](admin-en/sensitive-data-en.md)
  * [Filter Node and Cloud Synchronization Configuration](admin-en/configure-cloud-node-synchronization-en.md)
  * [Working with Filter Node Logs](admin-en/configure-logging.md)
  * Using Single Sign-On (SSO)
    * [Introduction](admin-en/configuration-guides/sso/intro.md)
    * Connecting SSO with G Suite
      * [Overview](admin-en/configuration-guides/sso/gsuite/overview.md)
      * [Step 1: Generating Parameters on the Wallarm Side](admin-en/configuration-guides/sso/gsuite/setup-sp.md)
      * [Step 2: Creating and Configuring an Application in G Suite](admin-en/configuration-guides/sso/gsuite/setup-idp.md)
      * [Step 3: Transferring G Suite Metadata to the Wallarm Setup Wizard](admin-en/configuration-guides/sso/gsuite/metadata-transfer.md)
      * [Step 4: Allowing Access to the Wallarm Application on the G Suite Side](admin-en/configuration-guides/sso/gsuite/allow-access-to-wl.md)
    * Connecting SSO with Okta
      * [Overview](admin-en/configuration-guides/sso/okta/overview.md)
      * [Step 1: Generating Parameters on the Wallarm Side](admin-en/configuration-guides/sso/okta/setup-sp.md)
      * [Step 2: Creating and Configuring an Application in Okta](admin-en/configuration-guides/sso/okta/setup-idp.md)
      * [Step 3: Transferring Okta Metadata to the Wallarm Setup Wizard](admin-en/configuration-guides/sso/okta/metadata-transfer.md)
      * [Step 4: Allowing Access to the Wallarm Application on the Okta Side](admin-en/configuration-guides/sso/okta/allow-access-to-wl.md)
    * [Configuring SSO Authentication for Users](admin-en/configuration-guides/sso/employ-user-auth.md)
    * [Disabling and Removing the Configured SSO Provider](admin-en/configuration-guides/sso/disable-sso-provider.md) 
* Integration
  * [Configuring a Failover Method](admin-en/configure-backup-en.md)
  * Blocking by IP Address
    * [Methods of Blocking by IP Address](admin-en/configure-ip-blocking-en.md)
    * [Blocking with iptables](admin-en/configure-ip-blocking-iptables-en.md)
    * [Blocking with NGINX](admin-en/configure-ip-blocking-nginx-en.md)
  * Using a Mirrored Wallarm Repository
    * [How to Mirror the Wallarm Repository for CentOS](admin-en/integration-guides/repo-mirroring/centos/how-to-mirror-repo-artifactory.md)
    * [How to Install Wallarm Packages from the Local JFrog Artifactory Repository for CentOS](admin-en/integration-guides/repo-mirroring/centos/how-to-use-mirrored-repo.md)
  * Monitoring the Filter Node
    * [Introduction](admin-en/monitoring/intro.md)
    * [How to Fetch Metrics](admin-en/monitoring/fetching-metrics.md)
    * [Available Metrics](admin-en/monitoring/available-metrics.md)
    * Examples of Exporting and Working with Metrics
      * Grafana
        * [Exporting Metrics to InfluxDB via the `collectd` Network Plugin](admin-en/monitoring/network-plugin-influxdb.md)
        * [Exporting Metrics to Graphite via the `collectd` Write Plugin](admin-en/monitoring/write-plugin-graphite.md)
        * [Working with the Filter Node Metrics in Grafana](admin-en/monitoring/working-with-grafana.md)
      * Nagios  
        * [Exporting Metrics to Nagios via the `collectd-nagios` Utility](admin-en/monitoring/collectd-nagios.md)
        * [Working with the Filter Node Metrics in Nagios](admin-en/monitoring/working-with-nagios.md)
      * Zabbix  
        * [Exporting Metrics to Zabbix via the `collectd-nagios` Utility](admin-en/monitoring/collectd-zabbix.md)
        * [Working with the Filter Node in Zabbix](admin-en/monitoring/working-with-zabbix.md)  
* Updating and Migrating
  * [Updating on Linux](admin-en/update-linux-en.md)
  * [Updating the Separately Installed Postanalytics Module](admin-en/update-postanalytics.md)
  * [Updating Docker](admin-en/update-docker-en.md)
  * [Migrating the Filter Node 2.12 to 2.14 on Linux](admin-en/migrating-212-214.md)
* Operations
  * [Changing Tarantool Memory Allocation](admin-en/configure-tarantool-en.md)
  * [Wallarm User Acceptance Testing Checklist](admin-en/uat-checklist-en.md)
* Troubleshooting
  * [Filter Node Errors out with «libproton error: did not allocate memory» [DEPRECATED]](admin-en/libproton-error-en.md)
  * [File Download Scenarios Fail](admin-en/file-download-error-en.md)
* [Wallarm API](admin-en/api-en.md)
* Support of the Scanner Operation 
  * [Disabling the IP Address Blocking of the Wallarm Scanner](admin-en/scanner-ips-whitelisting.md) 
  * Scanner Addresses
    * [Scanner Addresses for EU Cloud](admin-en/scanner-address-en.md)
    * [Scanner Addresses for US Cloud](admin-en/scanner-address-us-en.md)
  * [Contacting Wallarm Support to Stop the Scanner](admin-en/scanner-complaint-en.md)

## User guide

* [Introduction](user-guides/cloud-ui/user-intro.md)
* Dashboards
  * [Overview](user-guides/cloud-ui/dashboard/intro.md)
  * [The “WAF” Dashboard](user-guides/cloud-ui/dashboard/waf.md)
  * [The “Scanner” Dashboard](user-guides/cloud-ui/dashboard/scanner.md)
* Events
  * [Checking Events](user-guides/cloud-ui/events/check-attack.md)
  * [Analyzing Attacks](user-guides/cloud-ui/events/analyze-attack.md)
  * [Working with False Attacks](user-guides/cloud-ui/events/false-attack.md)
  * [Verifying Attacks](user-guides/cloud-ui/events/verify-attack.md)
* Vulnerabilities
  * [Checking Vulnerabilities](user-guides/cloud-ui/vulnerabilities/check-vuln.md)
  * [Analyzing Vulnerabilities](user-guides/cloud-ui/vulnerabilities/analyze-vuln.md)
  * [Closing and Opening Vulnerabilities](user-guides/cloud-ui/vulnerabilities/close-open-vuln.md)
  * [Rechecking Vulnerabilities](user-guides/cloud-ui/vulnerabilities/recheck-vuln.md)
  * [Working with False Vulnerabilities](user-guides/cloud-ui/vulnerabilities/false-vuln.md)
* Search and Filters
  * [Using Search](user-guides/cloud-ui/search-and-filters/use-search.md)
  * [Using Filters](user-guides/cloud-ui/search-and-filters/use-filter.md)
  * [Creating a Custom Report](user-guides/cloud-ui/search-and-filters/custom-report.md)
* Scanner
  * [Scanner Overview](user-guides/cloud-ui/scanner/intro.md)
  * [Working with the Scope](user-guides/cloud-ui/scanner/check-scope.md)
  * [Reserved Domains](user-guides/cloud-ui/scanner/reserved-domains.md)
  * [Scanner Settings](user-guides/cloud-ui/scanner/configure-scanner.md)
    * [Configuring Scanner Modules](user-guides/cloud-ui/scanner/configure-scanner-modules.md)
* Nodes
  * [Nodes Overview](user-guides/cloud-ui/nodes/nodes.md)
  * [Creating and Managing a Node](user-guides/cloud-ui/nodes/create-node.md)
* Rules
  * [Application Profile Rules](user-guides/cloud-ui/rules/intro.md)
  * [Inspecting Application Profile Rules](user-guides/cloud-ui/rules/view.md)
  * [Adding Rules in the Application Profile](user-guides/cloud-ui/rules/add-rule.md)
  * [Compilation and Update Of Security Rules](user-guides/cloud-ui/rules/compiling.md)
  * [How Wallarm Analyzes Requests](user-guides/cloud-ui/rules/request-processing.md)
  * [Filter Mode Rule](user-guides/cloud-ui/rules/wallarm-mode-rule.md)
  * [Rules for Data Masking](user-guides/cloud-ui/rules/sensitive-data-rule.md)
  * [Virtual Patching](user-guides/cloud-ui/rules/vpatch-rule.md)
  * [User-Defined Detection Rules](user-guides/cloud-ui/rules/regex-rule.md)
* Triggers
  * [What are Triggers](user-guides/cloud-ui/triggers/triggers.md)
  * [Creating Triggers](user-guides/cloud-ui/triggers/create-trigger.md)
  * [Disabling Triggers](user-guides/cloud-ui/triggers/disable-trigger.md)
  * [Deleting Triggers](user-guides/cloud-ui/triggers/delete-trigger.md)
* [IP Address Blacklist](user-guides/cloud-ui/blacklist/blacklist.md)
* Settings
  * [Profile](user-guides/cloud-ui/settings/account.md)
  * [General](user-guides/cloud-ui/settings/general.md)
  * [Subscriptions](user-guides/cloud-ui/settings/subscriptions.md)
  * [Applications](user-guides/cloud-ui/settings/applications.md)
  * [Markers](user-guides/cloud-ui/settings/markers.md)
  * [Users](user-guides/cloud-ui/settings/users.md)
  * [Activity Log](user-guides/cloud-ui/settings/audit-log.md)
  * Integrations
    * [Integrations Overview](user-guides/cloud-ui/settings/integrations/integrations-intro.md) 
    * [Email Reports and Notifications](user-guides/cloud-ui/settings/integrations/email.md)
    * [Slack Notifications](user-guides/cloud-ui/settings/integrations/slack.md)
    * [Telegram Reports and Notifications](user-guides/cloud-ui/settings/integrations/telegram.md)
    * [OpsGenie Notifications](user-guides/cloud-ui/settings/integrations/opsgenie.md)
    * [PagerDuty Notifications](user-guides/cloud-ui/settings/integrations/pagerduty.md)
    * [Splunk Notifications](user-guides/cloud-ui/settings/integrations/splunk.md)
    * [Sumo Logic Notifications](user-guides/cloud-ui/settings/integrations/sumologic.md)
* [Guide to Using SSO Authentication to Log in to Wallarm](user-guides/cloud-ui/use-sso.md)
    
## Partner Guide

* [Introduction](partner-en/partner-intro-en.md)
* [Signing up with Wallarm](partner-en/partner-signup-en.md)
* [Getting your UUID and Secret Key](partner-en/partner-uuid-en.md)
* [Installing the Filter Node](partner-en/partner-install-en.md)
* [Creating a Tenant](partner-en/partner-create-tenant-en.md)
* [Configuring Traffic Processing](partner-en/partner-set-tenant-en.md)

## Release Notes

* [Version 2.14](release-notes-en/relnotes-en_v2.14.md)
* [Version 2.12](release-notes-en/relnotes-en_v2.12.md)
* [Version 2.10](release-notes-en/relnotes-en_v2.10.md)
* [Version 2.8](release-notes-en/relnotes-en_v2.8.md)
* [Version 2.6](release-notes-en/relnotes-en_v2.6.md)
* [Version 2.4](release-notes-en/relnotes-en_v2.4.md)
* [Version 2.2](release-notes-en/relnotes-en_v2.2.md)

## Appendix

* [Glossary](glossary-en.md)
* [Attacks and Vulnerabilities List](attacks-vulns-list.md)
* [Contacting the Support Team](cloud-include/contacting-support.md)
