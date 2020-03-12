# Задание на программирование и автоматизацию. В сервисе http://3.122.236.22/ вы можете зарегистрировать пользователя и зайти в личный кабинет, но его сессия живет 1 секунду. После истечения времени жизни сессии пользователь не может зайти повторно ("одноразовая" учётная запись). Ваша задача автоматизировать переход с уровня на уровень и получить в конце флаг.

***Ответ: flag{jv5lthkBdpDez22}***

```Python
import requests,datetime
from requests_html import HTML
mylogin = "login"+datetime.datetime.now().strftime("%f")
mypass = "pass"+datetime.datetime.now().strftime("%f")
print(mylogin,mypass)
url = "http://3.122.236.22"
register = requests.post(url+"/register" ,data = {"txtUsername":mylogin, "txtPassword":mypass})

login = requests.post(url+"/login" ,data = {"txtUsername":mylogin, "txtPassword":mypass}, allow_redirects=False)
cookie = login.cookies.get_dict()

level1 = requests.get(url+"/level1", cookies=cookie)
#print(level1.text)
html = HTML(html=level1.text)
test = list(html.links)

level2 = requests.get(url+test[1], cookies=cookie)
#print(level2.text)
#print(test[1])

level3 = requests.post(url+test[1] ,cookies=cookie)

csrf_token = level3.text.split('\"')[59]
level4 = requests.post(url+"/level3" ,cookies=cookie,data = {"csrf_token":csrf_token})

express = level4.text.split('\"')[54].split("=")[0][1::]
res=eval(express)
level4 = requests.post(url+"/level4" ,cookies=cookie,data = {"captcha":res})
print(level4.text)

```
____
##### Score: 10/10

