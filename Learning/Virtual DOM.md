DOM (Document Object Model) -  объектная модель документа, представление UI в приоложении. 
Каждый тэг является объектом и все эти объект доступны для JS.
Virtual DOM - это копия DOM дерева, которая не может отображаться на экране. VDOM существует для оптимизации процессов. 
Если используем обычное DOM-дерево, то каждое изменение в приложении будет приводить к перерисовке всего DOM-дерева. 
VDOM решает эту проблему. Он находит элементы который были изменены и в RDOM изменяет только их. (использование key для тегов)


Согласование - процесс когда идеальное представление UI хранится в памяти и синхронизируется в RDOM.
![[Pasted image 20220312211731.png]]

https://www.youtube.com/watch?v=A0W2n2azH5s&t=271s

https://www.youtube.com/watch?v=NPXJnKytER4

Reconsilation(согласование) - алгоритмы которые мы называем вирутальным домом.

Все начинается с node с помощью которых строится dom-дерево.
это дерево называется current node.(хранится в памяти). Это дерево попадает в среду рендеринга и представляется в виде операций, которые выстраиваются по приоритету. При первом рендере отрисовывается все дерево. После выполнения пользователем какого-то действия создается дерево называемое work on progress tree, которое ищет разницу с current tree, псоле того как разница надена только она попадает в среду рендеринга -> опять происходит представление в виде операция и приоретизация. В результате эти операции обновят дерево и work in progress tree становится current tree.

Передовые алгоритмы имеют сложность O(n^3) => при отображении 1000 нод будет порядка 1млрд сравнений - слишком дорого

Реакт разработчики выбрали алгоритм O(n) - линейный, тк на 1000 нод требуется 1000 сравнений.

Ивристический алгоритм - [метод](https://ru.wikipedia.org/wiki/%D0%AD%D0%B2%D1%80%D0%B8%D1%81%D1%82%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B8%D0%B9_%D0%B0%D0%BB%D0%B3%D0%BE%D1%80%D0%B8%D1%82%D0%BC)/ Это алгоритм который не является лучшим, но его достаточно для решения задачи. Позволяет ускорить вычисления если неизвестно когда точное решение будет найдено (new type = new tree, new key = new tree).

Когда реакт сравнивает деревья, он начинает с корневых элементов.

Если элементы различных типов, то реакт размонтирует старое дерево и создает новое с нуля тэг а меняем на article -> старое дерево удаляется и создается новое с article.

При удалении дерево вызывается метод componentWillUnmount. При посторении нового компонента экземпляры компонента получаеют сomponentWillMount а уже после componentDidMount. Все состояние со старым деревом тоже удаляются.

```
<div>
  <Counter />
</div>

<span>
  <Counter />
</span>
```

Когда реакт начинает сравнивать элементы одного типа, он начинает смотреть на атрибуты элементов и обновляет только изменившиеся

НО когда компонент обновляется, его состояние сохраняется, реакт обновляет пропсы чтобы соответствовать новому  элементу, для это вызывает метод по получению пропсов и обновлению компонента, после чего рендерит.

Еще если мы добавляем какой-то элемент в существующий компонент, то если мы добавим элемент в начало, то будет перерендерино все что находится ниже, тк код читается сверху вниз. Эту проблему решает атрибут key. Он позволяет реакту понять какие элементы ранее существовали, а какие появились. 

Алгоритс согласования - деталь реализации. 
https://ru.reactjs.org/docs/reconciliation.html


## Приоретизация DOM-операций.
реакт сортирует задачи сам. например сначала покажет анимации а после уже будет обрабатывать данные

Для этого используется 2 метода
 - requestAnimationFrame - который используется для наиболее приоритетных операция
 - requestIdleCallback - для выполнения низкоприоритетных задач который можно выолнить позже

## React-Fiber
Позволяет определить какие операции нужно выполнить прямо сейчас, какие позже, а какие можно вообще не выполнять

https://github.com/acdlite/react-fiber-architecture !!!!!!!!!

