# Домашнее задание к занятию "3.9. Элементы безопасности информационных систем"  
### 1. Установите Bitwarden плагин для браузера. Зарегестрируйтесь и сохраните несколько паролей.
	Зарегистрировался, установил дополнение к браузер, сохранил для тестирования пару паролей.

### 2. Установите Google authenticator на мобильный телефон. Настройте вход в Bitwarden акаунт через Google authenticator OTP.
	Установил Google Auth на андройд. Настроил TWO-STEP LOGIN Authenticator App. Проверил вход через 2FA.

### 3. Установите apache2, сгенерируйте самоподписанный сертификат, настройте тестовый сайт для работы по HTTPS.
`sudo apt update && sudo apt upgrade -y` - обновляем систему.  
`sudo apt install apache2` - устанавливаем apache2.  
`sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt` - генерируем закрытый ключ и самоподписанный сертификат.
	
	Generating a RSA private key
	.............................................................+++++
	.............................................+++++
	writing new private key to '/etc/ssl/private/apache-selfsigned.key'
	-----
	You are about to be asked to enter information that will be incorporated
	into your certificate request.
	What you are about to enter is what is called a Distinguished Name or a DN.
	There are quite a few fields but you can leave some blank
	For some fields there will be a default value,
	If you enter '.', the field will be left blank.
	-----
	Country Name (2 letter code) [AU]:RU
	State or Province Name (full name) [Some-State]:SPB
	Locality Name (eg, city) []:SBB
	Organization Name (eg, company) [Internet Widgits Pty Ltd]:HOME
	Organizational Unit Name (eg, section) []:IT
	Common Name (e.g. server FQDN or YOUR name) []:test.local
	Email Address []:info@test.local
Генерируем конфигурацию с помощью сервиса ssl-config.mozilla.org.  
`sudo a2enmod rewrite` `sudo a2enmod ssl` `sudo a2enmod headers` - включаем необходимые модули.  
`sudo systemctl restart apache2` - перезапускаем apache2.  
Проверяем результат. Скриншот результат работы https - https://github.com/DPSRUS/devops-netology/blob/main/images/Https%20Apache.png

### 4. Проверьте на TLS уязвимости произвольный сайт в интернете (кроме сайтов МВД, ФСБ, МинОбр, НацБанк, РосКосмос, РосАтом, РосНАНО и любых госкомпаний, объектов КИИ, ВПК ... и тому подобное).
`bash testssl.sh -p 192.168.56.12`
	
	###########################################################
  	  testssl.sh       3.1dev from https://testssl.sh/dev/
   	 (13f0388 2022-02-02 13:34:23 -- )

   	   This program is free software. Distribution and
             modification under GPLv2 permitted.
   	   USAGE w/o ANY WARRANTY. USE IT AT YOUR OWN RISK!

   	    Please file bugs @ https://testssl.sh/bugs/

	###########################################################

	 Using "OpenSSL 1.0.2-chacha (1.0.2k-dev)" [~183 ciphers]
	 on vagrant:./bin/openssl.Linux.x86_64
	 (built: "Jan 18 17:12:17 2019", platform: "linux-x86_64")


	 Start 2022-02-03 16:23:09        -->> 192.168.56.12:443 (192.168.56.12) <<--

	 rDNS (192.168.56.12):   vagrant. vagrant.local.
	 Service detected:       HTTP


	 Testing protocols via sockets except NPN+ALPN 

	 SSLv2      not offered (OK)
	 SSLv3      not offered (OK)
 	TLS 1      not offered
 	TLS 1.1    not offered
	 TLS 1.2    offered (OK)
	 TLS 1.3    offered (OK): final
	 NPN/SPDY   not offered
	 ALPN/HTTP2 http/1.1 (offered)


	 Done 2022-02-03 16:23:13 [   7s] -->> 192.168.56.12:443 (192.168.56.12) <<--

### 5. Установите на Ubuntu ssh сервер, сгенерируйте новый приватный ключ. Скопируйте свой публичный ключ на другой сервер. Подключитесь к серверу по SSH-ключу.
`ssh-keygen` - генерируем пару ключей.
	
	Generating public/private rsa key pair.
	Enter file in which to save the key (/home/dz/.ssh/id_rsa): vagrant
	Enter passphrase (empty for no passphrase): 
	Enter same passphrase again: 
	Your identification has been saved in vagrant
	Your public key has been saved in vagrant.pub
	The key fingerprint is:
	SHA256:QhuptLRNTFOHjfoJJqeLQD7mobIQhQDZT6/CDHtx3Ic di@DZ
	The key's randomart image is:
	+---[RSA 3072]----+
	|+o    o..+.      |
	|o.. .o oo..      |
	|. .+oo*o         |
	|.o.o=BEo.        |
	|+= o+*++S.       |
	|o*= o  .o        |
	|=ooo .           |
	|+.. .            |
	|o.               |
	+----[SHA256]-----+
`ssh-copy-id vagrant@192.168.56.12` - копируем публичный ключ на сервер и привязываем его к пользователю.
	
	The authenticity of host '192.168.56.12 (192.168.56.12)' can't be established.
	ECDSA key fingerprint is SHA256:RztZ38lZsUpiN3mQrXHa6qtsUgsttBXWJibL2nAiwdQ.
	Are you sure you want to continue connecting (yes/no/[fingerprint])? y
	Please type 'yes', 'no' or the fingerprint: yes
	/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
	/usr/bin/ssh-copy-id: INFO: 4 key(s) remain to be installed -- if you are prompted now it is to install the new keys
	vagrant@192.168.56.12's password: 

	Number of key(s) added: 4

	Now try logging into the machine, with:   "ssh 'vagrant@192.168.56.12'"
	and check to make sure that only the key(s) you wanted were added
`ssh 'vagrant@192.168.56.12'` - пробуем подключиться с удаленному серверу по сертификату. Все работает.

### 6. Переименуйте файлы ключей из задания 5. Настройте файл конфигурации SSH клиента, так чтобы вход на удаленный сервер осуществлялся по имени сервера.
`nano ~/.ssh/config` - файл с настройками SSH клиента. Добавим необходимый сервер для подключения по имени.
	
	Host vagrant
	 HostName 192.168.56.12
	 user vagrant
	 IdentityFile ~/.ssh/vagrant
	 IdentitiesOnly yes
`ssh vagrant` - пробуем подкючиться по имени сервера. Все работает.

### 7. Соберите дамп трафика утилитой tcpdump в формате pcap, 100 пакетов. Откройте файл pcap в Wireshark.
`sudo tcpdump -i eth0 -c100 -n -nn -w vagrant.pcap` - собераем трафик.  
Открываем файл в Wireshark. Скриншот результата - https://github.com/DPSRUS/devops-netology/blob/main/images/Wireshark.png
