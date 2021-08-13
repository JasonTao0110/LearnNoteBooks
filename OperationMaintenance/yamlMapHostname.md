


```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: uploader-base
spec:
  replicas: 1
  selector:
    matchLabels:
      pod: uploader-base
  template:
    metadata:
      labels:
        pod: uploader-base
    spec:
      containers:
        - name: uploader-base
          image: uploader-baseimage
          imagePullPolicy: Always
          env:
            - name: CONF_ROOT
              value: /app-config
          volumeMounts:
            - name: config-volume
              mountPath: /app-config
          ports:
            - containerPort: 8000
      volumes:
        - name: config-volume
          configMap:
            name: uploader-app-config
      hostAliases:
        - ip: "192.168.1.13"
          hostnames:
            - "hadoop2"
        - ip: "192.168.1.19"
          hostnames:
            - "hadoop4"
        - ip: "192.168.1.15"
          hostnames:
            - "hadoop1"


```