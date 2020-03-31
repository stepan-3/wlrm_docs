[link-using-search]:    ../search-and-filters/use-search.md
[link-using-filters]:   ../search-and-filters/use-filter.md
[link-verify-attack]:   ../events/verify-attack.md
[link-check-vulns]:     ../vulnerabilities/check-vuln.md

[img-attacks-tab]:      ../../../../images/en/user-guides/cloud-ui/events/check-attack.png
[img-current-attacks]:  ../../../../images/en/user-guides/cloud-ui/events/current-attack.png
[img-incidents-tab]:    ../../../../images/en/user-guides/cloud-ui/events/incident-vuln.png
[img-vulns-tab]:        ../../../../images/en/user-guides/cloud-ui/events/check-vulns.png

# Checking Events

You can check attacks, incidents, and vulnerabilities in the *Events* tab of the Wallarm interface.
This tab displays data in the following tabs:

* The *Attacks* tab displays all groups of associated malicious requests.
* The *Incidents* tab displays all the malicious requests that exploit existing vulnerabilities.
* The *Vulnerabilities* tab displays all the discovered errors made when building or implementing a web application that can lead to an information security risk.

You can use the search field or manually set the data period.

## The *Attacks* Tab 

![Attacks tab][img-attacks-tab]

The *Attacks* tab displays information in the following columns:

* *Date*: The date and time of the malicious request. 
    * If several requests of the same type were detected at short intervals, the attack duration appears under the date. Duration is the time period between the first request of a certain type and the last request of the same type in the specified timeframe. 
    * If the attack is happening at the current moment, the “now” label will appear in a small red font.
* *Requests*: The number of requests in the attack in the specified time frame. 
* *Payloads*: The number of requests of the most encountered malicious code type and its name. The  number in a smaller font displayed under the main number shows the total number of requests of the same type in the attack during the specified timeframe. 
* *Top IP/Source*: The IP address from which the malicious requests originated. When the malicious requests originate from several IP addresses, the interface shows the IP address responsible for the most requests. 
     * The number in smaller black font displayed under the main number shows the total number of IP addresses from which the requests in the same attack originated during the specified timeframe. 
     * The number in small grey font shows the total number of IP addresses from which the requests in the same attack originated during the entire time.
     * If Wallarm can determine which data center the given IP addresses belong to, then one or more corresponding tags will be displayed in the column: the “AWS” tag for Amazon, the “GCP” tag for Google and the “Azure” tag for Microsoft data centers.
     * If the attack's source is the Tor network, then the “Tor” tag will be shown in the column.   
* *Domain*: The domain that the request targeted. 
    * The line in a smaller font displayed under the domain is the path that the request targeted.
* *Status*: The server's response status code on the request. When there are several response status codes, the most frequent one is displayed. 
    * The number in a smaller font displayed under the main number shows the total number of different response status codes of the protected resource on the selected attack in the specified timeframe. 
* *Parameter*: The malicious request's parameters.
* *Verification*: The attack verification status.

You can click the “**Sort by latest hit**” switch to sort attacks by the time of the last request from the most recent one to the oldest one.

## The *Incidents* Tab

The *Incidents* tab displays information similarly to the *Attacks* tab, except for the last column. The table of incidents does not have the *Verification* column, but the *Vulnerabilities* column instead.

The *Vulnerabilities* column displays the vulnerability, that the corresponding incident exploited.

![Incidents tab][img-incidents-tab]

Clicking on the corresponding vulnerability brings you to its detailed description and instructions on how to fix it.

You can click the “**Sort by latest hit**” switch to sort incidents by the time of the last request from the latest one to the oldest one.

## The *Vulnerabilities* tab

![Vulnerabilities tab][img-vulns-tab]

The *Vulnerabilities* tab displays information in the following columns:
* *Date*: The date and time of vulnerability discovery.
* *Risk*: The danger level of the vulnerability.
* *Target*: The side to be the victim in the case of vulnerability exploitation.
* *Type*: The type of the malicious code that exploits the vulnerability.
* *Domain*: The domain that the vulnerability was discovered at.
* *ID*: The unique identifier of the vulnerability in the Wallarm system.
* *Title*: The title of the vulnerability.


## Events that are Currently Happening

You can check events in real time. If your company resources are receiving malicious requests, the Wallarm interface will display the following elements:
* The number of events that have happened in the last 5 minutes, which will be displayed by the *event counter* next to the *Events* tab.
* The *now* label, which is shown under the event date in the “attacks” or the “incidents” table.

You may also add the `now` keyword to the search field to only display those events happening at the moment. 
* `attacks now`—only display attacks happening right now.
* `incidents now`—only display incidents happening right now.
* `attacks incidents now`—only display attacks and incidents happening right now.

![Attacks happening right now][img-current-attacks]

!!! info "See also"
    
    * [Using search][link-using-search]
    * [Using filters][link-using-filters]
    * [Checking vulnerabilities][link-check-vulns]
    
