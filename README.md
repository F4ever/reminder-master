# Reminder

Запуск 
```docker-compose up```

Использовал стек
1. MySQL (Появился опыт работы и могу сказать что postgre шагнул далеко вперед)
2. Djnago
3. Redis (В качестве брокера для celery задач)
4. Celery 
5. Сelery beat (для периодических задач)
6. ReactJs (Минимальный фронт который только мог сделать)
7. Docker
8. DjnagoUnitTests + Faker


Ендпоинты для аунтификации (127.0.0.1:8000/api-auth/)       
    Получение профиля, для проверки авторизации  
    ```'^me/$'```  
    Регистрация  
    ```'^register/$'```  
    Аунтификация  
    ```'^login/$'```  
    Выход  
    ```'^logout/$'```  
    
    
Основные ендпоинты для работы с нотификациями (127.0.0.1:8000/api/v1/)  
    Получение списка юзеров  
    ```'^users/$'```  
    Получение доступных юзеру нотификаций, создание (GET, POST)  
    ```'^notifications/$'```  
    Работа с определённой нотификацией (GET, PATCH, DELETE)  
    ```'^notifications/(?P<pk>[\d]+)/$'```  
    
Особенности работы приложения
    Каждую минуту вызывается таск в котором я получаю все нотификации, которые я должен отправить в эту минуту.
    Отправляю и помечаю каждую нотификацию, как выполненую.
    Главный воркер создаёт дополнительных воркеров, только для отправки письма, чтобы не стопить отправкой письма весь стек.
    
Структура:  
   В api - лежат апишки для конечного юзера  
   В auth - ендпоинты для авторизации  
   В core - модель юзера и базовые таски  
   В notification - Все модели и логика которая отправляет нотификации  
   В reminder - Настройки  
   В ui - frontend  

Примечания:  

Во время работы при bulk_create, mysql не может выдать id, для объектов.  
Тоесть я не могу ими воспользоваться для создания других сущностей и приходится каждую вручную сохранять каждую и это очень грустно.

If the model’s primary key is an AutoField it does not retrieve and set the primary key attribute, as save() does, unless the database backend supports it (currently PostgreSQL).

Вообще, таск довольно большой для тестового, я старался везде сделать аккуратно (кроме фронтенда), но тяжело сфокусироваться на чём-то особенно в короткий срок (мне вообще сказали что задача на 4-8 часов:) )
