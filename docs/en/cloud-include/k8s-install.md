{% codetabs name="EU Cloud", type="sh" -%}
helm install wallarm/wallarm-ingress -n ingress-controller --set controller.wallarm.token=<CLOUD NODE TOKEN> --set controller.wallarm.enabled=true
{%- language name="US Cloud", type="sh" -%}
helm install wallarm/wallarm-ingress -n ingress-controller --set controller.wallarm.apiHost=us1.api.wallarm.com --set controller.wallarm.token=<CLOUD NODE TOKEN> --set controller.wallarm.enabled=true
{%- endcodetabs %}