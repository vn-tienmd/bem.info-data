# jquery

Блок служит для загрузки и подключения на страницу библиотеки [jQuery](https://jquery.com) и ее расширений.
Расширения подключаются через зависимости от элементов блока.

## Способы использования

```js
modules.require(['jquery'], function($) {
    console.log($);
});
```

## Обзор

### Элементы блока

| Элемент | Способы использования | Описание |
| --------| --------------------- | -------- |
| <a href="#elems-config">config</a> | `JS` | Настройки jQuery. |
| <a href="#elems-event">event</a> | `JS` | Расширения событийной модели jQuery. |

### Свойства и методы элементов блока

| Элемент| Имя | Тип возвращаемого значения | Описание |
| -------| --- | ----------------------------- | -------- |
| <a href="#elems-config">config</a> | <a href="#fields-url">url</a> | `String` | Строка с URL, подключаемой библиотеки jQuery. |

### События элементов блока

| Элемент | Имя | Описание |
| ------- | --- | -------- |
| <a href="#elems-event">event</a> | <a href="#events-pointerclick">pointerclick</a> | Позволяет избавиться от задержки события `click` на тач-устройствах. |
|  | <a href="#events-pointerover">pointerover</a> | Генерируется, когда указатель устройства ввода находится над элементом. |
|  | <a href="#events-pointerenter">pointerenter</a> | Генерируется при входе указателя в активную зону элемента. |
|  | <a href="#events-pointerdown">pointerdown</a> | Генерируется при входе устройства ввода в состояние активного нажатия. |
|  | <a href="#events-pointermove">pointermove</a> | Генерируется при изменении координат указателя. |
|  | <a href="#events-pointerup">pointerup</a> | Генерируется по выходу из состояния активного нажатия. |
|  | <a href="#events-pointerout">pointerout</a> | Генерируется когда указатель покидает зону над элементаом. |
|  | <a href="#events-pointerleave">pointerleave</a> | Генерируется при выходе указателя из активной зоны элемента. |
|  | <a href="#events-pointerpress">pointerpress</a> | Генерируется по событию `pointerdown`. |
|  | <a href="#events-pointerrelease">pointerrelease</a> | Генерируется по событиям `pointerup` и `pointercancel`. |
|  | <a href="#events-pointercancel">pointercancel</a> | Генерируется в случаях, когда не предполагается дальнейшее возникновение pointer-событий или после генерации события `pointerdown`. |

### Публичные технологии блока

Блок реализован в технологиях:

* `js`

<a name="elems-config"></a>

# Элемент `config` блока `jquery`

Элемент предоставляет объект с настройками подключаемой библиотеки jQuery. Настройки хранятся как свойства объекта.

<a name="fields"></a>

## Свойства и методы объекта

<a name="fields-url"></a>

### Свойство `url`

Тип: `String`.

Содержит строку с URL для загрузки jQuery.
В проекте значение свойства может быть переопределено. Тогда при подключении блока будет использовано новое значение, если библиотека jQuery не была подключена предварительно.

```js
modules.define('jquery__config', function(provide) {

provide({ url: '//foo.bar/my-custom-jquery.js' });

});
```

<a name="elems-event"></a>

# Элемент `event` блока `jquery`

Элемент реализует поддержку дополнительных типов событий jQuery. Дополнительные типы подключаются с помощью соответствующих значений модификатора `type`.

<a name="modifiers"></a>

## Модификаторы элемента

<a name="modifiers-name"></a>

### Модификатор `type`

Предоставляет набор полифилов, реализующих уровень абстракции над jQuery-событиями устройств ввода. Это позволяет создавать общую логику для различных платформ (десктопы, телефоны и т.д.) и дополнять ее методами, специфическими для конкретного вида устройств.

Каждый полифил добавляет набор **pointer-событий**, для создания аппаратно-независимой логики.

Все pointer-события являются пользовательскими событиями jQuery. Подписка на pointer-события осуществляется стандартным образом:

```js
modules.define('pointer-test', ['i-bem-dom'], function(provide, bemDom) {

provide(bemDom.declBlock(this.name, /** @lends pointer-test.prototype */ {
    onSetMod : {
        js : {
            inited : function() {
                // при инициализации подписываемся на pointerpress на самом блоке
                this._domEvents().on('pointerpress', this._onPress);
            }
        }
    },
    _onPress : function(e) {
        console.log(e.type);
        // при вызове обработчика для pointerpress подписываемся на pointerrelease
        this._domEvents().on('pointerrelease', this._onRelease);
    },
    _onRelease : function(e) {
        console.log(e.type);
        // при вызове обработчика для pointerrelease отписываемся от pointerrelease
        this._domEvents().un('pointerrelease', this._onRelease);
    }
}));
});
```

В зависимости от значения модификатора, будут подключены разные полифилы.

<a name="modifiers-type-pointer"></a>

#### Модификатор `type` в значении `pointer`

Модификатор для подключения всех типов pointer-событий. Не вводит дополнительной логики.

<a name="modifiers-type-pointerclick"></a>

#### Модификатор `type` в значении `pointerclick`

Подключает полифил, реализующий событие `pointerclick`.

<a name="events-pointerclick"></a>

##### Событие `pointerclick`

Генерируется по нажатию левой клавиши мыши или в момент контакта с рабочей поверхностью устройства. Использование `pointerclick` позволяет избавиться от задержки события `click` на тач-устройствах.

<a name="modifiers-type-pointernative"></a>

#### Модификатор `type` в значении `pointernative`

Подключает полифил, реализующий базовую функциональность модели [W3C Pointer Events](http://www.w3.org/TR/pointerevents/).

С модификатором доступен следующий набор событий:

<a name="events-pointerover"></a>

##### Событие `pointerover`

Генерируется:

* Когда указатель находится над элементом.
* Перед событием `pointerdown`, для устройств, не поддерживающих `hover`.

<a name="events-pointerenter"></a>

##### Событие `pointerenter`

Генерируется:

* При входе указателя в *активную зону* элемента. Когда указатель находится над элементом или одним из его потомков.
* По событию `pointerdown` для устройств, не поддерживающих `hover`.

Событие аналогично `pointerover`, но не всплывает.

<a name="events-pointerdown"></a>

##### Событие `pointerdown`

Генерируется в момент, когда устройство ввода входит в состояние *активного нажатия*:

* Для мыши – когда хотя бы одна клавиша нажата.
* Для перьевых и тач- устройств – состояние контакта с рабочей поверхностью устройства.

<a name="events-pointermove"></a>

##### Событие `pointermove`

Генерируется при изменении координат указателя.

<a name="events-pointerup"></a>

##### Событие `pointerup`

Генерируется по выходу из состояния *активного нажатия*:

* Для мыши это соответствует переходу от состояния, когда была нажата хотя бы одна клавиша, к состоянию без нажатых клавиш.
* Для перьевых и тач- устройств – моменту разрыва контакта с рабочей поверхностью устройства.

<a name="events-pointerout"></a>

##### Событие `pointerout`

Генерируется при выходе указателя из *активной зоны* элемента:

* Когда указатель покидает зону над элементом или одним из его потомков.
* После событий `pointerup` и `pointercancel` для устройств, не поддерживающих `hover`. Например, когда стилус (палец) покидает рабочую зону устройства.

<a name="events-pointerleave"></a>

##### Событие `pointerleave`

Генерируется при выходе указателя из *активной зоны* элемента:

* Когда указатель покидает зону над элементом или одним из его потомков.
* После событий `pointerup` и `pointercancel` для устройств, не поддерживающих `hover`.

Событие аналогично `pointerout`, но не всплывает.

<a name="events-pointercancel"></a>

##### Событие `pointercancel`

Генерируется в случаях, когда:

* Не предполагается дальнейшее возникновение pointer-событий (например, в следствие изменения аппаратных настроек).
* После генерации события `pointerdown`, если указатель был использован для масштабирования страницы.

Например, событие генерируется при изменении ориентации устройства пока оно находится в состояние *активного нажатия*. Или при превышении лимита на количество одновременных нажатий, поддерживаемых устройством.

После генерации события `pointercancel` последовательно генерируются события `pointerout` и `pointerleave`.

<a name="modifiers-type-pointerpressrealease"></a>

#### Модификатор `type` в значении `pointerpressrealease`

Подключает полифил, реализующий события `pointerpress` и `pointerrelease`. Полифил использует события Pointer Events.

<a name="events-pointerpress"></a>

##### Событие `pointerpress`

Генерируется по событию `pointerdown`.

<a name="events-pointerrelease"></a>

##### Событие `pointerrelease`

Генерируется по событиям `pointerup` и `pointercancel`.
