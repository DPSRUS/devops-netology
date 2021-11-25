Домашнее задание 
1.  Установил VirtualBox - sudo apt install virtualbox
2.  Добавил репозиторий и установил vagrant:
    curl:-fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
    sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
    sudo apt-get update && sudo apt-get install vagrant
3.  Добавил в PS1= \t — вывод текущего времени, j\j —  вывод количество запущенных job. Итог: ~/Vagrant 21:06:28 j0 $ 
4.  Ubuntu 20.04 в VirtualBox посредством Vagrant запустил.
5.  Ресурсы по умолчанию:
    Память: 1024Mb.
    Процессор: 2 ядра.
    Жесткий диск: 64Gb, фактически занято 2Gb.
6.  Добавить ресурсов процессора и памяти можно через конфигурационный файл vagrant или графический интерфейс virtualbox
    config.vm.provider "virtualbox" do |v|
    v.memory = 1024
    v.cpus = 2
    end
7.  Попрактиковался.
8.  Размер истории определяется с помощью переменной HISTSIZE = , строка 643 в man bash.
    ignoreboth - параметр для переменной HISTCONTROL чтобы игнорировалась запись команд которые начинаются с пробела и повторяющиеся строки.
9.  {} относится к зарезервированым словам, блоки инструкций, массивы, циклы. строка 139 в man bash.
10. Создать 100000 файлов - touch test_{1..100000}
    Создать 300000 файлов не удается из-за ошибки "argument list too long touch", команда превышает предел "ARG_MAX" - Максимальная длина аргументов функций exec*()     в байтах вместе с данными среды.
11. [[ ]]  Возвращает статус 0 или 1. [[ -d /tmp ]] - проверяет есть ли файл /tmp и являтеся ли он директорией. Можно использовать в скриптах.
12. mkdir /tmp/new_path_directory/bash
    type -a bash - узанал где bash
    cp /bin/bash /tmp/new_path_directory/bash/ - скопировал программу.
    sudo nano /etc/environment -добавил каталог /tmp/new_path_directory/bash/ в переменное окружение.
    type -a bash - проверил вывод.
13.at - выполняет команду в указанное время, batch - выполняется когда средняя загрузка системы ниже 1.5 или указаном значение.
14. vagrant halt - выключить машину.
