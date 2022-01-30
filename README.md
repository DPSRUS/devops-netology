# Домашнее задание  
### 1. Подключитесь к публичному маршрутизатору в интернет. Найдите маршрут к вашему публичному IP
`route-views>show bgp 31.134.187.2` - просмотр таблиыц маршрутизации BGP до IP.

	BGP routing table entry for 31.134.128.0/18, version 47506
	Paths: (23 available, best #22, table default)
  	Not advertised to any peer
  	Refresh Epoch 1
  	 701 3356 9002 42668
    		137.39.3.55 from 137.39.3.55 (137.39.3.55)
		Origin IGP, localpref 100, valid, external
        	path 7FE02E7E5DF8 RPKI State not found
        	rx pathid: 0, tx pathid: 0
  	Refresh Epoch 1
  	 20912 3257 9002 42668
    		212.66.96.126 from 212.66.96.126 (212.66.96.126)
       		Origin IGP, localpref 100, valid, external
       		Community: 3257:8052 3257:50001 3257:54900 3257:54901 20912:65004 65535:65284
       		path 7FE13D9F3808 RPKI State not found
       		rx pathid: 0, tx pathid: 0
 	Refresh Epoch 1
 	 3333 9002 42668
 	 	193.0.0.56 from 193.0.0.56 (193.0.0.56)
  	    	Origin IGP, localpref 100, valid, external
      		path 7FE0B419C5A8 RPKI State not found
      		rx pathid: 0, tx pathid: 0

### 2. Создайте dummy0 интерфейс в Ubuntu. Добавьте несколько статических маршрутов. Проверьте таблицу маршрутизации.

`sudo modprobe -v dummy numdummies=2`  - включаем поддержку в ядре, создаем 2 инерфейса dummy.  
`lsmod | grep dummy` - проверяем.  
	
	dummy                  16384  0
`sudo ip link set up dummy0`  - подымаем интерфейс.  
`ip -br a`  - смотрим доступные интерфейсы.  

	lo               UNKNOWN        127.0.0.1/8 ::1/128 
	eth0             UP             10.0.2.15/24 fe80::a00:27ff:feb1:285d/64 
	eth1             UP             192.168.56.11/24 fe80::a00:27ff:feeb:72bc/64 
	dummy0           UNKNOWN        fe80::a4d6:4cff:fe14:d50d/64 
	dummy1           DOWN  
`sudo ip address add 10.10.10.22 dev dummy0` - назначим тестовый IP для dummy0.  
`ip -br a` - проверяем IP.

	lo               UNKNOWN        127.0.0.1/8 ::1/128 
	eth0             UP             10.0.2.15/24 fe80::a00:27ff:feb1:285d/64 
	eth1             UP             192.168.56.11/24 fe80::a00:27ff:feeb:72bc/64 
	dummy0           UNKNOWN        10.10.10.22/32 fe80::a4d6:4cff:fe14:d50d/64 
	dummy1           DOWN           
`ip -c ro` - смотрим таблицу маршрутизации.

	default via 10.0.2.2 dev eth0 proto dhcp src 10.0.2.15 metric 100 
	10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 
	10.0.2.2 dev eth0 proto dhcp scope link src 10.0.2.15 metric 100 
	192.168.56.0/24 dev eth1 proto kernel scope link src 192.168.56.11 
`sudo ip route add 8.8.8.8/32 via 192.168.56.11 dev eth1` - тестовый маршрут.  
`sudo ip route add 8.8.8.8/32 via 10.10.10.22 dev dummy0` - тестовый маршрут через dummy интерфейс.  
### 3. Проверьте открытые TCP порты в Ubuntu, какие протоколы и приложения используют эти порты? Приведите несколько примеров.  
`sudo ss -natp` - просмотр открытых портов TCP. Используютcя протоколы: SSH 22 (служба sshd), HTTP 80 (Curl запустил для тестов, чтобы порт показать), DNS 53 (служба systemd, выполняющая разрешение сетевых имён).  
	
	State                Recv-Q            Send-Q                        Local Address:Port                            Peer Address:Port             Process                                                           
	LISTEN               0                 4096                          127.0.0.53%lo:53                                   0.0.0.0:*                 users:(("systemd-resolve",pid=617,fd=13))                        
	LISTEN               0                 128                                 0.0.0.0:22                                   0.0.0.0:*                 users:(("sshd",pid=688,fd=3))                                    
	TIME-WAIT            0                 0                                 10.0.2.15:45744                         64.233.161.101:80                                                                                 
	ESTAB                0                 0                                 10.0.2.15:22                                  10.0.2.2:43248             users:(("sshd",pid=1444,fd=4),("sshd",pid=1409,fd=4))            
	LISTEN               0                 128                                    [::]:22                                      [::]:*                 users:(("sshd",pid=688,fd=4))  
### 4. Проверьте используемые UDP сокеты в Ubuntu, какие протоколы и приложения используют эти порты?
`sudo ss -naup` - просмотр открытых портов UDP. Используютcя протоколы: DNS 53 (служба systemd, выполняющая разрешение сетевых имён). DHCP 68 (systemd-networkd — системный демон для управления сетевыми настройками).
	
	State               Recv-Q              Send-Q                                Local Address:Port                           Peer Address:Port              Process                                                  
	UNCONN              0                   0                                     127.0.0.53%lo:53                                  0.0.0.0:*                  users:(("systemd-resolve",pid=617,fd=12))               
	UNCONN              0                   0                                192.168.56.11%eth1:68                                  0.0.0.0:*                  users:(("systemd-network",pid=1285,fd=21))              
	UNCONN              0                   0                                    10.0.2.15%eth0:68                                  0.0.0.0:*                  users:(("systemd-network",pid=1285,fd=23))     
### 5. Используя diagrams.net, создайте L3 диаграмму вашей домашней сети или любой другой сети, с которой вы работали.  
Ссылка на диаграмму - https://github.com/DPSRUS/devops-netology/blob/main/images/Home%20NET%20Diagram.drawio.png
