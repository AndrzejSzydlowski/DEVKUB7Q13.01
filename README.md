# Домашнее задание к занятию "13.1 контейнеры, поды, deployment, statefulset, services, endpoints"
## Задание 1: подготовить тестовый конфиг для запуска приложения 
### Конфигурация тестовой среды для фронтенда и бэкенда - [app-deploy.yaml](app-deploy.yaml) Конфигурация для БД - [pg.yaml](pg.yaml)

```
rimm@cp1:~$ kubectl get deployment
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
firstapp   1/1     1            1           3d22h
```

```
rimm@cp1:~$ kubectl describe deployments.apps firstapp
Name:                   firstapp
Namespace:              default
CreationTimestamp:      Fri, 22 Apr 2022 23:50:36 +0000
Labels:                 app=firstapp
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=firstapp
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=firstapp
  Containers:
   frontend:
    Image:        nginx:latest
    Port:         80/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
   debian:
    Image:      debian
    Port:       <none>
    Host Port:  <none>
    Command:
      sleep
      3600
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Progressing    True    NewReplicaSetAvailable
  Available      True    MinimumReplicasAvailable
OldReplicaSets:  <none>
NewReplicaSet:   firstapp-65c5fd89c5 (1/1 replicas created)
Events:          <none>
```

```
rimm@cp1:~$ kubectl create -f pg.yaml
statefulset.apps/postgresql created
service/postgresql created                                                                                                                          
rimm@cp1:~$ kubectl get statefulsets.apps
NAME         READY   AGE
postgresql   1/1     11s
```

```
rimm@cp1:~$ kubectl get service
NAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
kubernetes   ClusterIP   10.233.0.1     <none>        443/TCP    4d
postgresql   ClusterIP   10.233.52.56   <none>        5432/TCP   2m17s
```
```
rimm@cp1:~$ kubectl describe statefulsets.apps postgresql
Name:               postgresql
Namespace:          default
CreationTimestamp:  Tue, 26 Apr 2022 22:56:11 +0000
Selector:           app=postgresql
Labels:             <none>
Annotations:        <none>
Replicas:           1 desired | 1 total
Update Strategy:    RollingUpdate
  Partition:        0
Pods Status:        1 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=postgresql
  Containers:
   postgresql:
    Image:      postgres:13-alpine
    Port:       <none>
    Host Port:  <none>
    Environment:
      POSTGRES_PASSWORD:  postgres
      POSTGRES_USER:      postgres
      POSTGRES_DB:        news
    Mounts:               <none>
  Volumes:                <none>
Volume Claims:            <none>
Events:
  Type    Reason            Age    From                    Message
  ----    ------            ----   ----                    -------
  Normal  SuccessfulCreate  2m55s  statefulset-controller  create Pod postgresql-0 in StatefulSet postgresql successful
```

## Задание 2: подготовить конфиг для production окружения
### Конфигурация фронтенда - [frontend.yaml](frontend.yaml), бэкенда - [backend.yaml](backend.yaml) Конфигурация для БД - [bd.yaml](bd.yaml)
```
rimm@cp1:~$ kubectl get deployments.apps
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
firstapp     1/1     1            1           4d21h
prod-back    1/1     1            1           19s
prod-front   1/1     1            1           13m
```

```
rimm@cp1:~$ kubectl get service
NAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
kubernetes   ClusterIP   10.233.0.1     <none>        443/TCP          4d23h
postgresql   NodePort    10.233.28.76   <none>        5432:31974/TCP   60m
prod-back    NodePort    10.233.47.30   <none>        80:31456/TCP     9m49s
prod-front   NodePort    10.233.4.111   <none>        8080:32041/TCP   15m
```
