# devops-netology_4.2
## Обязательная задача 1

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
| Какое значение будет присвоено переменной `c`?  |  TypeError: unsupported operand type(s) for +: 'int' and 'str' |
| Как получить для переменной `c` значение 12?  | print(str(a) + b)|
| Как получить для переменной `c` значение 3?  | print(a + int(b))|

## Обязательная задача 2
Мы устроились на работу в компанию, где раньше уже был DevOps Engineer. Он написал скрипт, позволяющий узнать, какие файлы модифицированы в репозитории, относительно локальных изменений. Этим скриптом недовольно начальство, потому что в его выводе есть не все изменённые файлы, а также непонятен полный путь к директории, где они находятся. Как можно доработать скрипт ниже, чтобы он исполнял требования вашего руководителя?

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
```python
import os

bash_command = ["cd ~/netology/sysadm-homeworks", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(path + prepare_result)
```

### Вывод скрипта при запуске при тестировании:
```
/home/lsergeyo/netology/sysadm-homeworks/test1
/home/lsergeyo/netology/sysadm-homeworks/test2
```

## Обязательная задача 3
1. Доработать скрипт выше так, чтобы он мог проверять не только локальный репозиторий в текущей директории, а также умел воспринимать путь к репозиторию, который мы передаём как входной параметр. Мы точно знаем, что начальство коварное и будет проверять работу этого скрипта в директориях, которые не являются локальными репозиториями.

### Ваш скрипт:
```python
import os   
    
print("Введите путь к репозиторию)
path = input()

os.chdir(path)
bash_command = ["git status"]
result_os = os.popen(' && '.join(bash_command)).read()
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(path+prepare_result)
```

### Вывод скрипта при запуске при тестировании:
```
Введите путь к репозиторию
/lsergeyo/netology/sysadm-homeworks/
/home/lsergeyo/netology/sysadm-homeworks/test1
/home/lsergeyo/netology/sysadm-homeworks/test2
```

## Обязательная задача 4
1. Наша команда разрабатывает несколько веб-сервисов, доступных по http. Мы точно знаем, что на их стенде нет никакой балансировки, кластеризации, за DNS прячется конкретный IP сервера, где установлен сервис. Проблема в том, что отдел, занимающийся нашей инфраструктурой очень часто меняет нам сервера, поэтому IP меняются примерно раз в неделю, при этом сервисы сохраняют за собой DNS имена. Это бы совсем никого не беспокоило, если бы несколько раз сервера не уезжали в такой сегмент сети нашей компании, который недоступен для разработчиков. Мы хотим написать скрипт, который опрашивает веб-сервисы, получает их IP, выводит информацию в стандартный вывод в виде: <URL сервиса> - <его IP>. Также, должна быть реализована возможность проверки текущего IP сервиса c его IP из предыдущей проверки. Если проверка будет провалена - оповестить об этом в стандартный вывод сообщением: [ERROR] <URL сервиса> IP mismatch: <старый IP> <Новый IP>. Будем считать, что наша разработка реализовала сервисы: `drive.google.com`, `mail.google.com`, `google.com`.

### Ваш скрипт:
```python
import socket

n = 0
addr = {"drive.google.com": socket.gethostbyname("drive.google.com"),
       "mail.google.com": socket.gethostbyname("mail.google.com"),
        "google.com": socket.gethostbyname("google.com")}
while n < 5:
    n = n + 1
    for host in addr:
        if addr[host] != socket.gethostbyname(host):
            print("[ERROR] " + str(host) + " IP mismatch: " + addr[host] + "--- " + socket.gethostbyname(host))
        addr[host] = socket.gethostbyname(host)
        print(host + " --- " + addr[host] + " OK")
```

### Вывод скрипта при запуске при тестировании:
```
drive.google.com --- 108.177.14.194 OK
mail.google.com --- 216.58.209.165 OK
google.com --- 216.58.210.174 OK
drive.google.com --- 108.177.14.194 OK
mail.google.com --- 216.58.209.165 OK
google.com --- 216.58.210.174 OK
drive.google.com --- 108.177.14.194 OK
mail.google.com --- 216.58.209.165 OK
google.com --- 216.58.210.174 OK
drive.google.com --- 108.177.14.194 OK
mail.google.com --- 216.58.209.165 OK
google.com --- 216.58.210.174 OK
drive.google.com --- 108.177.14.194 OK
mail.google.com --- 216.58.209.165 OK
google.com --- 216.58.210.174 OK

```