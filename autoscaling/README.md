```bash
kubectl create  -f autoscaling/hpa-example.yml

kubectl run -i --tty load-generator --image=busybox /bin/sh

# test wget
wget http://hpa-example.default.svc.cluster.local:31001
cat index.html
rm index.html

while true; do wget -q -O- http://hpa-example.default.svc.cluster.local:31001
# crtl+c to stop
```

```bash
kubectl get hpa
# watch  kubectl get hpa

kubectl get pod
```
