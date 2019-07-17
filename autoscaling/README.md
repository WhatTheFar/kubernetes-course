# Autoscaling

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

# Auto-Scaling Commands (From another course)

## Commands

(In the first terminal)

```bash
# since the latest minikube doesn't enable metrics-server by default
minikube addons enable metrics-server

kubectl apply -f ./wordpress-deployment.yaml

kubectl autoscale deployment wordpress --cpu-percent=50 --min=1 --max=5
```

(In the other terminal)

```bash
kubectl run -i --tty load-generator --image=busybox --generator=run-pod/v1 /bin/sh

while true; do wget -q -O- http://wordpress.default.svc.cluster.local; done
```

Note: In the latest version of kubectl, using kubectl run with some generators except pods is deprecated. As consequence, we should specify the run-pod/v1 generator explicitly as below. For more information, take a look at https://kubernetes.io/docs/reference/kubectl/conventions/#generators

(In the first terminal)

```bash
kubectl get hpa
```

## Trouble Shooting

In the other terminal where the load-generator is running, if it shows below errors

```bash
/ # while true ; do wget -q -O- http://wordpress.default.svc.cluster.local ; done
wget: can't connect to remote host (10.XX.XXX.XXX): Network is unreachable
wget: can't connect to remote host (10.XX.XXX.XXX): Network is unreachable
wget: can't connect to remote host (10.XX.XXX.XXX): Connection timed out
wget: can't connect to remote host (10.XX.XXX.XXX): Network is unreachable
wget: can't connect to remote host (10.XX.XXX.XXX): Connection timed out
```

## Solution:

replace `http://wordpress.default.svc.cluster.local` with the output of

```bash
minikube service wordpress --url
```

For example:

```bash
while true ; do wget -q -O- http://192.168.99.100:32094 ; done
```

## Related Q&A:

https://www.udemy.com/kubernetes-from-a-devops-kubernetes-guru/learn/v4/questions/4400548

https://www.udemy.com/kubernetes-from-a-devops-kubernetes-guru/learn/v4/questions/4838588
