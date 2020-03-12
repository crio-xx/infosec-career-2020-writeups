# №2. Вы подключились в корпоративную сеть предприятия, ваш ноутбук не входит в домен Active Directory. Опишите как можно больше способов, как бы вы искали контроллер домена и определяли его полное FQDN имя.

#### 1) Поиск контроллера домена
  Для начала нам нужно узнать *имя домена*. Как правило оно передается в параметрах **DHCP** сервера *domain-name* (5) и или *domain-search* (119). Посмотреть значения этих параметров можно в файле */etc/resolv.conf*. После получения **имени домена**, мы можем запросить у *DNS-сервера* список всех **контроллеров домена**.
+  Способ 1. 
Запрос ldap SRV записей.
	
		nslookup -type=srv _ldap._tcp.dc._msdcs.infosec-career.tk
+  Способ 2. 
Запрос kerberos записей.
	
		dig any _kerberos._tcp.infosec-career.tk
  		dig any _kpasswd._tcp.infosec-career.tk
+  Способ 3. 
Запрос A записей. 
  
		dig infosec-career.tk
+  Способ 4. 
Запрос c использованием nltest. (если используется АРМ)
   
	 	nltest /dclist:infosec-career.tk

#### 2) Определение полного FQDN имени контроллера домена
**FQDN** (*Fully Qualified Domain Name*)
1. Получаем имя текущего домена
2. Выполняем DNS-запрос по имени домена и получаете список IP-адресов контроллеров
3. Выполняем DNS-запрос для каждого адреса и получаем их имена

Для выполнения DNS-запросов можно воспользоваться **Dns.GetHostEntry** или стандартной утилитой **nslookup**.
##### Способы: 
	nbtstat -a infosec-career.tk
	ping -a infosec-career.tk
	dig -x infosec-career.tk
	Сканирование SPN с помощью скрипта на PowerShell.
	dsquery server -forest | dsget server -desc -dnsname -isgc
	host / dnsdomainname
____
##### Score: 5/10
