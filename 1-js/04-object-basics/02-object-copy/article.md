<<<<<<< HEAD
# Copia de objetos, referencias

Una de las diferencias fundamentales de los objetos respecto a los primitivos es que son almacenados y copiados "por referencia".

Los valores primitivos: strings, numbers, booleans -- son asignados y copiados "como un valor completo".

Por ejemplo:
=======
# Object references and copying

One of the fundamental differences of objects versus primitives is that objects are stored and copied "by reference", as opposed to primitive values: strings, numbers, booleans, etc -- that are always copied "as a whole value".

That's easy to understand if we look a bit "under a cover" of what happens when we copy a value.

Let's start with a primitive, such as a string.

Here we put a copy of `message` into `phrase`:
>>>>>>> d6e88647b42992f204f57401160ebae92b358c0d

```js
let message = "Hello!";
let phrase = message;
```

Como resultado tenemos dos variables independientes, cada una almacenando la cadena `"Hello!"`.

![](variable-copy-value.svg)

<<<<<<< HEAD
Los objetos no son así.

**Una variable no almacena el objeto mismo sino su "dirección en memoria", en otras palabras "una referencia" a él.**

Aquí tenemos una imagen del objeto:
=======
Quite an obvious result, right?

Objects are not like that.

**A variable assigned to an object stores not the object itself, but its "address in memory", in other words "a reference" to it.**

Let's look at an example of such variable:
>>>>>>> d6e88647b42992f204f57401160ebae92b358c0d

```js
let user = {
  name: "John"
};
```

And here's how it's actually stored in memory:

![](variable-contains-reference.svg)

<<<<<<< HEAD
Aquí, el objeto es almacenado en algún lugar de la memoria. Y la variable `user` tiene una "referencia" a él.
=======
The object is stored somewhere in memory (at the right of the picture), while the `user` variable (at the left) has a "reference" to it.

We may think of an object variable, such as `user`, as of a sheet of paper with the address.

When we perform actions with the object, e.g. take a property `user.name`, JavaScript engine looks into that address and performs the operation on the actual object.

Now here's why it's important.
>>>>>>> d6e88647b42992f204f57401160ebae92b358c0d

**Cuando una variable de objeto es copiada -- la referencia es copiada, el objeto no es duplicado.**

Por ejemplo:

```js no-beautify
let user = { name: "John" };

let admin = user; // copia la referencia
```

Ahora tenemos dos variables, cada una con una referencia al mismo objeto:

![](variable-copy-reference.svg)

<<<<<<< HEAD
Podemos usar cualquiera de las variables para acceder al objeto y modificar su contenido:
=======
As you can see, there's still one object, now with two variables that reference it.

We can use any variable to access the object and modify its contents:
>>>>>>> d6e88647b42992f204f57401160ebae92b358c0d

```js run
let user = { name: 'John' };

let admin = user;

*!*
admin.name = 'Pete'; // cambiado por la referencia "admin"
*/!*

alert(*!*user.name*/!*); // 'Pete', los cambios se ven desde la referencia "user"
```

<<<<<<< HEAD
El ejemplo anterior demuestra que solamente hay un único objeto. Como si tuviéramos un gabinete con dos llaves y usáramos una de ellas (`admin`) para accederlo. Si más tarde usamos la llave (`user`), podemos ver los cambios.

## Comparación por referencia

En los objetos, los operadores de igualdad `==` e igualdad estricta `===` funcionan exactamente igual.

**Dos objetos son iguales solamente si ellos son el mismo objeto.**

Aquí dos variables referencian el mismo objeto, así que ellos son iguales:
=======

It's just as if we had a cabinet with two keys and used one of them (`admin`) to get into it. Then, if we later use another key (`user`) we can see changes.

## Comparison by reference

Two objects are equal only if they are the same object.

For instance, here `a` and `b` reference the same object, thus they are equal:
>>>>>>> d6e88647b42992f204f57401160ebae92b358c0d

```js run
let a = {};
let b = a; // copia la referencia

alert( a == b ); // true, verdadero. Ambas variables hacen referencia al mismo objeto
alert( a === b ); // true
```

<<<<<<< HEAD
Y aquí dos objetos independientes no son iguales, incluso estando ambos vacíos:
=======
And here two independent objects are not equal, even though they look alike (both are empty):
>>>>>>> d6e88647b42992f204f57401160ebae92b358c0d

```js run
let a = {};
let b = {}; // dos objetos independientes

alert( a == b ); // false
```

<<<<<<< HEAD
Para comparaciones como `obj1 > obj2`, o comparaciones contra un primitivo `obj == 5`, los objetos son convertidos a primitivos. Estudiaremos cómo funciona la conversión de objetos pronto, pero a decir verdad tales comparaciones ocurren raramente y suelen ser errores de código.
=======
For comparisons like `obj1 > obj2` or for a comparison against a primitive `obj == 5`, objects are converted to primitives. We'll study how object conversions work very soon, but to tell the truth, such comparisons are needed very rarely, usually they appear as a result of a programming mistake.
>>>>>>> d6e88647b42992f204f57401160ebae92b358c0d

## Clonación y mezcla, Object.assign

Entonces copiar una variable de objeto crea una referencia adicional al mismo objeto.

Pero ¿si necesitamos duplicar un objeto? ¿Crear una copia independiente, un clon?

Eso también es factible, pero un poco más difícil porque no hay un método incorporado para eso en JavaScript. En realidad, eso es raramente necesario. Copiar por referencia está bien la mayoría de las veces.

Pero si realmente queremos eso, necesitamos crear un nuevo objeto y replicar la estructura del existente iterando a través de sus propiedades y copiándolas en el nivel primitivo.

Como esto:

```js run
let user = {
  name: "John",
  age: 30
};

*!*
let clone = {}; // el nuevo objeto vacío

// copiemos todas las propiedades de user en él
for (let key in user) {
  clone[key] = user[key];
}
*/!*

// ahora clone es un objeto totalmente independiente con el mismo contenido
clone.name = "Pete"; // cambiamos datos en él

alert( user.name ); // John aún está en el objeto original
```

También podemos usar el método [Object.assign](mdn:js/Object/assign) para ello.

La sintaxis es:

```js
Object.assign(dest, [src1, src2, src3...])
```

- El primer argumento `dest` es el objeto destinatario.
- Los siguientes argumentos `src1, ..., srcN` (tantos como sea necesario) son objetos fuentes.
- Esto copia todas las propiedades de todos los objetos fuentes `src1, ..., srcN` dentro del destino `dest`. En otras palabras, las propiedades de todos los argumentos, comenzando desde el segundo, son copiadas en el primer objeto.
- El llamado devuelve `dest`.

Por ejemplo, podemos usarlo para combinar distintos objetos en uno:
```js
let user = { name: "John" };

let permissions1 = { canView: true };
let permissions2 = { canEdit: true };

*!*
// copia todas las propiedades desde permissions1 y permissions2 en user
Object.assign(user, permissions1, permissions2);
*/!*

// ahora user = { name: "John", canView: true, canEdit: true }
```

Si la propiedad por copiar ya existe, se sobrescribe:

```js run
let user = { name: "John" };

Object.assign(user, { name: "Pete" });

alert(user.name); // ahora user = { name: "Pete" }
```

También podemos usar `Object.assign` reemplazando el bucle `for..in` para hacer una clonación simple:

```js
let user = {
  name: "John",
  age: 30
};

*!*
let clone = Object.assign({}, user);
*/!*
```

Copia todas las propiedades de `user` en un objeto vacío y lo devuelve.

## Clonación anidada

Hasta ahora asumimos que todas las propiedades de `user` son primitivas. Pero las propiedades pueden ser referencias a otros objetos. ¿Qué hacer con ellas?

Como esto:
```js run
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50
  }
};

alert( user.sizes.height ); // 182
```

Ahora no es suficiente copiar `clone.sizes = user.sizes`, porque `user.sizes` es un objeto y será copiado por referencia. Entonces `clone` y `user` compartirán las mismas tallas (.sizes):

Como esto:

```js run
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50
  }
};

let clone = Object.assign({}, user);

alert( user.sizes === clone.sizes ); // true, el mimo objeto

// user y clone comparten sizes
user.sizes.width++;       // cambia la propiedad en un lugar
alert(clone.sizes.width); // 51, ve el resultado desde el otro
```

Para corregir esto, debemos usar un bucle de clonación que examine cada valor de `user[key]` y, si es un objeto, replicar su estructura también. Esto es llamado "clonación profunda".

Hay un algoritmo estándar para clonación profunda que maneja este caso y otros más complicados llamado [Structured cloning algorithm](https://html.spec.whatwg.org/multipage/structured-data.html#safe-passing-of-structured-data).

Podemos usar recursividad para implementarlo. O, para no inventar la rueda, tomar una implementación existente, por ejemplo [_.cloneDeep(obj)](https://lodash.com/docs#cloneDeep) de la librería JavaScript [lodash](https://lodash.com).

## Resumen

Los objetos son asignados y copiados por referencia. En otras palabras, una variable almacena no el valor del objeto sino una referencia (dirección de memoria) del valor. Entoncess copiar tal variable o pasarla como argumento de función copia la referencia, no el objeto.

Todas la operaciones a través de referencias copiadas (como agregar y borrar propiedades) son efectuadas en el mismo y único objeto .

Si queremos conseguir una "copia real" (un clon), podemos usar: Una "clonación superficial" por medio de la función `Object.assign` (con los objetos anidados copiados por referencia), o una "clonación profunda" con una función como [_.cloneDeep(obj)](https://lodash.com/docs#cloneDeep).
