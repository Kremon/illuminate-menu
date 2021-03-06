Laravel Menu Builder
--------------------

**Билдер меню для Laravel 4-5 под разметку [Bootstrap][twbs].**

Обратите внимание, что расширение поставляется без каких либо стилей и скриптов, их нужно отдельно установить с сайта
 [Twitter Bootstrap][twbs], или сделать своё оформление на основе правил, которые задает это фреймфорк.
 
[twbs]: http://getbootstrap.com "Twitter Bootstrap"

Установка
=========

Установка с помощью Composer:

```
composer require kalnoy/illuminate-menu:~1.0
```

Добавте провайдер:

```php
'providers' => [
    'Illuminate\Html\MenuServiceProvider',
],
```

И фасад для быстрого доступа:

```php
'aliases' => [
    'Menu' => 'Illuminate\Support\Facades\Menu',
],
```

Документация
============

Вывод меню:

```php
{!! Menu::render($items, $attributes) !!}
```

Где `$attributes` необязательный параметр с атрибутами для тэга `ul`.

Вывод только элементов меню без внешнего тэга:

```php
<ul>{!! Menu::items($items) !!}</ul>
```

Вывод одного элемента меню:

```php
{!! Menu::item($label, $url) !!}
{!! Menu::item($label, $options) !!}
{!! Menu::item($options) !!}
```

Список доступных опций доступен ниже.

Простой пример:

```php
Menu::render([
    'Локальная ссылка' => 'bar',
    'Внешняя ссылка' => 'http://bar',
    [ 'label' => 'Локальная ссылка', 'url' => 'bar' ],
    'Ссылка на роут' => [ 'route' => [ 'route.name', 'foo' => 'bar' ] ],
]);
```

Вывод элемента меню с выпадающим списком:

```php
{!! Menu::item([
    'label' => 'Настройки',
    'icon' => 'wrench',
    'items' => [
        'Foo' => 'bar',
        '-', // разделитель
        'Выйти' => [ 'route' => 'logout_path' ],
    ],
]) !!}
```

Управление видимостью элемента:

```php
{!! Menu::item([
    'label' => 'Foo',
    'url' => 'bar',
    'visible' => function () { return Config::get('app.debug'); },
] !!}
```

#### Опции

Вы можете использовать следующий список опций:

*   `label` заголовок элемента меню; автоматически переводится, т.е. можно указать идентификатор строки
*   `url` ссылка на локальный или внешний адрес
*   `route` чтобы указать роут с параметрами или без
*   `secure`; укажите `true` чтобы сделать локальную ссылку безопасной (https) (влияет только на опцию `url`)
*   `items` список элементов в выпадающем меню

Изменение состояния элемента:

*   `visible` булевское значение или closure для указания видимости элемента
*   `active` булевское значение или closure; если значение `true`, то элементу добавляется класс `active`
*   `disabled` булевское значение или closure; если `true`, то элементу добавляется класс `disabled` и он становится 
    некликабельным

Опции представления:

*   `icon` идентификатор иконки [glyphicon](http://getbootstrap.com/components/#glyphicons), например `pencil`
*   `badge` значение для метки (скалярное или closure)
*   любой другой параметр, который будет считаться как атрибут для элемента `<li>`.