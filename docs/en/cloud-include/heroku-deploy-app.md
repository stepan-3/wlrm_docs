{% codetabs name="EU Cloud", type="sh" -%}
$ heroku create
$ heroku buildpacks:add heroku/ruby
$ heroku buildpacks:add https://github.com/wallarm/heroku-buildpack-wallarm-node.git
$ heroku config:set WALLARM_USER=<your email>
$ heroku config:set WALLARM_PASSWORD=<your password>
$ git add .
$ git commit -am "init"
$ git push heroku master
$ heroku logs -t
{%- language name="US Cloud", type="sh" -%}
$ heroku create
$ heroku buildpacks:add heroku/ruby
$ heroku buildpacks:add https://github.com/wallarm/heroku-buildpack-wallarm-node.git
$ heroku config:set WALLARM_API_HOST=us1.api.wallarm.com
$ heroku config:set WALLARM_USER=<your email>
$ heroku config:set WALLARM_PASSWORD=<your password>
$ git add .
$ git commit -am "init"
$ git push heroku master
$ heroku logs -t
{%- endcodetabs %}