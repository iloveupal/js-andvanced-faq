# Ответы на продвинутые вопросы по Javascript

Данный рид предназначен для тех, кто хочет знать больше о спецификации языка и сдавать всякие тесты по знанию javascript на 100%.
Конечно, lodash или underscore обладает более широким функционалом и рекомендуется к использованию, но иногда знание простых вещей
в стандарте будет спасать от велосипедоконструирования, паники при необходимости использовать slice; давать более ясное понимание языка)

-------------------

##### Содержание 
- [call, apply & bind](#callapplybind)
- [fibonacci](#fibonacci)


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



<a name="fibonacci"/>

##Fibonacci

В данном примере будет рассмотрено два паттерна: кеширование и defer. 
Допустим, у нас есть функция, время выполнения которой занимает большое время.
Кеширование решает проблему повторного использования функции с теми же аргументами. 
По сути мы создаем статический map и записываем в него результаты таким образом: 
аргумент - значение. Каждый раз, вызывая функцию, мы проверяем, есть ли уже значение 
для данных аргументов. Это работает при условии, что функция *однозначна для данного аргумента*.

Так как js однопоточен, то очень здорово было бы не замораживать UI долгими вычислениями. 
В js можно отодвигать выполнение функции в конец очереди, и мы будем разделять нашу задачу на несколько маленьких частей
и отодвигать их в конец очереди, чтобы не замораживать ui. КЛАССНО, ДА??!

```javascript
var deferFibonacci = (n) => {
  if (!deferFibonacci.cache)
    deferFibonacci.cache = {};
  console.log('n = ', n);
  if (n < 2) return Promise.resolve(1);
	
  if (!!deferFibonacci.cache[n]) return Promise.resolve(deferFibonacci.cache[n])
	//сихнронный вариант
  var calculus = resolve => {
    var a;
    deferFibonacci(n - 1).then(
      res => {
        a = res;
        deferFibonacci.cache[n - 1] = a;
        return deferFibonacci(n - 2)
      }
    ).then(
      res => {
        deferFibonacci.cache[n - 2] = res;
        deferFibonacci.cache[n] = a + res;
        resolve(a + res)
      }
    )
  };
	//асинхронный
  var deffered = function(resolve) {
    setTimeout(calculus, 0, resolve);
  }
	//возвращаем главный промис
  return new Promise(deffered);
};
//ui не тормозит
deferFibonacci(145).then(res => console.log(res))
```