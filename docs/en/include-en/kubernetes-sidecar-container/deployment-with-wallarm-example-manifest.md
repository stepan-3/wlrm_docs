```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: wallarm # Wallarm element: definition of Wallarm sidecar container
        image: wallarm/node:2.14
        imagePullPolicy: Always
        env:
        - name: WALLARM_API_HOST # Wallarm API endpoint: "api.wallarm.com" for the EU cloud, "us1.api.wallarm.com" for the US cloud
          value: "api.wallarm.com"
        - name: DEPLOY_USER # username of the user with the Deploy role
          value: "username"
        - name: DEPLOY_PASSWORD # password of the user with the Deploy role
          value: "password"
        - name: DEPLOY_FORCE
          value: "true"
        - name: WALLARM_ACL_ENABLE
          value: "true"
        - name: TARANTOOL_MEMORY_GB # amount of memory in GB for request analytics data, recommended value is 75% of the total server memory
          value: 2
        ports:
        - name: http
          containerPort: 80 # port on which the Wallarm sidecar container accepts requests from the Service object
        volumeMounts:	
        - mountPath: /etc/nginx/sites-enabled	
          readOnly: true	
          name: wallarm-nginx-conf
      - name: myapp # definition of your main app container
        image: <Image>
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: 8080 # port on which the application container accepts incoming requests
      volumes: # 
      - name: wallarm-nginx-conf # Wallarm element: definition of the wallarm-nginx-conf volume
        configMap:
          name: wallarm-sidecar-nginx-conf
          items:
            - key: default
              path: default
```