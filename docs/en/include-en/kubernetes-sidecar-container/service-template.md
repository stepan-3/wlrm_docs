```
apiVersion: v1
kind: Service
metadata:
  name: myapp
spec:
  selector:
    app: myapp
  ports:
  - port: {{ .Values.service.port }}
    targetPort: 8080 # Wallarm sidecar container port; the value must be identical to ports.containerPort in definition of Wallarm sidecar container
```