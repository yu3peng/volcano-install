# volcano-install

## 1. 在 Ubuntu上安装 Kubernetes 集群
具体方式可以见：[ubuntu-Kubernetes](https://github.com/yu3peng/ubuntu-Kubernetes)

## 2. 安装 Helm 3
```
cd /opt && wget https://get.helm.sh/helm-v3.1.0-linux-amd64.tar.gz
tar -xvf helm-v3.1.0-linux-amd64.tar.gz
mv linux-amd64/helm /usr/local/bin/
```

## 3. 编译 volcano 源码
```
mkdir -p $GOPATH/src/volcano.sh/
cd $GOPATH/src/volcano.sh/
wget https://github.com/volcano-sh/volcano/archive/v0.4.tar.gz
tar -zxvf v0.4.tar.gz
mv volcano-0.4 volcano
cd volcano/
make images
```

## 4. Helm 安装 volcano
```
kubectl create namespace volcano-trial
helm install installer/helm/chart/volcano --namespace volcano-trial --name-template volcano-trial
```

## 5. 安装后效果
```
$ kubectl get all --namespace volcano-trial
NAME                                             READY   STATUS      RESTARTS   AGE
pod/volcano-trial-admission-55c65849b8-qllxl     1/1     Running     0          41s
pod/volcano-trial-admission-init-wj7wl           0/1     Completed   0          41s
pod/volcano-trial-controllers-5cc674b469-lkhm2   1/1     Running     0          42s
pod/volcano-trial-scheduler-8454cc9b86-w9v2j     1/1     Running     0          42s

NAME                                      TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
service/volcano-trial-admission-service   ClusterIP   10.110.163.70   <none>        443/TCP   42s

NAME                                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/volcano-trial-admission     1/1     1            1           42s
deployment.apps/volcano-trial-controllers   1/1     1            1           42s
deployment.apps/volcano-trial-scheduler     1/1     1            1           42s

NAME                                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/volcano-trial-admission-55c65849b8     1         1         1       42s
replicaset.apps/volcano-trial-controllers-5cc674b469   1         1         1       42s
replicaset.apps/volcano-trial-scheduler-8454cc9b86     1         1         1       42s

NAME                                     COMPLETIONS   DURATION   AGE
job.batch/volcano-trial-admission-init   1/1           17s        42s
```
