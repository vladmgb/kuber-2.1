# Домашнее задание к занятию «Хранение в K8s»

## Задание 1. Volume: обмен данными между контейнерами в поде
Задача
Создать Deployment приложения, состоящего из двух контейнеров, обменивающихся данными.

Манифест [Deployment](https://github.com/vladmgb/kuber-2.1/blob/main/containers-data-exchange.yaml)

Описание пода с контейнерами.

```

Name:             data-exchange-58fb54f789-rgwkg
Namespace:        default
Priority:         0
Service Account:  default
Node:             my-k8s/10.129.0.21
Start Time:       Thu, 25 Sep 2025 18:57:22 +0000
Labels:           app=data-exchange
                  pod-template-hash=58fb54f789
Annotations:      cni.projectcalico.org/containerID: 0582cd72cd39a72f86e0405c8f0ea78910111710d459b5b95cb5d3d1aec6f982
                  cni.projectcalico.org/podIP: 10.1.21.155/32
                  cni.projectcalico.org/podIPs: 10.1.21.155/32
Status:           Running
IP:               10.1.21.155
IPs:
  IP:           10.1.21.155
Controlled By:  ReplicaSet/data-exchange-58fb54f789
Containers:
  busybox:
    Container ID:  containerd://8d6f86b546322c6b94a71ebc9988b9abb1132924b6e251444c2a34a443a5b586
    Image:         busybox
    Image ID:      docker.io/library/busybox@sha256:d82f458899c9696cb26a7c02d5568f81c8c8223f8661bb2a7988b269c8b9051e
    Port:          <none>
    Host Port:     <none>
    Command:
      /bin/sh
      -c
    Args:
      while true; do date >> /exchange/shared.txt; sleep 5; done;
    State:          Running
      Started:      Thu, 25 Sep 2025 18:57:24 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /exchange from exchange-vol (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-wqsbm (ro)
  multitool:
    Container ID:  containerd://e4d53b0626ae2e7cb730d867479b63e44f380d6071def2a7b7735ffc4bd27568
    Image:         wbitt/network-multitool
    Image ID:      docker.io/wbitt/network-multitool@sha256:d1137e87af76ee15cd0b3d4c7e2fcd111ffbd510ccd0af076fc98dddfc50a735
    Port:          <none>
    Host Port:     <none>
    Command:
      /bin/sh
      -c
    Args:
      tail -f /exchange/shared.txt
    State:          Running
      Started:      Thu, 25 Sep 2025 18:57:25 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /exchange from exchange-vol (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-wqsbm (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True
  Initialized                 True
  Ready                       True
  ContainersReady             True
  PodScheduled                True
Volumes:
  exchange-vol:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:
    SizeLimit:  <unset>
  kube-api-access-wqsbm:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  4m20s  default-scheduler  Successfully assigned default/data-exchange-58fb54f789-rgwkg to my-k8s
  Normal  Pulling    4m20s  kubelet            Pulling image "busybox"
  Normal  Pulled     4m19s  kubelet            Successfully pulled image "busybox" in 923ms (923ms including waiting). Image size: 2223686 bytes.
  Normal  Created    4m19s  kubelet            Created container: busybox
  Normal  Started    4m19s  kubelet            Started container busybox
  Normal  Pulling    4m19s  kubelet            Pulling image "wbitt/network-multitool"
  Normal  Pulled     4m18s  kubelet            Successfully pulled image "wbitt/network-multitool" in 963ms (963ms including waiting). Image size: 25281012 bytes.
  Normal  Created    4m18s  kubelet            Created container: multitool
  Normal  Started    4m18s  kubelet            Started container multitool

```

Вывод команды чтения файла (tail -f <имя общего файла>).

<img width="773" height="285" alt="image" src="https://github.com/user-attachments/assets/e4f0b87a-e3ae-46fc-a505-2d752541a6fb" />



## Задание 2. PV, PVC

Задача
Создать Deployment приложения, использующего локальный PV, созданный вручную.

Шаги выполнения

1. Создать Deployment приложения, состоящего из контейнеров busybox и multitool, использующего созданный ранее PVC


2. Создать PV и PVC для подключения папки на локальной ноде, которая будет использована в поде.


3. Продемонстрировать, что контейнер multitool может читать данные из файла в смонтированной директории, в который busybox записывает данные каждые 5 секунд.



4. Удалить Deployment и PVC. Продемонстрировать, что после этого произошло с PV. Пояснить, почему. (Используйте команду kubectl describe pv).



5. Продемонстрировать, что файл сохранился на локальном диске ноды. Удалить PV. Продемонстрировать, что произошло с файлом после удаления PV. Пояснить, почему.


Что сдать на проверку

Манифесты:
pv-pvc.yaml
Скриншоты:
каждый шаг выполнения задания, начиная с шага 2.


## Задание 3. StorageClass
Задача
Создать Deployment приложения, использующего PVC, созданный на основе StorageClass.

Шаги выполнения

1. Создать Deployment приложения, состоящего из контейнеров busybox и multitool, использующего созданный ранее PVC.



2. Создать SC и PVC для подключения папки на локальной ноде, которая будет использована в поде.



3. Продемонстрировать, что контейнер multitool может читать данные из файла в смонтированной директории, в который busybox записывает данные каждые 5 секунд.


Что сдать на проверку

Манифесты:
sc.yaml
Скриншоты:
каждый шаг выполнения задания, начиная с шага 2
Описания:
объяснение наблюдаемого поведения ресурсов в двух последних шагах.
