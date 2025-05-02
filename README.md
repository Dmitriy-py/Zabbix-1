# Домашнее задание к занятию «Система мониторинга Zabbix»

# Климов Дмитрий

## Задание 1

Установите Zabbix Server с веб-интерфейсом.

Процесс выполнения
1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2. Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитороии Debian 11.
3. Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.
4. Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.


## Ответ

Vagrantfile:
 
```
# -*- mode: ruby -*-
# vi: set ft=ruby :

HOST_NAME = 'zabbix'
HOST_IP = '192.168.0.127'
HOST_USER = 'test'
HOST_USER_PASS = '123456789'
HOST_UPGRADE = 'true'
ZABBIX_DB_PASS = '1234567890'
HOST_SHOW_GUI = false
HOST_MEMMORY = "1024"
HOST_CPUS = 1
HOST_BRIDGE = "en0" # Замените на имя вашего сетевого адаптера
HOST_VM_BOX = "ubuntu/jammy64" # Ubuntu 22.04

# Используйте новый скрипт для Ubuntu
HOST_CONFIIG_SCRIPT = "zabbix-server-ubuntu.sh"

Vagrant.configure("2") do |config|
  config.vm.network "public_network", bridge: HOST_BRIDGE
  config.vm.box = HOST_VM_BOX
  config.vm.define HOST_NAME do |machine|
    machine.vm.network :public_network, ip: HOST_IP
    machine.vm.provider "virtualbox" do |current_vm|
      current_vm.name = HOST_NAME
      current_vm.gui = HOST_SHOW_GUI
      current_vm.memory = HOST_MEMMORY
      current_vm.cpus = HOST_CPUS
    end
  end
  config.vm.provision "shell", path: HOST_CONFIIG_SCRIPT, args: [HOST_USER, HOST_USER_PASS, ZABBIX_DB_PASS, HOST_UPGRADE]
end

```
