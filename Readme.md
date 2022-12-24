# TOR Proxy Server

## Ссылка на проект (и более подробную документацию):
https://hub.docker.com/r/dperson/torproxy

## Параметры Тор:
```
	https://2019.www.torproject.org/docs/tor-manual.html.en
```

## Устранение неполадок
Если будет bootstraping error - это значит, тор не может найти мостовые узлы, для
устранения этой пролемы следует перейти на сайт тор и получить адреса мостов,
после чего внести их в конфигурационный файл:

```
	https://bridges.torproject.org/options/
```

```environments
	TOR_UseBridges=1
	TOR_bridge=135.23.182.26:11393
	TOR_bridge=23.123.207.136:443
```

## Использование:
### Запуск

Инициализация Swarm-менеджера
```bash
	docker swarm init
```

Создадим приложение (без переменных окружения)
(Будут созданы и запущены: сеть и сервисы)
```bash
	docker stack deploy --compose-file docker-compose.yaml tor-proxy-manager
```

Создадим приложение (с переменными окружения) 
Переменные окружения теперь не считываются с файла и их нужно передавать вручную:
```bash
	env $(cat .settings | xargs) docker stack deploy --compose-file docker-compose.yaml tor-proxy-manager
```

Посмотреть контейнеры приложения:
```bash
	docker stack services tor-proxy-manager
```

### Проверка
ip-address - адрес, где запущен прокси
```sh
	curl -Lx http://<ip-address>:8118  https://icanhazip.com
```

### Остановка и Удаление
Удаление приложения
```bash
	docker stack rm tor-proxy-manager
```

Отключение менеджера (управляющего роем) 
```bash
	docker swarm leave --force
```