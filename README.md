## Задание 1
Есть скрипт:

```python
#!/usr/bin/env python3
a = 1
b = '2'
c = a + b
```
### Вопросы:

| Вопрос  | Ответ |
| ------------- | ------------- |
| Какое значение будет присвоено переменной `c`?  | TypeError: unsupported operand type(s) for +: 'int' and 'str' |
| Как получить для переменной `c` значение 12?  | необходимо изменить тип a на str  |
| Как получить для переменной `c` значение 3?  | необходимо поменять тип b на int  |

![dz1](https://github.com/EVolgina/devops-netology16/blob/main/zd1-1.PNG)

![dz1-2](https://github.com/EVolgina/devops-netology16/blob/main/zd-2.PNG)

## Задание 2
Мы устроились на работу в компанию, где раньше уже был DevOps-инженер. Он написал скрипт, позволяющий узнать, какие файлы модифицированы в репозитории относительно локальных изменений. Этим скриптом недовольно начальство, потому что в его выводе есть не все изменённые файлы, а также непонятен полный путь к директории, где они находятся.
Как можно доработать скрипт ниже, чтобы он исполнял требования вашего руководителя?
```python
#!/usr/bin/env python3

import os

bash_command = ["cd ~/netology/sysadm-homeworks", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
        break
```

### Ваш скрипт:
В этом скрипте мы сначала меняем текущий рабочий каталог на репозиторий Git. Затем мы используем GitPython, чтобы получить список измененных файлов в репозитории. Наконец, мы просматриваем список измененных файлов и выводим их полные пути
```python
#./scripts/chek.py
import os
import subprocess
from git import Repo

#изменить каталог на GIT репозиторий
os.chdir('/home/vagrant/skripts/devops-netology16')   

# получаем статус Git репозитория используя GitPython
repo = Repo(os.getcwd())
modified_files = [item.a_path for item in repo.index.diff(None)]
for item in repo.index.diff(None):
    if item.change_type == 'M':
        modified_files.append(item.a_path)

# выводим измененные данные
for file in modified_files:
    print(os.path.join(os.getcwd(), file))

```

### Вывод скрипта при запуске во время тестирования:

![vivod](https://github.com/EVolgina/devops-netology16/blob/main/rez2.PNG)
![viv2](https://github.com/EVolgina/devops-netology16/blob/main/rez2-1.PNG)


## Задание 3
Доработать скрипт выше так, чтобы он не только мог проверять локальный репозиторий в текущей директории, но и умел воспринимать путь к репозиторию, который мы передаём, как входной параметр. Мы точно знаем, что начальство будет проверять работу этого скрипта в директориях, которые не являются локальными репозиториями.

### Ваш скрипт: 
В этом обновленном скрипте мы используем модуль argparse для анализа аргументов командной строки. Мы определяем единственный аргумент с именем path, который представляет путь к репозиторию Git. Затем мы меняем каталог на указанный путь и используем GitPython, чтобы получить список измененных файлов в репозитории. Наконец, мы просматриваем список измененных файлов и выводим их полные пути. обновленный скрипт, который принимает путь к репозиторию в качестве входного параметра:

```python
#./scripts/mod_chek.py

import os
import argparse
import subprocess
from git import Repo

# проанализировать аргументы
parser = argparse.ArgumentParser()
parser.add_argument('path', help='/home/vagrant/scrips/devops-netology16')
args = parser.parse_args()

# меняем катаалог на Git репозиторий
os.chdir(args.path)

# получаем статус Git репозитория используя GitPython
repo = Repo(os.getcwd())
modified_files = [item.a_path for item in repo.index.diff(None)]
for item in repo.index.diff(None):
    if item.change_type == 'M':
        modified_files.append(item.a_path)

# выводим список модифицированных файлов
for file in modified_files:
    print(os.path.join(os.getcwd(), file))
```
### Вывод скрипта при запуске во время тестирования:
![zd3vivod](https://github.com/EVolgina/devops-netology16/blob/main/zd3vivod.PNG)

## Задание 4
Наша команда разрабатывает несколько веб-сервисов, доступных по HTTPS. Мы точно знаем, что на их стенде нет никакой балансировки, кластеризации, за DNS прячется конкретный IP сервера, где установлен сервис.
Проблема в том, что отдел, занимающийся нашей инфраструктурой, очень часто меняет нам сервера, поэтому IP меняются примерно раз в неделю, при этом сервисы сохраняют за собой DNS-имена. Это бы совсем никого не беспокоило, если бы несколько раз сервера не уезжали в такой сегмент сети нашей компании, который недоступен для разработчиков.\
Мы хотим написать скрипт, который:\
опрашивает веб-сервисы;\
получает их IP;\
выводит информацию в стандартный вывод в виде: <URL сервиса> - <его IP>.\
Также должна быть реализована возможность проверки текущего IP сервиса c его IP из предыдущей проверки. Если проверка будет провалена — оповестить об этом в стандартный вывод сообщением: [ERROR] <URL сервиса> IP mismatch: <старый IP> <Новый IP>. Будем считать, что наша разработка реализовала сервисы: drive.google.com, mail.google.com, google.com.\

### Ваш скрипт: 
В этом скрипте  определяем словарь под названием services, который содержит URL-адреса веб-служб, которые мы хотим проверить. Затем мы определяем функцию с именем get_ip(), которая принимает URL-адрес в качестве входных данных и возвращает соответствующий IP-адрес. Мы перебираем словарь сервисов, получаем их текущие IP-адреса с помощью функции get_ip() и печатаем URL-адреса и IP-адреса. Также проверяем, изменился ли IP-адрес с момента последней проверки, и печатаем сообщение об ошибке, если это произошло.
```python
#./scripts/chek_ser.py

import socket

# определить словать сервисов и их URL
services = {
    'drive.google.com': '',
    'mail.google.com': '',
    'google.com': ''
}

# опеределить функцию для данного IP адреса для данного URL
def get_ip(url):
    return socket.gethostbyname(url)

# просмотр всех сервисов и получение их IP адресов
for name, url in services.items():
    ip = get_ip(name)

    # преверим изменился ли IP с последней проверки
    if url != '' and url != ip:
        print(f'[ERROR] {name} IP mismatch: {url} {ip}')

    # обновление URL с текущим IP
    services[name] = ip

    # вывод URL и IP адресов
    print(f'{name} - {ip}')

```
### Вывод скрипта при запуске во время тестирования:

![zd4](https://github.com/EVolgina/devops-netology16/blob/main/zd4vivvod.PNG)
