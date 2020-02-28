[English version](README.en.md) is also available.

## Что это?
Набор скриптов для того, чтобы быстро поднять простенький кластер Kubernetes.
Cеть может быть Flannel или Calico, на выбор.

Окружение:
* виртуальная машина (VirtualBox, но сойдет любой способ)
* Ubuntu 18.04

Подразумевается, что у каждой виртуальной машины есть два сетевых интерфейса:
* NAT (для доступа в Internet)
* Host-only (для общения с другими машинами и нашей хостовой машиной)

Можно использовать и один интерфейс, если машины могут видеть на нем друг друга и иметь доступ в интернет (NAT Network в Virtual Box).

## Создание кластера

1. Устанавливаем необходимые пакеты и закрепляем их версии

`sudo ./install`

2. Создаем кластер. В качестве аргумента нужно передать имя интерфейса (обычно это Host-only):
* на нем будет слушать Kubernetes API server
* он будет использоваться для общения между нодами

Чтобы посмотреть работающие интерфейсы, просто запускаем без параметров

`sudo ./init`

Чтобы инициализировать кластер, запускаем с параметром:

`sudo ./init enp0s8`

**На этой стадии конфигурационный файл для kubectl будет скопирован в `~/.kube/config`, поэтому если в этом файле есть что-то важное, сохраните его.**

3. Предыдущая стадия подготовит манифесты сетевых плагинов, нужно просто выбрать, какой из них установить.

### Для Flannel

`kubectl apply -f network/flannel.yml`

### Для Calico

`kubectl apply -f network/calico.yaml`


## Присоединение ноды

1. Устанавливаем необходимые пакеты и закрепляем их версии

`sudo ./install`

2. Чтобы присоединить ноду к кластеру нужно вызвать скрипт `./join` и передать ему два параметра:
* имя интерфейса для общения между нодами
* endpoint, на котором слушает Kubernetes API server

Чтобы посмотреть работающие интерфейсы, просто запускаем без параметров

`sudo ./join`

Чтобы присоединитсья, запускаем с параметрами:

`sudo ./join enp0s8 192.168.56.110:6443`