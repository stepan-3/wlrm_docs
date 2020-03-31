{% codetabs name="EU Cloud", type="sh" -%}
heroku config:set WALLARM_USER=<your email>
heroku config:set WALLARM_PASSWORD=<your password>
{%- language name="US Cloud", type="sh" -%}
heroku config:set WALLARM_API_HOST=us1.api.wallarm.com
heroku config:set WALLARM_USER=<your email>
heroku config:set WALLARM_PASSWORD=<your password>
{%- endcodetabs %}