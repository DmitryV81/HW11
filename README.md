Домашняя работа по теме "Первые шаги с Ansible"

Задание:

Подготовить стенд на Vagrant как минимум с одним сервером. На этом сервере используя Ansible необходимо развернуть nginx со следующими условиями:

- необходимо использовать модуль yum/apt;
 
- конфигурационные файлы должны быть взяты из шаблона jinja2 с перемененными;

- после установки nginx должен быть в режиме enabled в systemd;

- должен быть использован notify для старта nginx после установки;

- сайт должен слушать на нестандартном порту - 8080, для этого использовать переменные в Ansible.



Ход выполнения домашнего задания.

Организуем для проекта файлы и директории

Ниже представлен вывод команды tree:

```
.
├── group_vars
│   └── all
├── hosts
├── host_vars
│   └── webserver
├── README.md
├── roles
│   └── nginx
│       ├── defaults
│       ├── files
│       ├── handlers
│       │   └── main.yml
│       ├── library
│       ├── lookup_plugins
│       ├── meta
│       ├── module_utils
│       ├── tasks
│       │   └── main.yml
│       ├── templates
│       │   └── nginx.j2
│       └── vars
├── site.yml
└── Vagrantfile

14 directories, 9 files
```
1. site.yml - файл, в формате yml, в которомописаны хосты, над которые будут сконфигурированы, права root (нужны или нет) и роли

2. Каталог roles содержит описание одной роли - nginx

3. В каталоге host_vars содержится сопоставление имени хоста и ip-адреса

4. Файл hosts содержит имя роли и хост к которому эта роль будет применена

5. В каталоге group_vars содержатся переменные, которые используются в работе данного проекта

6. Файл Vagrantfile - создание виртуальной машины и запуск ansible в provision. Для запуска используем команду 

```
vagrant up
```

* Можно не использовать Vagrant, тогда для запуска ansible необходимо внести изменения в файл host_vars/webserver, прописав в нём ip-адрес хоста, над которым будут выполняться действия, и, выполнить команду:
```
ansible-playbook -i hosts site.yml
```

Подробнее о содержимом каталога roles:

Основной файл, содержащий команды по установке, настройке и запуску nginx расположен по пути nginx/tasks/main.yml

С помощью него производится установка, копирование конфигурационного файла nginx (используется шаблон jinja2), перезапуск nginx с использованием notify. И запуск службы nginx.

Конфигурационный файл nginx расположен по пути: nginx/templates/nginx.j2

В него подставляются значения переменных  listen_port и servername

По пути nginx/handlers/main.yml расположен файл для работы notify - перезапуск nginx

В результате работы мы получили виртуальную машину с установленым и настроенным на работу на порту 8080 nginx:

![Результат работы](https://github.com/DmitryV81/HW11/blob/main/screenshot.png)
