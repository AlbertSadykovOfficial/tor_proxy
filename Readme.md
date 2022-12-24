# TOR Proxy Server

## Ссылка на проект (и более подробную документацию):
https://hub.docker.com/r/dperson/torproxy

## Параметры Тор:
Установим обновление ip адреса - кажую минуту с проверкой текущего времени каждые 15 секунд:
```
	TOR_MaxCircuitDirtiness=60
	TOR_NewCircuitPeriod=15
```
Указываем вручную мостовые узлы, чтобы без проблем подсоединиться к сети (для получения актуальных
узлов, следует пеерйти на сайт проекта тор - https://bridges.torproject.org/options/):
```
	TOR_UseBridges=1
	TOR_bridge=135.23.182.26:11393
	TOR_bridge=23.123.207.136:443
```

Все параметры Тор:
```
	https://2019.www.torproject.org/docs/tor-manual.html.en
```

## Устранение неполадок
Если будет bootstrapping error - это значит, тор не может найти мостовые узлы, для
устранения этой пролемы следует перейти на сайт тор и получить адреса мостов,
после чего внести их в конфигурационный файл:

```
	https://bridges.torproject.org/options/
```

```
	TOR_UseBridges=1
	TOR_bridge=135.23.182.26:11393
	TOR_bridge=23.123.207.136:443
```

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