{% codetabs name="EU Cloud", type="sh" -%}
docker run -d -e DEPLOY_USER="deploy@example.com" -e DEPLOY_PASSWORD="very_secret" -e NGINX_BACKEND=example.com -e TARANTOOL_MEMORY_GB=memvalue -p 80:80 wallarm/node
{%- language name="US Cloud", type="sh" -%}
docker run -d -e WALLARM_API_HOST=us1.api.wallarm.com -e DEPLOY_USER="deploy@example.com" -e DEPLOY_PASSWORD="very_secret" -e NGINX_BACKEND=example.com -e TARANTOOL_MEMORY_GB=memvalue -p 80:80 wallarm/node
{%- endcodetabs %}