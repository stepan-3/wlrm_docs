[img-influxdb-query-graphical]:     ../../../images/en/monitoring/grafana-influx-1.png
[img-influxdb-query-plaintext]:     ../../../images/en/monitoring/grafana-influx-2.png
[img-query-visualization]:          ../../../images/en/monitoring/grafana-query-visualization.png
[img-grafana-0-attacks]:            ../../../images/en/monitoring/grafana-0-attacks.png
[img-grafana-16-attacks]:           ../../../images/en/monitoring/grafana-16-attacks.png

[link-grafana]:                     https://grafana.com/

[doc-network-plugin-influxdb]:      network-plugin-influxdb.md
[doc-network-plugin-graphite]:      write-plugin-graphite.md
[doc-gauge-attacks]:                available-metrics.md#number-of-attacks-reported
[doc-available-metrics]:            available-metrics.md

[anchor-query]:                     #fetching-the-required-metrics-from-the-data-source
[anchor-verify-monitoring]:         #verifying-monitoring

#   Working with the Filter Node Metrics in Grafana

If you have configured the export of metrics in InfluxDB or Graphite, then you can visualize the metrics with [Grafana][link-grafana].

<!-- -->
      #### Info:: A few assumptions
      
      This document assumes that you have deployed Grafana alongside [InfluxDB][doc-network-plugin-influxdb] or [Graphite][doc-network-plugin-graphite].
      
      
      The [`curl_json-wallarm_nginx/gauge-attacks`][doc-gauge-attacks] metric, which shows the number of attacks on an application that is protected by the `node.example.local` filter node, is used as an example.
         
      However, you can monitor any [supported metric][doc-available-metrics]. 
<!-- -->

In your browser, go to `http://10.0.30.30:3000` to open the Grafana web console, then log in to the console using the standard username (`admin`) and password (`admin`). 

In order to monitor a filter node using Grafana, you will need to
1.  Connect a data source.
2.  Fetch the required metrics from the data source.
3.  Set up metric visualization. 

It is assumed that you are using one of the following data sources:
*   InfluxDB
*   Graphite

<!-- -->
        #### Info:: See also:
      
      [Verifying Monitoring.][anchor-verify-monitoring]
<!-- -->

##  Connecting a Data Source

### InfluxDB

To connect an InfluxDB server as the data source take the following steps:
1.  On the main page of the Grafana console, click the *Add data source* button.
2.  Select “InfluxDB” as the data source type.
3.  Fill in the required parameters:
    *   Name: InfluxDB
    *   URL: `http://influxdb:8086`
    *   Database: `collectd`
    *   User: `root`
    *   Password: `root`
4.  Click the *Save & Test* button.

<!-- -->

### Graphite

To connect a Graphite server as the data source take the following steps:
1.  On the main page of the Grafana console, click the *Add data source* button.
2.  Select “Graphite” as the data source type.
3.  Fill in the required parameters:
    *   Name: Graphite
    *   URL: `http://graphite:8080`.
    *   Version: select the newest available version from the drop-down list.
4.  Click the *Save & Test* button.

<!-- -->
      #### Info:: Checking a Data Source Status
        
      If a data source was connected successfully, the “Data source is working” message should appear.
<!-- -->

### Further Actions

Perform the following actions to enable Grafana to monitor metrics:
1.  Click the *Grafana* icon in the upper left corner of the console to return to the main page.
2.  Create a new dashboard by clicking the *New Dashboard* button. Then [add a query][anchor-query] to fetch a metric to the dashboard by clicking the *Add Query* button. 

##  Fetching the Required Metrics from the Data Source

### InfluxDB

To fetch a metric from the InfluxDB data source do the following:
1.  Select the newly created “InfluxDB” data source from the *Query* drop-down list.
2.  Design a query to the InfluxDB
    *   either by using the graphical query design tool,

        ![Graphical query design tool][img-influxdb-query-graphical]

    *   or by manually filling in a query in plain text (to do this, click the *Toggle text edit* button, which is highlighted in the screenshot below).

        ![Plaintext query design tool][img-influxdb-query-plaintext]

<!-- -->

The query to fetch the `curl_json-wallarm_nginx/gauge-attacks` metric is:
```
SELECT value FROM curl_json_value WHERE (host = 'node.example.local' AND instance = 'wallarm_nginx' AND type = 'gauge' AND type_instance = 'attacks')    
```

<!-- -->

### Graphite

To fetch a metric from the Graphite data source do the following:
1.  Select the newly created “Graphite” data source from the *Query* drop-down list.
2.  Select the elements of the required metric in a sequential manner by clicking the *select metric* button for the metric’s element in the *Series* line.

    The elements of `the curl_json-wallarm_nginx/gauge-attacks` metric go as follows:
    1.  The hostname, as it was set in the `write_graphite` plugin configuration file.
   
        The `_` character serves as a delimiter by default in this plugin; therefore, the `node.example.local` domain name will be represented as `node_example_local` in the query.
   
    2.  The name of the `collectd` plugin that provides a specific value. For this metric, the plugin is `curl_json`.
   
    3.  The name of the plugin instance. For this metric, the name is `wallarm_nginx`.
    4.  The type of value. For this metric, the type is `gauge`.
    5.  The name of value. For this metric, the name is `attacks`.

<!-- -->

### Further Actions

After the creation of the query, set up a visualization for the corresponding metric.

##  Setting Up Metric Visualization

Switch from the *Query* tab to the *Visualization* tab, and select the desired visualization for the metric.

For the `curl_json-wallarm_nginx/gauge-attacks` metric, we recommend using the “Gauge” visualization:
*   Select the *Calc: Last* option to display the current metric value.
*   If necessary, you can configure thresholds and other parameters. 

![Configure visualization][img-query-visualization]

<!-- -->

### Further Actions

After configuring visualization take the following steps:
*   Complete the query configuration by clicking on the *“←”* button in the upper left corner of the console.  
*   Save any changes that were made to the dashboard.
*   Verify and confirm that Grafana is successfully monitoring the metric.

<!-- -->

##  Verifying Monitoring

After you have connected one of the data sources and configured the query and visualization for the `curl_json-wallarm_nginx/gauge-attacks` metric, check the monitoring operation:
1.  Enable automatic metric updates at five-second intervals (select a value from the drop-down list in the upper right corner of the Grafana console).
2.  Make sure that the current number of attacks on the Grafana dashboard matches the output from `wallarm-status` on the filter node:
--8<-- "en/include-en/monitoring/wallarm-status-check.md"
    
    <!-- -->
    
    ![Checking the attack counter][img-grafana-0-attacks]
    
3.  Perform a test attack on an application protected by the filter node. To do this, you can send a malicious request to the application either with the `curl` utility or a browser.
--8<-- "en//include-en/monitoring/sample-malicious-request.md"
<!-- -->
    
4.  Make sure that the attack counter has increased both in the `wallarm-status` output and on the Grafana dashboard:    
--8<-- "en/include-en/monitoring/wallarm-status-output-padded.md"
<!-- -->
    
    ![Checking the attack counter][img-grafana-16-attacks]

<!-- -->

The Grafana dashboard now displays the `curl_json-wallarm_nginx/gauge-attacks` metric values for the `node.example.local` filter node.