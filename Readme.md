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