# Домашнее задание к занятию "4.1. Командная оболочка Bash: Практические навыки"  
### 1. Есть скрипт:
	
	a=1
	b=2
	c=a+b
	d=$a+$b
	e=$(($a+$b))
Какие значения переменным c,d,e будут присвоены? Почему? 
` echo $c $d $e`
	
	$c - a+b - в переменную записали строку a+b.
	$d - 1+2 - в переменную записали значение переменной a, символ +, значение переменной b.
	$e - 3 - в переменную записали сложение переменной a и b.
	
### 2. На нашем локальном сервере упал сервис и мы написали скрипт, который постоянно проверяет его доступность, записывая дату проверок до тех пор, пока сервис не станет доступным. В скрипте допущена ошибка, из-за которой выполнение не может завершиться, при этом место на Жёстком Диске постоянно уменьшается. Что необходимо сделать, чтобы его исправить:
	
	while ((1==1)
	do
	curl https://localhost:4757
	if (($? != 0))
	then
	date >> curl.log
	fi
	done
Решение:
	
	#!/usr/bin/env bash
	while [ 1==1 ]
	do
        curl https://localhost:4757
        if (($? != 0))
        then
        date >> curl.log
        else
        break
        fi
	done
		
### 3. Необходимо написать скрипт, который проверяет доступность трёх IP: 192.168.0.1, 173.194.222.113, 87.250.250.242 по 80 порту и записывает результат в файл log. Проверять доступность необходимо пять раз для каждого узла.

Скрипт для проверки:

	#!/usr/bin/env bash
	for IP in 192.168.0.1 173.194.222.113 87.250.250.242
        do
        for (( n = 1; n <= 5; n++ ))
        do 
        nc -zvw1 $IP 80 &>> telnet.log
        done
        done     
Рельтат проверки:
	
	nc: connect to 192.168.0.1 port 80 (tcp) timed out: Operation now in progress
	nc: connect to 192.168.0.1 port 80 (tcp) timed out: Operation now in progress
	nc: connect to 192.168.0.1 port 80 (tcp) timed out: Operation now in progress
	nc: connect to 192.168.0.1 port 80 (tcp) timed out: Operation now in progress
	nc: connect to 192.168.0.1 port 80 (tcp) timed out: Operation now in progress
	Connection to 173.194.222.113 80 port [tcp/http] succeeded!
	Connection to 173.194.222.113 80 port [tcp/http] succeeded!
	Connection to 173.194.222.113 80 port [tcp/http] succeeded!
	Connection to 173.194.222.113 80 port [tcp/http] succeeded!
	Connection to 173.194.222.113 80 port [tcp/http] succeeded!
	Connection to 87.250.250.242 80 port [tcp/http] succeeded!
	Connection to 87.250.250.242 80 port [tcp/http] succeeded!
	Connection to 87.250.250.242 80 port [tcp/http] succeeded!
	Connection to 87.250.250.242 80 port [tcp/http] succeeded!
	Connection to 87.250.250.242 80 port [tcp/http] succeeded!

### 4. Необходимо дописать скрипт из предыдущего задания так, чтобы он выполнялся до тех пор, пока один из узлов не окажется недоступным. Если любой из узлов недоступен - IP этого узла пишется в файл error, скрипт прерывается
	#!/usr/bin/env bash
	while  (( 1 == 1 ))
	do
        for ip in 192.168.56.12 10.0.2.15
        do
        nc -zvw3 $ip 80 2> error_ip.log 
        done
	if (($? == 0))
        then
        sleep 2
        else
        date  >> error_ip.log
        break
        fi
	done
