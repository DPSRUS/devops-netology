# Домашнее задание к занятию "4.2. Использование Python для решения типовых DevOps задач"

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
| Какое значение будет присвоено переменной `c`?  | Значение присвоено не будет, так как складываем число с символом. |
| Как получить для переменной `c` значение 12?  | a = '1'  |
| Как получить для переменной `c` значение 3?  | b = 2  |

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
#!/usr/bin/env python3

import os

bash_command = ["cd ~/dps/", "pwd", "git status"] 
result_os = os.popen(' && '.join(bash_command)).read() 
wdir = result_os.split('\n')
for result in result_os.split('\n'): 
    if result.find('modified') != -1:
        prepare_result = wdir[0]+'/'+result.replace('\tmodified:   ', '')
        print(prepare_result)
```

### Вывод скрипта при запуске при тестировании:
```
dps@Dps:~20:29:28 j1 $ ./test.py 
/home/dps/git_py/test.txt
```

## Обязательная задача 3
1. Доработать скрипт выше так, чтобы он мог проверять не только локальный репозиторий в текущей директории, а также умел воспринимать путь к репозиторию, который мы передаём как входной параметр. Мы точно знаем, что начальство коварное и будет проверять работу этого скрипта в директориях, которые не являются локальными репозиториями.

### Ваш скрипт:
```python
#!/usr/bin/env python3

import os
import sys

bash_command = ["cd "+sys.argv[1], "pwd", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
wdir = result_os.split('\n')
   if result.find('modified') != -1:
      prepare_result = wdir[0]+'/'+result.replace('\tmodified:   ', '')
      print(prepare_result)

```

### Вывод скрипта при запуске при тестировании:
```
dps@Dps:~20:58:29 j1 $ ./test.py ./git_py/
/home/dps/git_py/test.txt
```

## Обязательная задача 4
1. Наша команда разрабатывает несколько веб-сервисов, доступных по http. Мы точно знаем, что на их стенде нет никакой балансировки, кластеризации, за DNS прячется конкретный IP сервера, где установлен сервис. Проблема в том, что отдел, занимающийся нашей инфраструктурой очень часто меняет нам сервера, поэтому IP меняются примерно раз в неделю, при этом сервисы сохраняют за собой DNS имена. Это бы совсем никого не беспокоило, если бы несколько раз сервера не уезжали в такой сегмент сети нашей компании, который недоступен для разработчиков. Мы хотим написать скрипт, который опрашивает веб-сервисы, получает их IP, выводит информацию в стандартный вывод в виде: <URL сервиса> - <его IP>. Также, должна быть реализована возможность проверки текущего IP сервиса c его IP из предыдущей проверки. Если проверка будет провалена - оповестить об этом в стандартный вывод сообщением: [ERROR] <URL сервиса> IP mismatch: <старый IP> <Новый IP>. Будем считать, что наша разработка реализовала сервисы: `drive.google.com`, `mail.google.com`, `google.com`.

### Ваш скрипт:
```python
#!/usr/bin/env python3

from socket import gethostbyname

srv = {'drive.google.com': '0.0.0.0', 'mail.google.com': '0.0.0.0', 'google.com': '0.0.0.0'}
i = 50
init = 0

while 1==1:
   for host in srv:
     ip = gethostbyname(host)
     print (host + " : " + ip)
     if ip != srv[host]:
      if i == 1 and init != 1:
      print(str(' [ERROR] ' + str(host) + ' IP mistmatch: ' + srv[host] + ' ' + ip))
     srv[host] = ip
```

### Вывод скрипта при запуске при тестировании:
```
google.com : 173.194.221.100
drive.google.com : 142.251.1.194
mail.google.com : 216.58.212.165

[ERROR] drive.google.com IP mistmatch: 0.0.0.0 173.194.73.194
[ERROR] mail.google.com IP mistmatch: 0.0.0.0 64.233.165.83
[ERROR] google.com IP mistmatch: 0.0.0.0 173.194.221.139
[ERROR] mail.google.com IP mistmatch: 64.233.165.83 64.233.165.19
[ERROR] mail.google.com IP mistmatch: 64.233.165.19 64.233.165.83
[ERROR] google.com IP mistmatch: 173.194.221.139 173.194.222.101
[ERROR] google.com IP mistmatch: 173.194.222.101 173.194.222.100
[ERROR] google.com IP mistmatch: 173.194.222.100 64.233.164.139
[ERROR] google.com IP mistmatch: 64.233.164.139 64.233.164.138
[ERROR] drive.google.com IP mistmatch: 173.194.73.194 108.177.14.194
[ERROR] drive.google.com IP mistmatch: 108.177.14.194 173.194.73.194
```
