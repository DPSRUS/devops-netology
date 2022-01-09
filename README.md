Домашнее задание  
1.`telnet stackoverflow.com 80` - получили ответ 301 - ресурс перемещен на другой адрес.
	
	Trying 151.101.129.69...  
	Connected to stackoverflow.com.  
	Escape character is '^]'.  

	GET /questions HTTP/1.0  
	HOST: stackoverflow.com  

	HTTP/1.1 301 Moved Permanently  
	cache-control: no-cache, no-store, must-revalidate  
	location: https://stackoverflow.com/questions  
	x-request-guid: e960456f-c016-4444-ba5f-e1d224d6e907  
	feature-policy: microphone 'none'; speaker 'none'  
	content-security-policy: upgrade-insecure-requests; frame-ancestors 'self' https://stackexchange.com  
	Accept-Ranges: bytes  
	Date: Sun, 09 Jan 2022 12:47:42 GMT  
	Via: 1.1 varnish  
	Connection: close  
	X-Served-By: cache-hhn4083-HHN  
	X-Cache: MISS  
	X-Cache-Hits: 0  
	X-Timer: S1641732462.218986,VS0,VE170  
	Vary: Fastly-SSL  
	X-DNS-Prefetch-Control: off  
	Set-Cookie: prov=ea7bbad7-eca2-0fde-4210-b29bc527df64; domain=.stackoverflow.com; expires=Fri, 01-Jan-2055 00:00:00 GMT; path=/; HttpOnly  

	Connection closed by foreign host.  

	
2.Ответ на первый запрос:  

	Status
	200
	OK
	VersionHTTP/2
	Transferred51.93 KB (173.22 KB size)
 Дольше всего запрос обрабатывался на favicon.ico?v=ec617d715196 - 1.33 s.  
 Скриншот консоли браузера  - https://github.com/DPSRUS/devops-netology/blob/main/images/Screenshot%20from%202022-01-09%2016-05-19.png  
3.`curl ifconfig.me`

	31.134.187.52
4.`whois 31.134.187.52`
	
	descr:          NevalinkRoute
	origin:         AS42668

5.`traceroute -An 8.8.8.8`
	
	traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets  
 	1  192.168.2.1 [*]  3.631 ms  3.515 ms  3.450 ms  
 	2  10.149.178.1 [*]  5.940 ms  5.884 ms  8.037 ms  
 	3  10.148.252.33 [*]  3.559 ms  3.507 ms  3.457 ms  
 	4  10.149.252.217 [*]  3.408 ms  5.574 ms  5.525 ms  
 	5  89.223.47.29 [AS42668]  7.678 ms  7.629 ms  7.578 ms  
 	6  89.223.47.26 [AS42668]  5.317 ms  27.675 ms  27.593 ms  
 	7  31.134.191.50 [AS42668]  32.390 ms  8.992 ms  8.311 ms  
 	8  * * *  
 	9  74.125.244.129 [AS15169]  8.623 ms 209.85.240.254 [AS15169]  8.569 ms 74.125.37.218 [AS15169]  8.523 ms  
	10  74.125.244.180 [AS15169]  8.481 ms  8.441 ms 74.125.244.132 [AS15169]  8.401 ms  
	11  72.14.232.85 [AS15169]  8.361 ms 216.239.48.163 [AS15169]  12.420 ms 72.14.232.84 [AS15169]  4.388 ms  
	12  142.250.232.179 [AS15169]  7.897 ms 216.239.63.65 [AS15169]  7.236 ms 142.251.61.221 [AS15169]  36.943 ms  
	13  * 142.250.56.221 [AS15169]  38.936 ms 72.14.237.199 [AS15169]  38.589 ms  
	14  * * *  
	15  * * *  
	16  * * *  
	17  * * *  
	18  * * *  
	19  * * *  
	20  * * *  
	21  * * *  
	22  8.8.8.8 [AS15169]  8.192 ms  8.006 ms *  
6.`mtr 8.8.8.8`  
	
	-> 8.8.8.8                                                                                                                                                             2022-01-09T16:32:17+0300  
	Keys:  Help   Display mode   Restart statistics   Order of fields   quit  
                                                                                                                                                                          Packets               Pings  
	 Host                                                                                                                                                                   Loss%   Snt   Last   Avg  Best  Wrst StDev  
 	1. router.lan                                                                                                                                                           0.0%    31    1.0   1.2   0.8   3.0   0.6  
 	2. 10.149.178.1                                                                                                                                                         0.0%    31    1.4   1.5   1.2   2.6   0.3  
 	3. 10.148.252.33                                                                                                                                                        0.0%    31    3.4   1.7   1.2   4.5   0.7  
 	4. 10.149.252.217                                                                                                                                                       0.0%    31    1.8   2.3   1.7   8.5   1.4  
 	5. 89.223.47.29                                                                                                                                                         0.0%    31    2.6   3.0   2.3   5.7   1.0  
 	6. 89.223.47.26                                                                                                                                                         0.0%    31    2.1  10.5   1.9 132.3  32.4  
 	7. 31.134.191.50                                                                                                                                                        0.0%    31    2.5   2.7   2.3   5.8   0.6  
 	8. 74.125.244.129                                                                                                                                                       0.0%    30    3.4   3.5   3.2   4.2   0.3  
 	9. 74.125.244.133                                                                                                                                                       0.0%    30    2.3   5.2   2.3  36.1   7.2  
	10. 72.14.232.84                                                                                                                                                         0.0%    30    2.7   4.4   2.7  20.0   3.3  
	11. 216.239.48.163                                                                                                                                                       0.0%    30    6.0  15.1   5.8  72.3  15.8  
	12. 142.250.238.179                                                                                                                                                      0.0%    30    6.8   7.6   6.7   9.9   0.8  
	13. (waiting for reply)  
	14. (waiting for reply)  
	15. (waiting for reply)  
	16. (waiting for reply)  
	17. (waiting for reply)  
	18. (waiting for reply)  
	19. dns.google                                                                                                                                                           80.0%    30    5.9   6.4   5.9   7.6   0.7  
 216.239.48.163  0.0%    30    6.0  15.1 - на данном участке наибольшая задержка.  
 7.`dig @dns.google` - узнаем NS сервера.  

	; <<>> DiG 9.16.15-Ubuntu <<>> @dns.google  
	; (4 servers found)  
	;; global options: +cmd  
	;; Got answer:  
	;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 578  
	;; flags: qr rd ra ad; QUERY: 1, ANSWER: 13, AUTHORITY: 0, ADDITIONAL: 1  

	;; OPT PSEUDOSECTION:  
	; EDNS: version: 0, flags:; udp: 512  
	;; QUESTION SECTION:  
	;.				IN	NS  

	;; ANSWER SECTION:  
	.			61389	IN	NS	f.root-servers.net.  
	.			61389	IN	NS	l.root-servers.net.  
	.			61389	IN	NS	d.root-servers.net.  
	.			61389	IN	NS	b.root-servers.net.  
	.			61389	IN	NS	i.root-servers.net.  
	.			61389	IN	NS	c.root-servers.net.  
	.			61389	IN	NS	e.root-servers.net.  
	.			61389	IN	NS	a.root-servers.net.  
	.			61389	IN	NS	h.root-servers.net.  
	.			61389	IN	NS	g.root-servers.net.  
	.			61389	IN	NS	j.root-servers.net.  
	.			61389	IN	NS	k.root-servers.net.  
	.			61389	IN	NS	m.root-servers.net.  

	;; Query time: 4 msec  
	;; SERVER: 8.8.4.4#53(8.8.4.4)  
	;; WHEN: Вс янв 09 16:38:06 MSK 2022  
	;; MSG SIZE  rcvd: 239  
`dig  dns.google` - узнаем А записи.  

	; <<>> DiG 9.16.15-Ubuntu <<>> dns.google  
	;; global options: +cmd  
	;; Got answer:  
	;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 16385  
	;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1  

	;; OPT PSEUDOSECTION:  
	; EDNS: version: 0, flags:; udp: 65494  
	;; QUESTION SECTION:  
	;dns.google.			IN	A  

	;; ANSWER SECTION:  
	dns.google.		247	IN	A	8.8.8.8  
	dns.google.		247	IN	A	8.8.4.4  

	;; Query time: 0 msec  
	;; SERVER: 127.0.0.53#53(127.0.0.53)  
	;; WHEN: Вс янв 09 16:40:47 MSK 2022  
	;; MSG SIZE  rcvd: 71  
8.`dig -x 8.8.8.8` - узнаем PTR запись.  

	; <<>> DiG 9.16.15-Ubuntu <<>> -x 8.8.8.8  
	;; global options: +cmd  
	;; Got answer:
	;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 31228
	;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 3

	;; OPT PSEUDOSECTION:
	; EDNS: version: 0, flags:; udp: 65494
	;; QUESTION SECTION:
	;8.8.8.8.in-addr.arpa.		IN	PTR

	;; ANSWER SECTION:
	8.8.8.8.in-addr.arpa.	18124	IN	PTR	dns.google.

	;; ADDITIONAL SECTION:
	dns.google.		33	IN	A	8.8.4.4
	dns.google.		33	IN	A	8.8.8.8

	;; Query time: 0 msec
	;; SERVER: 127.0.0.53#53(127.0.0.53)
	;; WHEN: Вс янв 09 16:44:21 MSK 2022
	;; MSG SIZE  rcvd: 105
