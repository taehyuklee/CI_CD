# Service 관련 정리 

Service의 주요 목적은 격리된 Container간의 통신을 뚫어주는 중간다리 역할을 하는 것이다. 

- gitlab을 Pod를 띄우고 Service (NodePart)로 띄우는 예시이다.
``` shell
apiVersion: apps/v1
kind: Deployment
metadata:
  name: gitlab
  namespace: gitlab
spec:
  replicas: 1
  selector:
    matchLabels:
      app: gitlab
  template:
    metadata:
      labels:
        app: gitlab
    spec:
      containers:
      - image: gitlab/gitlab-ce
        name: gitlab
        ports:
        - containerPort: 80
        - containerPort: 443
        - containerPort: 22
        volumeMounts:
          - mountPath: /etc/gitlab
            name: gitlab-config-volume
          - mountPath: /var/opt/gitlab
            name: gitlab-data-volume
      volumes:
        - name: gitlab-config-volume
          hostPath:
            path: /etc/gitlab
        - name: gitlab-data-volume
          hostPath:
            path: /var/opt/gitlab
```
