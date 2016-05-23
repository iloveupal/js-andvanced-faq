# Ответы на продвинутые вопросы по Javascript

Данный рид предназначен для тех, кто хочет знать больше о спецификации языка и сдавать всякие тесты по знанию javascript на 100%.
Конечно, lodash или underscore обладает более широким функционалом и рекомендуется к использованию, но иногда знание простых вещей
в стандарте будет спасать от велосипедоконструирования, паники при необходимости использовать slice; давать более ясное понимание языка)

-------------------

##### Содержание 
- [call, apply & bind](#callapplybind)


-------------------

<a name="callapplybind"/>

## call, apply & bind

- Методы call, apply, bind привязывают к функции контекст. 
- Разница между call и apply заключается в способе передачи аргументов.
- bind возвращает новую функцию с заданным контекстом, которую можно будет вызвать позже

``` javascript
introduce = (name,profession) => {
    console.log(`My name is ${name} and my profession is ${profession}`); 
}

introduce('Derek','musician'); 
introduce.call(this,'Derek','musician');
introduce.apply(this,['Derek','musician']);
introduce.bind(this)('Derek','musician');
//Все 4 примера выведут в консоль: 
//My name is Derek and my profession is musician
```

*Неочевидное применение apply:* возможность использовать функции, которые принимают список параметро
но не принимают массив. Например, Math.max
``` javascript
let arr = [1,3,5,7];
Math.max(arr); //NaN 
Math.max.apply(null,arr); //7
``` 
Конечно, в новой спецификации лучше использовать оператор ```...```



