# Плейбук ElKi

## Назначение
Устанавливает Elastiсsearch и Kibana в docker-окружении.

## Параметры

Вы можете задать необходимы для установки версии через параметры:
* java_jdk_version - версия JDK
* elastic_version  - версия elastiсsearch
* kibana_version - версия kibana

## Теги

* java
* elastic
* kibana

## Известные проблемы

Вы можете указать желаемую версию java, но установлена будет всё-равно 11.0.16.1

## Окружение

Вы можете запустить docker-окружение командами:

> docker run --name elastik -d centos:7 /bin/bash -c "sleep 2d"
> 
> docker run --name kibana -d centos:7 /bin/bash -c "sleep 2d"

