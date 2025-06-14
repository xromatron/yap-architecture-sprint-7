# Задание 4. Защита доступа к кластеру Kubernetes

## 1. Поднимите пустой Minikube. 

```shell
    minikube start
```

## 2. Определите все роли и их полномочия при работе с Kubernetes.

[Таблица](Роли-Полномочия-Пользователи.md)

## 3. Подготовьте скрипты для создания пользователей. 

Рекомендуем создать не менее двух пользователей.

```shell
    kubectl apply -f users.yaml
```

```shell
    kubectl get serviceaccounts
```

## 4. Подготовьте скрипты, чтобы создать роли. 

Они должны соответствовать ролям из вашей таблицы.

```shell
    kubectl apply -f clusterroles.yaml
```

```shell
     sh -c "kubectl get clusterroles.rbac.authorization.k8s.io | grep -v system"
```

## 5. Подготовьте скрипты, чтобы связать пользователей с ролями.

```shell
    kubectl apply -f clusterrolebindings.yaml
```

```shell
     sh -c "kubectl get clusterrolebindings | grep -v system"
```
