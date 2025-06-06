![](assets/_logo_django_.jpg)
# Фреймворк Django (семинары)
# Урок 3. Работа с представлениями и шаблонами

<br><br>
## Описание
На этом семинаре мы:
- изучим работу диспетчера URL;
- изучим шаблоны и передачу контекста в них;
- поработаем с условиями, циклами и наследованием шаблонов.
<br><br><hr>
## Домашнее задание

<br><br>
Уважаемые студенты! Обращаем ваше внимание, что сдавать домашнее задание необходимо через Git.

Задание: <br>
Продолжаем работать с товарами и заказами.

Создайте шаблон, который выводит список заказанных клиентом товаров, из всех его заказов с сортировкой по времени:
- за последние 7 дней (неделю)
- за последние 30 дней (месяц)
- за последние 365 дней (год)

Товары в списке не должны повторятся.
<br><br><hr>
## Решение задания

<br><br>
**Для запуска проекта**:
- Скачать архив с проектом;
- Перейти в директорию проекта 'shop_project';
- Запустить команду ```python manage.py runserver```;
- Открыть в браузере страницу с адресом http://127.0.0.1:8000/;
- По окончанию работы с проектом, отключить комбинацией клавиш 'Ctrl+C'.

<br><br>
### 1. Действия для запуска проекта (начало)

- Перейти в папку проекта и активировать виртуальное окружение:

```bash
   cd E:\shop_project
   myvenv\Scripts\activate.bat
```

- Запустить миграцию базы данных, чтобы убедиться, что база данных синхронизирована с последней версией:

 ```bash   
   python manage.py makemigrations
   python manage.py migrate
 ```

- После внесения изменений перезапустить сервер, выполнив команду:
```bash 
  python manage.py runserver 
```
- Так как, по умолчанию Django стартует на порту `8000`, следует открыть браузер<br> и перейти по адресу: [http://127.0.0.1:8000/](http://127.0.0.1:8000/).

### 2. Маршрутизация в браузере

- Главная страница проекта http://127.0.0.1:8000/
- Список всех клиентов http://127.0.0.1:8000/clients/
- Список всех продуктов http://127.0.0.1:8000/products/
- Список всех заказов http://127.0.0.1:8000/orders/
- Информация о конкретном клиенте http://127.0.0.1:8000/clients/1/ #http://127.0.0.1:8000/clients/{ID_клиента}/
- Информация о конкретном товаре http://127.0.0.1:8000/products/1/ #http://127.0.0.1:8000/products/{ID_товара}/
- Информация о конкретном заказе http://127.0.0.1:8000/orders/1/ #http://127.0.0.1:8000/orders/{ID_заказа}/
- Страница редактирования конкретного заказа http://127.0.0.1:8000/edit-order/1/ #http://127.0.0.1:8000/edit-order/{ID_заказа}/
- Страница добавления нового клиента http://127.0.0.1:8000/add-client/
- Страница добавления нового товара http://localhost:8000/add-product/
- Страница добавления нового заказа http://localhost:8000/add-order/
- Страница редактирования профиля http://localhost:8000/update-profile/

### 3. Шаблоны список заказов клиента

- Список всех заказов клиента http://127.0.0.1:8000/clients-orders/
- Список всех личных заказов http://127.0.0.1:8000/client-orders/

### 4. Остановка сервера

После окончания работы с сервером Django требуется остановка сервера или деактивация, выход из локального окружения. <br>
- Всегда останавливать сервер перед переходом к другим действиям.
- Использовать команду `deactivate` только после того, как вышли из запущенного сервера Django.
- Новый проект можно создавать сразу после освобождения консоли путём остановки текущего сервера.
- Если сервер не остановить, консоль останется заблокированной процессом Django, и никакие новые команды не смогут быть выполнены.
- Деактивация виртуального окружения возможна только тогда, когда оно было предварительно активировано.

Сервер Django остановится автоматически, если отправить сигнал прерывания процесса. Для этого можно воспользоваться комбинацией клавиш: <br>
- В Windows или Linux: **Ctrl+C**
- В macOS: **Command + C**

После нажатия комбинации клавиш сервер завершит свою работу, и можно вводить команды в консоли.
Ввести команду деактивации, которая вернет обратно в глобальную среду системы разработки:
```bash
myvenv\Scripts\deactivate.bat
```

<br><br><hr>
## Инструкция

- Проверка структуры таблиц в базе данных в SQLite через командную строку:

```
    sqlite3 db.sqlite3
    .tables
    .schema shop_profile
```

- Заполнение базы данных случайными тестовыми данными, для удобства тестирования
  1. Использовать модулем Faker, для этого установить библиотеку:
   ```bash
    pip install faker
   ```
  
   2. Теперь запустить указанный скрипт генерации данных командой:
   ```bash
    python manage.py populate_db
   ```


<br><br><hr>
## Дополнительная информация

<br><br>
### Работа диспетчера URL
Каждая страница на сайте ассоциируется с определенным маршрутом (URL), и Django обрабатывает этот маршрут, вызывая соответствующую view-функцию.
Система диспетчеризации URL в Django позволяет связывать конкретные маршруты запросов браузера с функциями-обработчиками (view-функциями), которые отвечают за формирование содержимого страницы.

##### Реализация маршрутов

##### 1. Создание URL-конфигурации:  
   Конфигурация маршрутов задаётся в файле `urls.py`, находящемся в каждом приложении Django. Здесь указывается, какой URL ведёт к какому обработчику (view-функции).

   ```
   # urls.py
   from django.urls import path
   from . import views

   urlpatterns = [
       path('', views.home, name='home'),          # Главная страница
       path('about/', views.about, name='about'), # О компании
       path('contact/', views.contact, name='contact'), # Контактная информация
   ]
   ```

##### 2. View-функции:  
   View-функции находятся в файле `views.py` и принимают HTTP-запросы, формируют ответ и возвращают HTML-контент.

   ```
   # views.py
   from django.shortcuts import render

   def home(request):
       return render(request, 'home.html')

   def about(request):
       return render(request, 'about.html')

   def contact(request):
       return render(request, 'contact.html')
   ```

<br><br>
### Шаблоны и передача контекста в них

Шаблоны используются для отделения бизнес-логики от представления страниц. Django-шаблоны поддерживают простой синтаксис, позволяющий динамически вставлять данные из представлений (view-функций).

- Контекст (context) - набор переменных, передаваемый из view-функции в шаблон.
- Шаблонизатор - механизм, заменяющий специальные метки в шаблоне на реальные данные из контекста.

##### Пример передачи контекста:

##### 1. Формирование контекста в view-функции: 
   Представления передают данные в шаблоны через словарь (контекст).

   ```
   # views.py
   def home(request):
       context = {
           'title': 'Главная',
           'message': 'Добро пожаловать на наш сайт!',
       }
       return render(request, 'home.html', context)
   ```

##### 2. Использование контекста в шаблонах: 
   Данные из контекста доступны в шаблоне с помощью двойных фигурных скобок `{{}}`.

   ```
   <!-- templates/home.html -->
   <h1>{{ title }}</h1>
   <p>{{ message }}</p>
   ```

<br><br>
### Условия, циклы и наследование шаблонов

#### 1. Условные конструкции в шаблонах:
Django поддерживает условные операторы прямо в шаблонах, позволяя выбирать контент в зависимости от условий.

```
{% if user.is_authenticated %}
   Привет, {{ user.username }}!
{% else %}
   Вам необходимо войти.
{% endif %}
```

#### 2. Циклы в шаблонах:
Для итерации по спискам и другим коллекциям удобно использовать цикл `{% for %}`, аналогичный Python-циклам.

```
<h2>Список сотрудников:</h2>
<ul>
{% for employee in employees %}
   <li>{{ employee.name }}, возраст: {{ employee.age }}</li>
{% empty %}
   Сотрудники пока не зарегистрированы.
{% endfor %}
</ul>
```

#### 3. Наследование шаблонов:
Наследование позволяет повторно использовать общие элементы дизайна и минимизировать дублирование разметки.

```
<!-- base.html -->
<!DOCTYPE html>
<html lang="ru">
<head>
    <title>{% block title %}{% endblock %}</title>
</head>
<body>
    <header>
        {% include 'navbar.html' %}
    </header>
    <main>
        {% block content %}{% endblock %}
    </main>
</body>
</html>

<!-- home.html -->
{% extends 'base.html' %}

{% block title %}Главная{% endblock %}

{% block content %}
<p>Приветствуем вас!</p>
{% endblock %}
```


<br><br><br><br>
<hr><hr><hr><hr>