# Arrays

Los objetos te permiten guardar una colección de pares de valores. Eso está bien.

Pero muy seguido encontramos que necesitamos una _colección ordenada_, donde tenemos un 1er, 2do, 3er elemento y así. Por ejemplo, necesitamos eso para guardar una lista de algo: usuarios, bienes, elementos HTML, etc.

No es conveniente usar un objeto en esos casos, porque no provee métodos para manejar el orden de los elementos. No podemos insertar una nueva propiedad "entre" los que ya existen. Los objetos no están destinados para tal uso.

Existe una estructura de datos especial llamada `Array`, para guardar colecciones ordenadas.

## Declaración

Hay dos sintaxis para crear un array vacío:

```js
let arr = new Array();
let arr = [];
```

Casi todo el tiempo, la segunda sintaxis es usada. Podemos proveer elementos iniciales entre los corchetes:

```js
let fruits = ['Apple', 'Orange', 'Plum'];
```

Los elementos del array estan numerados, empezando por cero.

Podemos obtener un elemento por su numero en corchetes:

```js run
let fruits = ['Apple', 'Orange', 'Plum'];

alert(fruits[0]); // Apple
alert(fruits[1]); // Orange
alert(fruits[2]); // Plum
```

Podemos reemplazar un elemento:

```js
fruits[2] = 'Pear'; // now ["Apple", "Orange", "Pear"]
```

...O añadir uno nuevo al array:

```js
fruits[3] = 'Lemon'; // now ["Apple", "Orange", "Pear", "Lemon"]
```

El número total de elementos en el array es su `length`:

```js run
let fruits = ['Apple', 'Orange', 'Plum'];

alert(fruits.length); // 3
```

También podemos usar `alert` para mostrar el array completo.

```js run
let fruits = ['Apple', 'Orange', 'Plum'];

alert(fruits); // Apple,Orange,Plum
```

Un array puede contener elementos de cualquier tipo.

Por ejemplo:

```js run no-beautify
// mix of values
let arr = [
    'Apple',
    { name: 'John' },
    true,
    function() {
        alert('hello');
    }
];

// get the object at index 1 and then show its name
alert(arr[1].name); // John

// get the function at index 3 and run it
arr[3](); // hello
```

````smart header="Trailing comma"
Un array, al igual que un objeto, puede terminar con una coma:
```js
let fruits = [
  "Apple",
  "Orange",
  "Plum"*!*,*/!*
];
```

El uso de una "coma final" hace mas fácil insertar/remover nuevos items, porque todas las líneas son parecidas.
````

## Métodos pop/push, shift/unshift

Una [cola](<https://https://es.wikipedia.org/wiki/Cola_(inform%C3%A1tica)>) o "queue" es uno de los usos más comunes de un array. En is one of most common uses of an array. En las ciencias de la computación, esto significa una colección ordenada de elementos que soportan dos operaciones:

-   `push` agrega un elemento al final.
-   `shift` agrega un elemento al principio, avanzando la cola, así el segundo elemento se convierte el primero.

![](queue.svg)

Los Arrays soportan ambas operaciones.

En la práctica lo necesiamos muy seguido. Por ejemplo, For example, a cola of messages that need to be shown on-screen.

Hay otro caso de uso para los arrays -- la estructura de datos llamada [pila](<https://es.wikipedia.org/wiki/Pila_(inform%C3%A1tica)>).

Soporta dos operaciones:

-   `push` agregar elementos al final.
-   `pop` quitar elementos del final.

Así nuevos elementos son añadidos o quitados siempre del "final".

Una pila o "stack" es usualmente ilustrada como un maso de cartas: nuevas cartas se agregan de arriba y se sacan de arriba:

![](stack.svg)

En una pila, el último item agregado es recibido primero, For stacks, the latest pushed item is received first, that's also called LIFO (Last-In-First-Out) principle. For colas, we have FIFO (First-In-First-Out).

Arrays in JavaScript can work both as a cola and as a stack. They allow you to add/remove elements both to/from the beginning or the end.

In computer science the data structure that allows it is called [deque](https://en.wikipedia.org/wiki/Double-ended_queue).

**Methods that work with the end of the array:**

`pop`
: Extracts the last element of the array and returns it:

    ```js run
    let fruits = ["Apple", "Orange", "Pear"];

    alert( fruits.pop() ); // remove "Pear" and alert it

    alert( fruits ); // Apple, Orange
    ```

`push`
: Append the element to the end of the array:

    ```js run
    let fruits = ["Apple", "Orange"];

    fruits.push("Pear");

    alert( fruits ); // Apple, Orange, Pear
    ```

    The call `fruits.push(...)` is equal to `fruits[fruits.length] = ...`.

**Methods that work with the beginning of the array:**

`shift`
: Extracts the first element of the array and returns it:

    ```js
    let fruits = ["Apple", "Orange", "Pear"];

    alert( fruits.shift() ); // remove Apple and alert it

    alert( fruits ); // Orange, Pear
    ```

`unshift`
: Add the element to the beginning of the array:

    ```js
    let fruits = ["Orange", "Pear"];

    fruits.unshift('Apple');

    alert( fruits ); // Apple, Orange, Pear
    ```

Methods `push` and `unshift` can add multiple elements at once:

```js run
let fruits = ['Apple'];

fruits.push('Orange', 'Peach');
fruits.unshift('Pineapple', 'Lemon');

// ["Pineapple", "Lemon", "Apple", "Orange", "Peach"]
alert(fruits);
```

## Internals

An array is a special kind of object. The square brackets used to access a property `arr[0]` actually come from the object syntax. Numbers are used as keys.

They extend objects providing special methods to work with ordered collections of data and also the `length` property. But at the core it's still an object.

Remember, there are only 7 basic types in JavaScript. Array is an object and thus behaves like an object.

For instance, it is copied by reference:

```js run
let fruits = ['Banana'];

let arr = fruits; // copy by reference (two variables reference the same array)

alert(arr === fruits); // true

arr.push('Pear'); // modify the array by reference

alert(fruits); // Banana, Pear - 2 items now
```

...But what makes arrays really special is their internal representation. The engine tries to store its elements in the contiguous memory area, one after another, just as depicted on the illustrations in this chapter, and there are other optimizations as well, to make arrays work really fast.

But they all break if we quit working with an array as with an "ordered collection" and start working with it as if it were a regular object.

For instance, technically we can do this:

```js
let fruits = []; // make an array

fruits[99999] = 5; // assign a property with the index far greater than its length

fruits.age = 25; // create a property with an arbitrary name
```

That's possible, because arrays are objects at their base. We can add any properties to them.

But the engine will see that we're working with the array as with a regular object. Array-specific optimizations are not suited for such cases and will be turned off, their benefits disappear.

The ways to misuse an array:

-   Add a non-numeric property like `arr.test = 5`.
-   Make holes, like: add `arr[0]` and then `arr[1000]` (and nothing between them).
-   Fill the array in the reverse order, like `arr[1000]`, `arr[999]` and so on.

Please think of arrays as special structures to work with the _ordered data_. They provide special methods for that. Arrays are carefully tuned inside JavaScript engines to work with contiguous ordered data, please use them this way. And if you need arbitrary keys, chances are high that you actually require a regular object `{}`.

## Performance

Methods `push/pop` run fast, while `shift/unshift` are slow.

![](array-speed.svg)

Why is it faster to work with the end of an array than with its beginning? Let's see what happens during the execution:

```js
fruits.shift(); // take 1 element from the start
```

It's not enough to take and remove the element with the number `0`. Other elements need to be renumbered as well.

The `shift` operation must do 3 things:

1. Remove the element with the index `0`.
2. Move all elements to the left, renumber them from the index `1` to `0`, from `2` to `1` and so on.
3. Update the `length` property.

![](array-shift.svg)

**The more elements in the array, the more time to move them, more in-memory operations.**

The similar thing happens with `unshift`: to add an element to the beginning of the array, we need first to move existing elements to the right, increasing their indexes.

And what's with `push/pop`? They do not need to move anything. To extract an element from the end, the `pop` method cleans the index and shortens `length`.

The actions for the `pop` operation:

```js
fruits.pop(); // take 1 element from the end
```

![](array-pop.svg)

**The `pop` method does not need to move anything, because other elements keep their indexes. That's why it's blazingly fast.**

The similar thing with the `push` method.

## Loops

One of the oldest ways to cycle array items is the `for` loop over indexes:

```js run
let arr = ["Apple", "Orange", "Pear"];

*!*
for (let i = 0; i < arr.length; i++) {
*/!*
  alert( arr[i] );
}
```

But for arrays there is another form of loop, `for..of`:

```js run
let fruits = ['Apple', 'Orange', 'Plum'];

// iterates over array elements
for (let fruit of fruits) {
    alert(fruit);
}
```

The `for..of` doesn't give access to the number of the current element, just its value, but in most cases that's enough. And it's shorter.

Technically, because arrays are objects, it is also possible to use `for..in`:

```js run
let arr = ["Apple", "Orange", "Pear"];

*!*
for (let key in arr) {
*/!*
  alert( arr[key] ); // Apple, Orange, Pear
}
```

But that's actually a bad idea. There are potential problems with it:

1. The loop `for..in` iterates over _all properties_, not only the numeric ones.

    There are so-called "array-like" objects in the browser and in other environments, that _look like arrays_. That is, they have `length` and indexes properties, but they may also have other non-numeric properties and methods, which we usually don't need. The `for..in` loop will list them though. So if we need to work with array-like objects, then these "extra" properties can become a problem.

2. The `for..in` loop is optimized for generic objects, not arrays, and thus is 10-100 times slower. Of course, it's still very fast. The speedup may only matter in bottlenecks or seem irrelevant. But still we should be aware of the difference.

Generally, we shouldn't use `for..in` for arrays.

## A word about "length"

The `length` property automatically updates when we modify the array. To be precise, it is actually not the count of values in the array, but the greatest numeric index plus one.

For instance, a single element with a large index gives a big length:

```js run
let fruits = [];
fruits[123] = 'Apple';

alert(fruits.length); // 124
```

Note that we usually don't use arrays like that.

Another interesting thing about the `length` property is that it's writable.

If we increase it manually, nothing interesting happens. But if we decrease it, the array is truncated. The process is irreversible, here's the example:

```js run
let arr = [1, 2, 3, 4, 5];

arr.length = 2; // truncate to 2 elements
alert(arr); // [1, 2]

arr.length = 5; // return length back
alert(arr[3]); // undefined: the values do not return
```

So, the simplest way to clear the array is: `arr.length = 0;`.

## new Array() [#new-array]

There is one more syntax to create an array:

```js
let arr = *!*new Array*/!*("Apple", "Pear", "etc");
```

It's rarely used, because square brackets `[]` are shorter. Also there's a tricky feature with it.

If `new Array` is called with a single argument which is a number, then it creates an array _without items, but with the given length_.

Let's see how one can shoot themself in the foot:

```js run
let arr = new Array(2); // will it create an array of [2] ?

alert(arr[0]); // undefined! no elements.

alert(arr.length); // length 2
```

In the code above, `new Array(number)` has all elements `undefined`.

To evade such surprises, we usually use square brackets, unless we really know what we're doing.

## Multidimensional arrays

Arrays can have items that are also arrays. We can use it for multidimensional arrays, to store matrices:

```js run
let matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]];

alert(matrix[1][1]); // the central element
```

## toString

Arrays have their own implementation of `toString` method that returns a comma-separated list of elements.

For instance:

```js run
let arr = [1, 2, 3];

alert(arr); // 1,2,3
alert(String(arr) === '1,2,3'); // true
```

Also, let's try this:

```js run
alert([] + 1); // "1"
alert([1] + 1); // "11"
alert([1, 2] + 1); // "1,21"
```

Arrays do not have `Symbol.toPrimitive`, neither a viable `valueOf`, they implement only `toString` conversion, so here `[]` becomes an empty string, `[1]` becomes `"1"` and `[1,2]` becomes `"1,2"`.

When the binary plus `"+"` operator adds something to a string, it converts it to a string as well, so the next step looks like this:

```js run
alert('' + 1); // "1"
alert('1' + 1); // "11"
alert('1,2' + 1); // "1,21"
```

## Summary

Array is a special kind of object, suited to storing and managing ordered data items.

-   The declaration:

    ```js
    // square brackets (usual)
    let arr = [item1, item2...];

    // new Array (exceptionally rare)
    let arr = new Array(item1, item2...);
    ```

    The call to `new Array(number)` creates an array with the given length, but without elements.

-   The `length` property is the array length or, to be precise, its last numeric index plus one. It is auto-adjusted by array methods.
-   If we shorten `length` manually, the array is truncated.

We can use an array as a deque with the following operations:

-   `push(...items)` adds `items` to the end.
-   `pop()` removes the element from the end and returns it.
-   `shift()` removes the element from the beginning and returns it.
-   `unshift(...items)` adds items to the beginning.

To loop over the elements of the array:

-   `for (let i=0; i<arr.length; i++)` -- works fastest, old-browser-compatible.
-   `for (let item of arr)` -- the modern syntax for items only,
-   `for (let i in arr)` -- never use.

We will return to arrays and study more methods to add, remove, extract elements and sort arrays in the chapter <info:array-methods>.
