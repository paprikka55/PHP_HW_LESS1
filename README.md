# Домашнее задание. Урок 1. Введение в PHP
## Малятин Александр Сергеевич
***Задание:***
```
Собрать для себя окружение из Nginx + PHP-FPM и PHP CLI
```

Создан файл [docker-compose.yaml](docker-compose.yaml). С его помощью создаются три контейнера: [nginx](./nginx/Dockerfile), [php-fpm](./fpm/Dockerfile) и [php-cli](./cli/Dockerfile)
nginx и php-fpm из примера на практическом занятии, php-cli запускается из образа созданного на основе образа php:8.2-cli из Docker Hub. Для того чтобы контейнер не заканчивал работу добавлен ENTRYPOINT sleep 7d. Ожидания на 7 дней. 
Запуск кода выполняется командой:
```bash
docker exec php-cli php /code/index.php
```

***Задание:***

```
Выполните код в контейнере PHP CLI и объясните, что выведет данный код и почему:

<?php
$a = 5;
$b = '05';
var_dump($a == $b);
var_dump((int)'012345');
var_dump((float)123.0 === (int)123.0);
var_dump(0 == 'hello, world');
?>
```
Создали файл [task1.php](./code/task1.php) с текстом из задания. 
Запускаем код:
```bash
docker exec php-cli php /code/task1.php
```
Вывод:
```bash 
bool(true)
int(12345)
bool(false)
bool(false)
```

Команда 
```php 
var_dump
``` 
выводит тип переменной (результата операций с переменными) и его значения.

```php 
var_dump($a == $b);
```
Получаем ```bool(true)``` - при выполнении операции произошло неявное преобразование типов. Строка преобразовалась в int

```php
var_dump((int)'012345');
```
Получаем ```int(12345)```. Явно преобразуем строку в число.

```php
var_dump((float)123.0 === (int)123.0);
```
Получаем bool(false), так как значения явно преобразованы в разные типы данных. === - оператор Тождественно равно. Возвращает true, только если значения равны и имеет тот же тип.

```php
var_dump(0 == 'hello, world');
```
Получаем bool(false)
Сравнение выполняется численно, если оба операнда - числовые строки, или один операнд - число, а другой - числовая строка(PHP считает строку (string) числовой, если строку возможно интерпретировать как целое число (int) или как число с плавающей точкой (float)). Так как строка 'hello, world' - не является числовой, сравниваются строки.

***Задание:***
```
В контейнере с PHP CLI поменяйте версию PHP с 8.2 на 7.4. Изменится ли вывод?
```
Создали новый [Dockerfile](./cli/Dockerfile_cli74) с образом php:7.4-cli
Перезапустили пересобрали docker compose и запустили контейнеры
Вывод после запуска task1.php:
```
bool(true)
int(12345)
bool(false)
bool(true)
```
До PHP 8.0.0, если строка (string) сравнивалась с числом или числовой строкой, то перед выполнением сравнения строка (string) преобразовывалась в число. Поэтому при выполнении:
```php
var_dump(0 == 'hello, world');
```
Сначала строка 'hello, world' приводится к типу int потом идет сравнение.

***Задание:***
```
Используя только две числовые переменные, поменяйте их значение местами. Например, если a = 1, b = 2, надо, чтобы получилось: b = 1, a = 2. Дополнительные переменные, функции и конструкции типа list() использовать нельзя.
```

***Решение:***

[Файл task2.php](./code/task2.php)