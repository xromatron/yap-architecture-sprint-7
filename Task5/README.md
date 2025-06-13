# Задание 5. Управление трафиком внутри кластера Kubertnetes

```shell
    minikube start
```

Проверим текущий namespace
```shell
    sh -c "kubectl config view | grep namespace"
```

Создадим новый для развертывания подов
```shell
    kubectl create namespace yap7
```

Переключимся на него
```shell
    kubectl config set-context $(kubectl config current-context) --namespace=yap7
```

Развернем сервисы
```shell
    kubectl run front-end-app --image=nginx --labels role=front-end --expose --port 80
    kubectl run back-end-api-app --image=nginx --labels role=back-end-api --expose --port 80
    kubectl run admin-front-end-app --image=nginx --labels role=admin-front-end --expose --port 80
    kubectl run admin-back-end-api-app --image=nginx --labels role=admin-back-end-api --expose --port 80
```

Проверим что все запустились
```shell
    kubectl get po
```

Можно запустить dashboard // и выбрать namespace yap7
```shell
    minikube dashboard
```

Проверим что до применения политик доступ есть
```shell
    kubectl run test-$RANDOM --rm -i -t --image=alpine -- sh
```
В терминале введем
```shell
    wget -qO- --timeout=2 http://front-end-app
    wget -qO- --timeout=2 http://back-end-api-app
    wget -qO- --timeout=2 http://admin-front-end-app
    wget -qO- --timeout=2 http://admin-back-end-api-app
```

Выполним проверку доступа из front-end-app к back-end-api-app
```shell
    kubectl exec -it front-end-app -- sh
```

В терминале введем
```shell
    curl http://back-end-api-app
```

Применить сетевую политику
```shell
    kubectl apply -f non-admin-api-allow.yaml
```

Выполним проверку доступа из admin-front-end-app к back-end-api-app
```shell
    kubectl exec -it admin-front-end-app -- sh
```

В терминале введем
```shell
    curl http://back-end-api-app
```

