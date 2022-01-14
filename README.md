# Winter Binary Studio Academy 2022 PHP

## Домашнє завдання Laravel introduction

**Логування** - це збереження Інформації про всі процеси резервного копіювання, аварійного завершення застосунку, тощо… Всі події повинні автоматично зберігається в лог, так що Ви завжди можете бути в курсі поточного стану програми.
Кожного разу, коли нам потрібно створити програму, з’являється необхідність, інтеграції системи логування. Причини для її імплементації різноманітні: 
- налагодження;
- статистика;
- потенційні попередження;
- загальні помилки.
- …

Більшість фреймворків і пакетів відповідають стандарту PSR-3, який описує, як працює система логування. По суті, це інтерфейс, на який ви повинні покладатися, передаючи журнали в систему. Серед них Monolog є найбільш використовуваним, оскільки він дуже гнучкий і легкий для розуміння.
Повертаючись до суті, реалізація PSR-3 описує вісім рівнів журналу. У порядку «серйозності»: надзвичайна ситуація, оповіщення, критична помилка, помилка, попередження, сповіщення, інформація та налагодження. (Emergency, Alert, Critical, Error, Warning, Notice, Informational, Debug)

_**Вашим завданням буде розробити свою систему логування.**_

### Перед початком роботи

- Ознайомитися з [теоретичними знаннями](https://en.wikipedia.org/wiki/Logging_(software)).
- Ознайомитися зі [стандартом PSR-3](https://www.php-fig.org/psr/psr-3/).
- Ознайомитися з [можливостями логування у системі Laravel](https://laravel.com/docs/8.x/logging).

### Завдання 1

* Написати міграцію на створення таблиці ‘logs’ в базі даних. У таблиці мають бути присутні наступні поля:


     ‘level’. У цій колонці можуть бути лише наступні значення: `['info', 'warning', 'error', 'debug', 'critical', 'alert', 'emergency', 'notice']`.

     ‘driver'. За замовчуванням значення повинне бути database.

     ‘message’. Обов’язкове поле.

     ‘trace’. Обов’язкове поле.

     ‘channel’. Може приймати значення null, якщо дані відсутні.

     ‘created_at’, ‘updated_at’. Для реалізації використовувати `$table->timestamps();`.

* Виконати міграцію за допомогою команди php artisan migrate.
* Повинна бути можливість відкотити (Rollback) міграцію.
* Створити Модель ‘Log’ у директорії ‘Назва_вашого_проекту/app/Models/
* Написати seeder для заповнення таблиці випадковими (random) даними.

### Завдання 2

* Реалізувати Інтерфейс LogRepositoryInterface та зареєструвати в сервіс контейнері Laravel.
* Реалізувати клас `GetAllLogsAction` та повернути `GetAllLogsResponse` зі всіма логами.
* Реалізувати клас `GetLogsByLevelAction` та повернути `GetLogsByLevelResponse`. _На вході масив з назвами рівнів логів. Необхідно повернути логи тільки відповідних рівнів._  
* Реалізувати клас `GetLogsStatisticAction` та повернути `GetLogsStatisticResponse`. _Необхідно отримати дані у зі структурою 'назва рівня' і 'кількість логів цього рівняю' Наприклад:_

`[
  'info' => 5,
  'warning' => 3,
  'error' => 3,
  'debug' => 21,
  'critical' => 1,
  'alert' => 0,
  'emergency' => 0,
  'notice' => 0,
]`


### Завдання 3

* Реалізувати маршрут `api/logs` в файлі `routes/api.php` по якому можна отримати всі логи у форматі json
* Реалізувати маршрут `api/logs/{level}` в файлі `routes/api.php` по якому можна отримати всі логи певного рівня у форматі json
* Реалізувати маршрут `api/logs/statistic` в файлі `routes/web.php` по якому можна отримати статистику по всіх рівнях і відрендирити їх у view `logs.blade.php`

### Завдання 4* _(Не обов’язкове для виконання)_
Вам потрібно буде реалізувати те саме, що і у завданні 3, але використовувати логування Laravel з коробки. Вам потрібно буде штучно заповнити журнал логування, який знаходиться за шляхом “Назва_вашого_проекту/storage/logs/laravel.log”. Наступним кроком буде реалізація LogStorageService. Вам потрібно буде розпарсити файл логів і повернути результати у такому ж виді, як і у завданні 3.
* Реалізувати маршрут `api/storage/logs` в файлі `routes/api.php` по якому можна отримати всі логи у форматі json
* Реалізувати маршрут `api/storage/logs/{level}` в файлі `routes/api.php` по якому можна отримати всі логи певного рівня у форматі json
* Реалізувати маршрут `api/storage/logs/statistic` в файлі `routes/web.php` по якому можна отримати статистику по всіх рівнях і відрендирити їх у view `logs_storage.blade.php`


### Перевірка
Своє рішення можна перевірити запустивши тести PHPUnit.

```bash
./vendor/bin/phpunit
```

### Docker

```
cp .env.example .env
cp .env.testing.example .env.testing
docker-compose up -d
docker-compose exec app php artisan migrate
docker-compose exec app php artisan db:seed
docker-compose exec app ./vendor/bin/phpunit
```

### Критерії оцінювання

* Завдання виконані коректно (7 балів)

* Валідація даних виконана коректно (використовуються кастомні винятки, додаток не ламається при невалдіних вхідних значеннях) (1 бал)

* Код написаний чисто, коментарів у коді немає і код відповідає стандарту [PSR-12](https://www.php-fig.org/psr/psr-12/) (1 бал)

* Awesomness - залишається на розсуд перевіряючого (1 бал)

### Фіналізуємо

Ваше рішення необхідно розмістити в окремій репозиторії на [Bitbucket](https://bitbucket.org/)
і надіслати посилання на нього.

Задавайте питання у коментарях до завдання у разі виникнення проблем.

*Форкати* цей репозиторій **заборонено**!
