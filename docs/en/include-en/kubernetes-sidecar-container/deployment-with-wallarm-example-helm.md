```
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    checksum/config: {{ include (print $.Template.BasePath "/wallarm-sidecar-configmap.yaml") . | sha256sum }} # Wallarm element: annotation to update running pods after changing Wallarm ConfigMap
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
        image: {{ .Values.wallarm.image.repository }}:{{ .Values.wallarm.image.tag }}
        imagePullPolicy: {{ .Values.wallarm.image.pullPolicy | quote }}
        env:
        - name: WALLARM_API_HOST
          value: {{ .Values.wallarm.wallarm_host_api | quote }}
        - name: DEPLOY_USER
          value: {{ .Values.wallarm.deploy_username | quote }}
        - name: DEPLOY_PASSWORD
          value: {{ .Values.wallarm.deploy_password | quote }}
        - name: DEPLOY_FORCE
          value: "true"
        - name: TARANTOOL_MEMORY_GB
          value: {{ .Values.wallarm.tarantool_memory_gb | quote }}
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
      volumes:
      - name: wallarm-nginx-conf # Wallarm element: definition of the wallarm-nginx-conf volume
        configMap:
          name: wallarm-sidecar-nginx-conf
          items:
            - key: default
              path: default
```