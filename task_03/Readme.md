# Ответьте на следующие вопросы, касающиеся проведения пентеста в соответсвии с требованиями стандарта PCI DSS:    а) укажите виды проводимых пентестов; б) укажите, что включает в себя область пентеста (скоуп); в) как часто проводятся перечисленные в п. а) работы?

#### а) Виды проводимых пентестов

Тестирование проводится в формате **gray-box** (для минимизации угрозы *DoS* с предупреждением администраторов соответствующих систем)

+ 4 вида работ:
     + Анализ защищенности беспроводных сетей
     + Сканирование информационной сети на наличие уязвимостей (AsV)
     + Проведение проверок на сетевом уровне (Network-layer penetration tests)
     + Проведение проверок на уровне приложений (Application-layer penetration tests)
     
#### б) Область пентеста (Скоуп)
+ Оборудование, которое попадает в scope: 
    - системы, которые:
        - предоставляют службы безопасности (например, серверы аутентификации),
        - способствуют сегментации сети (например, внутренние межсетевые экраны),
        - могут влиять на безопасность среды ДДК (например, серверы разрешения имен или веб-переадресации);
    - компоненты виртуализации, например:
        - виртуальные машины,
        - виртуальные коммутаторы и (или) маршрутизаторы,
        - виртуальные приложения и (или) компьютеры,
        - гипервизоры;
    - сетевые компоненты, в том числе:
        - межсетевые экраны,
        - коммутаторы,
        - маршрутизаторы,
        - беспроводные точки доступа,
        - устройства сетевой безопасности,
        - прочие устройства безопасности;
    - типы серверов, в том числе:
        - веб-серверы,
        - серверы приложений,
        - серверы баз данных,
        - серверы аутентификации,
        - почтовые серверы, 
        - прокси-серверы,
        - серверы службы времени (NTP),
        - DNS-серверы;
    - приложения, включая все приобретенные или заказные приложения, в том числе внутренние и внешние (например, доступные
через Интернет);
    - любой другой компонент или устройство, расположенное внутри среды ДДК или подключенное к ней. 

- Цитата из официального документа **Penetration Testing Guidance**.
 
    - *The scope of an external penetration test is the exposed external perimeter of the CDE and critical
systems connected or accessible to public network infrastructures. It should assess any unique access
to the scope from the public networks, including services that have access restricted to individual
external IP addresses. Testing must include both application-layer and network-layer assessments.
External penetration tests also include remote access vectors such as dial-up and VPN connections.*
 
    - *The scope of the internal penetration test is the internal perimeter of the CDE from the perspective of
any out-of-scope LAN segment that has access to a unique type of attack on the CDE perimeter.
Critical systems or those systems that may impact the security of the CDE should also be included in
the scope. Testing must include both application-layer and network-layer assessments.*

    - *The penetration test may include systems not directly related to the
processing, transmission or storage of cardholder data to ensure these assets, if compromised, could
not impact the security of the CDE.*


#### в) Частота проведения пентест-работ 
Проверка проводится **не реже раза в год**, а также после внесения **значительных изменений**. (*пункт 11.3 из Penetration Testing Guidance*)

____
##### Score: 4/10
