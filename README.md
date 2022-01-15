Домашнее задание  
1.`ip -c -br l` - просмотр сетевых в linux. ipconfig /all- windows

	lo               UNKNOWN        00:00:00:00:00:00 <LOOPBACK,UP,LOWER_UP>  
	wlp3s0           UP             a0:d3:7a:f9:c4:0a <BROADCAST,MULTICAST,UP,LOWER_UP>  

2.Link Layer Discovery Protocol (LLDP). Необходимо установить пакет lldpd. Команда lldpctl.  
3.VLAN - разделение на виртуальные частные сети. Пакет vlan. Модуль ядра 8021q. Пример конфига через netplan:
	
	network:
 	 version: 2
  	 ethernets:
    	  eth0:
           dhcp4: true
  	  vlans:
      	  eth0.10:
           id: 10
           link: eth0
           addresses: [192.168.100.2/24]
`ip -c -br a`  

	lo               UNKNOWN        127.0.0.1/8 ::1/128  
	eth0             UP             10.0.2.15/24 fe80::a00:27ff:feb1:285d/64  
	eth0.10@eth0     UP             192.168.100.2/24 fe80::a00:27ff:feb1:285d/64  
4.Типы агрегации интерфейсов:
 Mode-0(balance-rr), Mode-1(active-backup), Mode-2(balance-xor), Mode-3(broadcast), Mode-4(802.3ad),  
 Mode-5(balance-tlb), Mode-6(balance-alb)  - для балансировки нагрузки.  
 Пример конфига в netplan:  
		
	bonds:
	   bond0:
	    dhcp4: no
	     interfaces: [eth0, eth1]
	     parameters: 
	     mode: 802.3ad
	     mii-monitor-interval: 1
5.Сеть с маской /29: `sipcalc 192.168.1.1/29`  

	-[ipv4 : 192.168.1.1/29] - 0

	[CIDR]
	Host address		- 192.168.1.1
	Host address (decimal)	- 3232235777
	Host address (hex)	- C0A80101
	Network address		- 192.168.1.0
	Network mask		- 255.255.255.248
	Network mask (bits)	- 29
	Network mask (hex)	- FFFFFFF8
	Broadcast address	- 192.168.1.7
	Cisco wildcard		- 0.0.0.7
	Addresses in network	- 8 - адресов!
	Network range		- 192.168.1.0 - 192.168.1.7
	Usable range		- 192.168.1.1 - 192.168.1.6
`sipcalc 10.10.10.0/24` 
	
	-[ipv4 : 10.10.10.0/24] - 0
	
	[CIDR]
	Host address		- 10.10.10.0
	Host address (decimal)	- 168430080
	Host address (hex)	- A0A0A00
	Network address		- 10.10.10.0
	Network mask		- 255.255.255.0
	Network mask (bits)	- 24
	Network mask (hex)	- FFFFFF00
	Broadcast address	- 10.10.10.255
	Cisco wildcard		- 0.0.0.255
	Addresses in network	- 256 - адресов!
	Network range		- 10.10.10.0 - 10.10.10.255
	Usable range		- 10.10.10.1 - 10.10.10.254
Доступно 256 адресов, на сети с маской /29 необходимо 8 адресов. 256/8=32 сети с маской /29 можно создать.
6. Для адресации можно взять адрес из диапозона 100.64.0.0 — 100.127.255.255 (маска подсети 255.192.0.0 или /10) - Данная подсеть рекомендована согласно RFC 6598 для использования в качестве адресов для CGN (Carrier-Grade NAT). С маской /26 для максимум 40-50 хостов внутри подсети.
`sipcalc 100.64.0.0/26`
	
	-[ipv4 : 100.64.0.0/26] - 0

	[CIDR]
	Host address		- 100.64.0.0
	Host address (decimal)	- 1681915904
	Host address (hex)	- 64400000
	Network address		- 100.64.0.0
	Network mask		- 255.255.255.192
	Network mask (bits)	- 26
	Network mask (hex)	- FFFFFFC0
	Broadcast address	- 100.64.0.63
	Cisco wildcard		- 0.0.0.63
	Addresses in network	- 64
	Network range		- 100.64.0.0 - 100.64.0.63
	Usable range		- 100.64.0.1 - 100.64.0.62
7.arp -a - arp таблица в Windows.  
`ip neighbour show` - arp таблица в Linux.  

	10.0.2.3 dev eth0 lladdr 52:54:00:12:35:03 STALE  
	10.0.2.2 dev eth0 lladdr 52:54:00:12:35:02 REACHABLE  
`sudo ip neighbour flush dev eth0` -  очистить ARP кеш полностью.   
`sudo ip neighbour del 10.0.2.3 dev eth0` - удалить только один нужный IP.
