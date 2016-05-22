
### Использование call, bind, apply
- Методы call, apply, bind привязывают к функции контекст;
- Разница между call и apply заключается в способе передачи аргументов;
- bind возвращает новую функцию с заданным контекстом, которую можно будет вызвать позже.

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
