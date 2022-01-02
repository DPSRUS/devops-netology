Домашнее задание  
1. Node_exporter:  
   `wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz`  
   `tar xvzf node_exporter-1.3.1.linux-amd64.tar.gz`  
   `cp node_exporter /usr/local/`  
   `sudo useradd -rs /bin/false node_exporter`  
   `sudo nano /etc/systemd/system/node_exporter.service`  
    
        [Unit]
        Description=Node Exporter
        After=network-online.target
        
        [Service]
        User=node_exporter
        Group=node_exporter
        Type=simple
        ExecStart=/usr/local/bin/node_exporter $opts
        EnvironmentFile=-/usr/local/bin/node_exporter_opts - указываем файл переменными для опций запуска.
        
        [Install]
        WantedBy=multi-user.target  
   `sudo nano /usr/local/bin/node_exporter_opts` - для теста отключаем сборщики по умолчанию. 
   
   		opts=--collector.disable-defaults   
    
   `sudo systemctl daemon-reload`  
   `sudo systemctl start node_exporter`  
   `sudo systemctl enable node_exporter` - добавляем в атозапуск.  
   `sudo systemctl status node_exporter` - проверяем статус и доп. опцию запуска.
   
 	   ● node_exporter.service - Node Exporter  
  	   Loaded: loaded (/etc/systemd/system/node_exporter.service; disabled; vendor preset: enabled)  
    	 Active: active (running) since Sun 2022-01-02 15:09:05 UTC; 2s ago  
       Main PID: 2412 (node_exporter)  
       Tasks: 4 (limit: 1071)  
       Memory: 1.8M  
       CGroup: /system.slice/node_exporter.service  
               └─2412 /usr/local/bin/node_exporter --collector.disable-defaults - доп. опиция в файле.                 
2. Базовые опции для мониторинга хоста по CPU, памяти, диску и сети - cpu, cpufreq, diskstats, filesystem, meminfo, netstat  
3. Netdata  
	`sudo apt install -y netdata`  
  
  	 Vagrant.configure("2") do |config|
     	config.vm.box = "bento/ubuntu-20.04"
 		end
		 Vagrant::Config.run do |config|
	 	 config.vm.forward_port 9100, 8080 - проброс для Node_exporter
 		 config.vm.forward_port 19999, 19999 - проброс для Netdata
		end
	
  Просмотрел и изучил метрики http://localhost:19999/#menu_system_submenu_cpu;theme=slate
  
5. `dmesg | grep -i "virt"` - Пробуем понять используется ли виртуализация.  
		
  	 	[    0.000000] DMI: innotek GmbH VirtualBox/VirtualBox, BIOS VirtualBox 12/01/2006  
			[    0.003606] CPU MTRRs all blank - virtualized system.  
			[    0.095715] Booting paravirtualized kernel on KVM  
			[    2.637575] systemd[1]: Detected virtualization oracle.  
5. `sudo sysctl -a | grep fs.nr_open`  - количество файловых дескрипторов для одного процесса, значение по умолчанию. 

		fs.nr_open = 1048576 
   
   `ulimit -n`  - "мягкие" ограничения, не позволят достичь лимита системы по умолчанию.  
   
 	 	1024 
6. `sudo -i`  
	 `screen`  
   `unshare -f -pid --mount-proc sleep 1h&`  
   `nsenter --target 1861 --pid --mount`  
   `ps aux`  
		
    	USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND  
			root           1  0.0  0.0   9828   528 pts/1    S+   17:49   0:00 sleep 1h  - PID 1
			root           2  0.2  0.3  11560  3948 pts/0    S    17:51   0:00 -bash  
			root          11  0.0  0.3  13216  3260 pts/0    R+   17:51   0:00 ps aux  
7.  :(){ :|:& };: - цитата:  "команда является логической бомбой. Она оперирует определением функции с именем ‘:‘, которая вызывает сама себя дважды: один раз на переднем плане и один раз в фоне. Она продолжает своё выполнение снова и снова, пока система не зависнет."  
		`dmesg`  - смотрим логи.
   
		[ 5752.790410] cgroup: fork rejected by pids controller in /user.slice/user-1000.slice/session-13.scope - сработали ограничение на запуск процессов в сессии cgroups. 
		    
   `cat /sys/fs/cgroup/pids/user.slice/user-1000.slice/pids.max`  - узнаем значение по умолчанию, которое можно увеличить.
		2356 

    
