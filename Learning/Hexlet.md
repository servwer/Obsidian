Полиморфизм - описывает идею о том, что любое поведение родительского класса может быть переопределено в дочернем классе

Агрегация - любые вычисления которые строятяся на основе всего набора данных,  например, поиск максимального, среднего, суммы и так далее

множества https://ru.hexlet.io/courses/js-arrays/lessons/set-theory/theory_unit

https://ru.hexlet.io/pages/recommended-books

_javascript github kata_


Bubble sort
```js
// Функция изменяет входящий массив coll
const bubbleSort = (coll) => {
  let stepsCount = coll.length - 1;
  // Объявляем переменную swapped, значение которой показывает был ли
  // совершен обмен элементов во время перебора массива
  let swapped;
  // do..while цикл. Работает почти идентично while
  // Разница в проверке. Тут она делается не до выполнения тела, а после.
  // Такой цикл полезен там, где надо выполнить тело хотя бы раз в любом случае.
  do {
    swapped = false;
    // Перебираем массив и меняем местами элементы, если предыдущий
    // больше, чем следующий
    for (let i = 0; i < stepsCount; i += 1) {
      if (coll[i] > coll[i + 1]) {
        // temp – временная константа для хранения текущего элемента
        const temp = coll[i];
        coll[i] = coll[i + 1];
        coll[i + 1] = temp;
        // Если сработал if и была совершена перестановка,
        // присваиваем swapped значение true
        swapped = true;
      }
    }
    // Уменьшаем счетчик на 1, т.к. самый большой элемент уже находится
    // в конце массива
    stepsCount -= 1;
  } while (swapped); // продолжаем, пока swapped === true

  return coll;
};

console.log(bubbleSort([3, 2, 10, -2, 0])); // => [ -2, 0, 2, 3, 10 ]
```


Стек — упорядоченная коллекция элементов, в которой добавление новых и удаление старых элементов всегда происходит с одного конца коллекции.

Вызов функций — не что иное, как добавление элемента в стек, а возврат – извлечение из стека.