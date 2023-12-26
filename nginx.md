```yaml
---
apiVersion: "apps/v1"
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
    replicas: 2
    selector:
      matchLabels:
        app: nginx
    template:
      metadata:
        labels:
            app: nginx
      spec:
        containers:
        - name: nginx
          image: nginx:latest
          ports:
          - containerPort: 80
          volumeMounts:
          - name: nginx-config
            mountPath: /etc/nginx/nginx.conf
            subPath: nginx.conf
        volumes:
        - name: nginx-config
          configMap:
            name: nginx-config
            items:
            - key: nginx.conf
              path: nginx.conf
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-config
data:
    nginx.conf: |
        worker_processes  1;
        events {
            worker_connections  1024;
        }
        http {
            server {
                listen 80;
                location ^~  /download {
                   alias /bin/; ##需要下载的文件存放的目录
                   sendfile on;
                   autoindex on; #默认是不允许列出整个目录的
                   autoindex_exact_size off;
                   autoindex_localtime on;
                }    
            }
        }
    


```
