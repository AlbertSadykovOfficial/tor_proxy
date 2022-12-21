# TOR Proxy Server

## Ссылка на проект (и более подробную документацию):
https://hub.docker.com/r/dperson/torproxy

## Ссылка на пример kubernetes:
Использование:
https://spacelift.io/blog/kubernetes-tutorial
Файл:
https://kubernetes.io/docs/concepts/workloads/controllers/deployment/

## Использование:
### Запуск (yaml)
```sh
	kubectl apply -f docker-compose.yaml
```

### Запуск
1 экземпляр:
```sh
	kubectl run tor-proxy --image dperson/torproxy
```

3 экземпляра:
```sh
	kubectl create deployment tor-proxy --image dperson/torproxy --replicas 3
```

### Масштабирование
Расширить до 5 экземпляров:
```sh
	kubectl scale deployment tor-proxy --replicas 5
```

### Экспорт портов
#### Открытие порта
```sh
	kubectl expose deployment/tor-proxy --port 8118 --type NodePort
```
#### Проброс на другой порт (Пробрасываем наверх 80)
Через сервис:
```sh
	kubectl port-forward service/tor-proxy 80:8118
```
Напрямую:
```sh
	kubectl port-forward deployment/tor-proxy 80:8118
```

## Проверка
Посмотреть поды:
```sh
	kubectl get pods
```

Посмотреть сервисы:
```sh
	kubectl get services
```

ip-address - адрес, где запущен прокси
```sh
	curl -Lx http://<ip-address>:8118  https://icanhazip.com
```

## Удаление
```sh
	kubectl delete service tor-proxy
```