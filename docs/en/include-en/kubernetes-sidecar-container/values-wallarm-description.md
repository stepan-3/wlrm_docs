```
wallarm:
  image:
     repository: wallarm/node
     tag: 2.14
     pullPolicy: Always
  wallarm_host_api: "api.wallarm.com" # Wallarm API endpoint: "api.wallarm.com" for the EU cloud, "us1.api.wallarm.com" for the US cloud
  deploy_username: "username" # username of the user with the Deploy role
  deploy_password: "password" # password of the user with the Deploy role
  app_container_port: 80 # port on which the container accepts incoming requests, the value must be identical to ports.containerPort in definition of your main app container
  mode: "block" # request filtering modes: "off" to disable request processing, "monitoring" to process but not block requests, "block" to process all requests and block the malicious ones
  tarantool_memory_gb: 2 # amount of memory in GB for request analytics data, recommended value is 75% of the total server memory
```