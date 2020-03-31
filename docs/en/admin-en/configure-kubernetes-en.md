[link-helm-chart-details]:  https://github.com/wallarm/ingress-chart#configuration

# Fine-tuning of Wallarm Ingress Controller

!!! info "Official documentation for NGINX Ingress Controller"
>
    The fine-tuning of Wallarm Ingress Controller is quite similar to that of NGINX Ingress Controller described in the [official documentation](https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/). When working with Wallarm, all options for setting up the original NGINX Ingress Controller are available.

## Additional Settings for Helm Chart

The settings are performed via the `values.yaml` file. By default, the file looks as follows:

```
  wallarm:
    enabled: false
    apiHost: api.wallarm.com
    apiPort: 444
    apiSSL: true
    token: ""
    tarantool:
      kind: Deployment
      service:
        annotations: {}
      replicaCount: 1
      arena: "0.2"
      livenessProbe:
        failureThreshold: 3
        initialDelaySeconds: 10
        periodSeconds: 10
        successThreshold: 1
        timeoutSeconds: 1
      resources: {}
    metrics:
      enabled: false

      service:
        annotations:
          prometheus.io/scrape: "true"
          prometheus.io/path: /wallarm-metrics
          prometheus.io/port: "18080"

        ## List of IP addresses at which the stats-exporter service is available
        ## Ref: https://kubernetes.io/docs/user-guide/services/#external-ips
        ##
        externalIPs: []

        loadBalancerIP: ""
        loadBalancerSourceRanges: []
        servicePort: 9913
        type: ClusterIP
    synccloud:
      resources: {}
    collectd:
      resources: {}
    acl:
      enabled: false
      resources: {}
```

Description of main parameters you can set up is provided below. Other parameters always have default value or need to be changed rarely, its description is provided by the [link][link-helm-chart-details].

### wallarm.enabled

Allows you to enable or disable Wallarm functions.

**Default value**: `false`

### wallarm.apiHost

Wallarm API endpoint. Can be:
* `api.wallarm.com` for the [EU cloud](../quickstart-en/qs-intro-en.html#eu-cloud),
* `us1.api.wallarm.com` for the [US cloud](../quickstart-en/qs-intro-en.html#us-cloud).

**Default value**: `api.wallarm.com`

### wallarm.token

The *Cloud Node* token is created on 
--8<-- "en/cloud-include/wallarm-portal-nodes.md"
. It is required to access to Wallarm API.

**Default value**: `not specified`

### wallarm.tarantool.replicaCount

The number of running pods for postanalytics. Postanalytics is used for the behavior-based attack detection.

**Default value**: `1`

### wallarm.tarantool.arena

Specifies the amount of memory allocated for postanalytics service. It is recommended to set up a value sufficient to store requests data for the last 5-15 minutes.

**Default value**: `0.2`

### wallarm.metrics.enabled

This switch toggles information and metrics collection. If [Prometheus](https://github.com/helm/charts/tree/master/stable/prometheus) is installed in the Kubernetes cluster, no additional configuration is required.

**Default value**: `false`

## Global Controller Settings 

Implemented via [ConfigMap](https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/configmap/).

Besides the standard ones, the following additional parameters are supported:

* `enable-wallarm` - enables the Wallarm module in NGINX
* [wallarm-upstream-connect-attempts](configure-parameters-en.md#wallarmtarantoolupstream)
* [wallarm-upstream-reconnect-interval](configure-parameters-en.md#wallarmtarantoolupstream)
* [wallarm-process-time-limit](configure-parameters-en.md#wallarmprocesstimelimit)
* [wallarm-process-time-limit-block](configure-parameters-en.md#wallarmprocesstimelimitblock)
* [wallarm-request-memory-limit](configure-parameters-en.md#wallarmrequestmemorylimit)
* `enable-wallarm-acl` - enables blocking by IP addresses [specified](../user-guides/cloud-ui/blacklist/blacklist.md) in your Wallarm account
* [wallarm-acl-mapsize](configure-parameters-ru.md#wallarmaclmapsize)

## Ingress Annotations

These annotations are used for setting up parameters for processing individual instances of Ingress.

[Besides the standard ones](https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/annotations/), the following additional annotations are supported:

* [nginx.ingress.kubernetes.io/wallarm-mode](configure-parameters-en.md#wallarmmode), default: off
* [nginx.ingress.kubernetes.io/wallarm-mode-allow-override](configure-parameters-en.md#wallarmmodeallowoverride)
* [nginx.ingress.kubernetes.io/wallarm-fallback](configure-parameters-en.md#wallarmfallback)
* [nginx.ingress.kubernetes.io/wallarm-instance](configure-parameters-en.md#wallarminstance)
* [nginx.ingress.kubernetes.io/wallarm-block-page](configure-parameters-en.md#wallarmblockpage)
* [nginx.ingress.kubernetes.io/wallarm-parse-response](configure-parameters-en.md#wallarmparseresponse)
* [nginx.ingress.kubernetes.io/wallarm-parse-websocket](configure-parameters-en.md#wallarmparsewebsocket)
* [nginx.ingress.kubernetes.io/wallarm-unpack-response](configure-parameters-en.md#wallarmunpackresponse)
* [nginx.ingress.kubernetes.io/wallarm-parser-disable](configure-parameters-en.md#wallarmparserdisable)
* [nginx.ingress.kubernetes.io/wallarm-acl](configure-parameters-en.md#wallarmacl)

To apply the settings to your Ingress, please use the following command:

```term
# kubectl annotate --overwrite ingress YOUR_INGRESS_NAME ANNOTATION_NAME=VALUE
```

* `YOUR_INGRESS_NAME` is the name of your Ingress,
* `ANNOTATION_NAME` is the name of the annotation from the list above,
* `VALUE` is the value of the annotation from the list above.

For example, to enable IP blocking, [create](../user-guides/cloud-ui/blacklist/blacklist.md) the addresses list in your Wallarm account and execute the following command:

```term
# kubectl annotate --overwrite ingress YOUR_INGRESS_NAME nginx.ingress.kubernetes.io/wallarm-acl=on
```