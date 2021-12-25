Домашнее задание 
1.  vagrant@vagrant:~$ strace /bin/bash -c 'cd /tmp' 2> strace
    cat strace | grep tmp
    chdir("/tmp") - системный вызов chdir, fchdir - изменить рабочий каталог

2.  file использует файлы:
    openat(AT_FDCWD, "/etc/magic", O_RDONLY) = 3
    openat(AT_FDCWD, "/usr/share/misc/magic.mgc", O_RDONLY) = 3

3.  vagrant@vagrant:~$ ping 8.8.8.8 > ping_test_del &
    vagrant@vagrant:~$ sudo ls -l /proc/1053/fd/
        total 0
        lrwx------ 1 root root 64 Dec 25 17:00 0 -> /dev/pts/1
        l-wx------ 1 root root 64 Dec 25 17:00 1 -> /home/vagrant/ping_test_del
        lrwx------ 1 root root 64 Dec 25 17:00 2 -> /dev/pts/1
        lrwx------ 1 root root 64 Dec 25 17:00 3 -> 'socket:[27687]'
        lrwx------ 1 root root 64 Dec 25 17:00 4 -> 'socket:[27688]'
    vagrant@vagrant:~$ rm ping_test_del 
    vagrant@vagrant:~$ sudo cat /proc/1053/fd/1 > /home/vagrant/recovery_ping
    vagrant@vagrant:~$ cat recovery_ping 
        PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
        64 bytes from 8.8.8.8: icmp_seq=1 ttl=63 time=7.72 ms
        64 bytes from 8.8.8.8: icmp_seq=2 ttl=63 time=7.58 ms

4.  зомби-процессы не занимают ресурсы (CPU,RAM,IO), но они использует слот в таблице процессов ядра, а при заполнении таблицы, будет невозможно создать новые процессы.
5.  sudo apt-get install bpfcc-tools linux-headers-$(uname -r) - установил необходимый пакет.
    root@vagrant:/usr/sbin# opensnoop-bpfcc - запустил 
    PID    COMM               FD ERR PATH
    784    vminfo              4   0 /var/run/utmp
    579    dbus-daemon        -1   2 /usr/local/share/dbus-1/system-services
    579    dbus-daemon        18   0 /usr/share/dbus-1/system-services
    579    dbus-daemon        -1   2 /lib/dbus-1/system-services

6.  uname -a - использует системный вызов uname().  
    из man umame(2):
        Part of the utsname information is also accessible via /proc/sys/kernel/{ostype, hostname, osrelease, version,domainname}

7.  ; - разделитель команд. Команды отработают по очереди при любых результатах.
    && - выполнит вторую каманду, если первая отработала без ошибок.
    set -e прерывает выполнение команды, если команда завершается с ненулевым статусом. Использовать &&, если применить set -e смысла нет.

8.  set -euxo pipefail состоит из опций:
        -e прерывает выполнение команды, если команда завершается с ненулевым статусом.
        -u прерывает выполнение команды если неустановленные или неопределенные переменные.
        -x выводит аргументы команды во время выполнения.
        -o устанавливает опцию - pipefail
        опция pipefail - прекращает выполнение скрипта, даже если одна из частей пайпа завершилась ошибкой.

9.  vagrant@vagrant:~$ ps -A -o stat | sort | uniq -c
      8 I
     40 I< - наиболее часто встречающийся.
      1 R+
     24 S  - наиболее часто встречающийся.
      2 S+
      1 S<s
      1 SLsl
      2 SN
      1 STAT
      1 Sl
     14 Ss
      1 Ss+
      4 Ssl
      Доп. символы к статутсу процесса: 
      + - в группе процессов так называемого "переденего плана", которые выводят информацию в tty.
      < - высокий приоритет у процесса.
      s - лидер сеанса.
      l - является многопоточным.

