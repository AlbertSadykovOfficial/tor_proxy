# TOR Proxy Server

## Ссылка на проект (и более подробную документацию):
https://hub.docker.com/r/dperson/torproxy

## Использование:
### Запуск
```sh
	sudo docker run -it -p 8118:8118 -p 9050:9050 -d dperson/torproxy
```

### Проверка
ip-address - адрес, где запущен прокси
```sh
	curl -Lx http://<ip-address>:8118  https://icanhazip.com
```