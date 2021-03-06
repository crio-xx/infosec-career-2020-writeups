### На Windows 7 у вас есть 2 учетные записи: admin (локальный админ) и User (не имеющий права администратора). Вы знаете пароль только учетной записи User. 
### Можете ли вы зайти на узел с учетной записью User с помощью инструмента PsExec? 
### Если нет, то почему? Если да, то почему?
__________________________

***Ответ:*** Нет, не сможем.

Так как **PsExec** для успешной работы должен временно добавить на компьютер свою службу ***PSEXESVC***, то ему требуются *права администратора*, а без них мы только увидим сообщение об отказе.

При этом, даже имея учетную запись *локального администратора* мы можем попасть под действие **UAC** (*User Account Control*), т.е. наше удаленное подключение будет всё равно ограничено. 

Существует способ выдачи привилегированного доступа всем членам локальной группы Администраторы на удаленной машине, без необходимости запроса на повышение доступа. Для этого создается ключ реестра **LocalAccountTokenFilterPolicy** с значением **1** по адресу:      
    HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

Это является серьезным *ослаблением безопасности компьютера*, т.к. дает полный доступ для членов ***локальной группы Администраторы***. Т.е. получив хэш пароля учетной записи пользователя из этой группы и использовав его при удаленном доступе (*pass-the-hash*) пользователь сразу получает полный доступ в систему.

____
##### Score: 8/10
