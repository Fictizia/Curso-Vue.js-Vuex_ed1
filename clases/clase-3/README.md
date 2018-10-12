## Planning del día 

1. Repaso clase anterior
2. Formularios
3. Comunicación entre componentes
4. Propiedades computadas y Watchers
5. Ejercicio "Registro de usuarios"
6. Conceptos avanzados de Vue
9. Ejercicio "Carrito de la compra"

## Índice 

* [1. Formularios](#1.-Formularios)
    * [1.1. Uso básico](#1.1.-Uso-básico)
        * [1.1.1. Texto](#1.1.1.-Texto)
        * [1.1.2. Texto multilínea](#1.1.2.-Texto-multilínea)
        * [1.1.3. Checkbox](#1.1.3.-Checkbox)
        * [1.1.4. Radio](#1.1.4.-Radio)
        * [1.1.5. Select](#1.1.5.-Select)
    * [1.2. Enlazando valores](#1.2.-Enlazando-valores)
        * [1.2.1. Checkbox](#1.2.1.-Checkbox)
        * [1.2.2. Radio](#1.2.2.-Radio)
        * [1.2.3. Select](#1.2.3-Select)
    * [1.3. Modificadores](#1.3.-Modificadores)
* [2. Comunicación entre componentes](#2.-Comunicación-entre-componentes)
    * [2.1. Props](#2.1.-Props)
        * [2.1.1. Nomenclatura de las props](#2.1.1.-Nomenclatura-de-las-props)
        * [2.1.2. Tipado de las props](#2.1.2.-Tipado-de-las-props)
        * [2.1.3. Pasando props estáticas o dinámicas](#2.1.3.-Pasando-props-estáticas-o-dinámicas)
            * [2.1.3.1. Pasando un número](#2.1.3.1.-Pasando-un-número)
            * [2.1.3.2. Pasando un booleano](#2.1.3.2.-Pasando-un-booleano)
            * [2.1.3.3. Pasando un array](#2.1.3.3.-Pasando-un-array)
            * [2.1.3.4. Pasando un objeto](#2.1.3.4.-Pasando-un-objeto)
            * [2.1.3.5. Pasando las propiedades de un objeto](#2.1.3.5.-Pasando-las-propiedades-de-un-objeto)
        * [2.1.4. Flujos en una única dirección](#2.1.4.-Flujos-en-una-única-dirección)
        * [2.1.5. Validación de props](#2.1.5.-Validación-de-props)
        * [2.1.6. Atributos que no son props](#2.1.6.-Atributos-que-no-son-props)
    * [2.2. Evento personalizados](#2.2.-Evento-personalizados)
        * [2.2.1. Nomenclatura de los eventos personalizados](#2.2.1.-Nomenclatura-de-los-eventos-personalizados)
        * [2.2.2. Enlazando eventos nativos a un componente](#2.2.2.-Enlazando-eventos-nativos-a-un-componente)
        * [2.2.3. El modificador `.sync`](#2.2.3.-El-modificador-.sync)
* [3. Propiedades computadas y watchers](3.-Propiedades-computadas-y-watchers)
    * [3.1. Ejemplo básico](#3.1-Ejemplo-básico)
    * [3.2. Propiedades computadas vs Métodos](#3.2.-Propiedades-computadas-vs-Métodos)
    * [3.3. Propiedades computadas vs propiedades observadas](#3.3.-Propiedades-computadas-vs-propiedades-observadas)
    * [3.4. Asignando valores a propiedades computadas](#3.4.-Asignando-valores-a-propiedades-computadas)
* [4. Ejercicio "Registro de usuarios"](#Ejercicio-"Registro-de-usuarios")
* [5. Conceptos avanzados de Vue](#5.-Conceptos-avanzados-de-Vue)
    * [5.1. Slots](#5.1.-Slots)
        * [5.1.1. Slot Content](#5.1.1.-Slot-Content)
        * [5.1.2. Nombrado de slots](#5.1.2.-Nombrado-de-slots)
        * [5.1.3. Slot por defecto](#5.1.3.-Slot-por-defecto)
        * [5.1.4. Scope de compilación](#5.1.4.-Scope-de-compilación)
    * [5.2. Filtros](#5.2.-Filtros)
    * [5.3. Directivas](#5.3.-Directivas)
        * [5.3.1. Ciclo de vida de una directiva y los hooks](#5.3.1.-Ciclo-de-vida-de-una-directiva-y-los-hooks)
        * [5.3.2. Parámetros de los hooks](#5.3.2.-Parámetros-de-los-hooks)
        * [5.3.3. Shorthand](#5.3.3.-Shorthand)
        * [5.3.4. Objeto como parámetro de la directiva](#5.3.4.-Objeto-como-parámetro-de-la-directiva)
    * [5.4. Mixins](#5.4.-Mixins)
        * [5.4.1. Cómo mezcla los objetos](#5.4.1.-Cómo-mezcla-los-objetos)
        * [5.4.2. Mixins globales](#5.4.2.-Mixins-globales)

# 1. Formularios

## 1.1. Uso básico

Al igual que nosotros podemos enlazar datos de un modelo para que se muestren por pantalla, podemos enlazar elementos de formulario como `input` o `select` para obtener datos de los usuarios y guardarlos en nuestros modelos.

La manera de hacerlo es por medio de la directiva `v-model`. Es una directiva que nos permite hacer doble enlace de datos y que no es más que azucar sintáctico de todo lo que ya hemos enseñado en el curso.

> TIP: recuerda que cuando indcas `v-model` en un elemento de tu formulario, el valor inicial de estos elementos será ignorado. Si necesitas un valor por defecto en tu formulario, inicializa tus modelos en `data`.

### 1.1.1. Texto

Empecemos por el uso más simple:

```html
<input v-model="message" placeholder="Edítame">
<p>El mensaje es: {{ message }}</p>
```

### 1.1.2. Texto multilínea

Con los `textarea` tambien funciona:

```html
<span>El mensaje multilínea es:</span>
<p style="white-space: pre-line;">{{ message }}</p>
<br>
<textarea v-model="message" placeholder="Añade múltiples líneas"></textarea>
```

### 1.1.3. Checkbox

Si queremos guardar un valor booleano, podemos usar un simple checkbox:

```html
<input type="checkbox" id="checkbox" v-model="checked">
<label for="checkbox">{{ checked }}</label>
```

Y si queremos guardar el valor de varias opciones:

```html
<div id='example-3'>
  <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
  <label for="jack">Jack</label>
  <input type="checkbox" id="john" value="John" v-model="checkedNames">
  <label for="john">John</label>
  <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
  <label for="mike">Mike</label>
  <br>
  <span>Nombres confirmados: {{ checkedNames }}</span>
</div>
```
```js
new Vue({
  el: '#example-3',
  data: {
    checkedNames: []
  }
})
```
Se guardarán en un array. Algo muy útil si necesitamos iterar entre las seleccionadas.

### 1.1.4. Radio

Si tenemos que elegir entre una de las opciones dadas, usaremos un `radiobutton`:

```html
<input type="radio" id="one" value="Uno" v-model="picked">
<label for="one">Uno</label>
<br>
<input type="radio" id="two" value="Dos" v-model="picked">
<label for="two">Dos</label>
<br>
<span>Elección: {{ picked }}</span>
```

### 1.1.5. Select

Si tenemos un `select` de opción única:

```html
<select v-model="selected">
  <option disabled value="">Por favor, selecciona uno</option>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
<span>Seleccionado: {{ selected }}</span>
```
```js
new Vue({
  el: '...',
  data: {
    selected: ''
  }
})
```

> TIP: Recuerda que si el valor inicial no corresponde con ninguno de los valores seleccionables, el `select` se quedará en un estado de no seleccionado.

Y si contamos con una selección múltiple: 

```html
<select v-model="selected" multiple>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
<br>
<span>Seleccionados: {{ selected }}</span>
```

Si queremos combinarlo con un `v-for`:

```html
<select v-model="selected">
  <option v-for="option in options" v-bind:value="option.value">
    {{ option.text }}
  </option>
</select>
<span>Seleccionado: {{ selected }}</span>
```
```js
new Vue({
  el: '...',
  data: {
    selected: 'A',
    options: [
      { text: 'Uno', value: 'A' },
      { text: 'Dos', value: 'B' },
      { text: 'Tres', value: 'C' }
    ]
  }
})
```

## 1.2. Enlazando valores

Como vemos, con `v-model` podemos enlazar valores que por lo general son estáticos. Veamos otras vez estos casos:

```html
<!-- `picked` es un string "a" cuando pulsamos -->
<input type="radio" v-model="picked" value="a">

<!-- `toggle` fluctua entre true o false -->
<input type="checkbox" v-model="toggle">

<!-- `selected` es un string "abc" cuando la primera opción es selccionada -->
<select v-model="selected">
  <option value="abc">ABC</option>
</select>
```

Pero a veces poríamos querer tener valores dinámicos. Para conseguirlo, combinaríamos `v-model` con `v-bind`. Veamos cómo:

### 1.2.1. Checkbox

```html
<input
  type="checkbox"
  v-model="toggle"
  true-value="yes"
  false-value="no"
>
```
```js
// cuando marcas:
vm.toggle === 'yes'
// cuando desmarcas:
vm.toggle === 'no'
```

### 1.2.2. Radio

```html
<input type="radio" v-model="pick" v-bind:value="a">
```
```js
// cuando marcas:
vm.pick === vm.a
```

### 1.2.3. Select

```html
<select v-model="selected">
  <!-- podemos indicar un objecto literal -->
  <option v-bind:value="{ number: 123 }">123</option>
</select>
```
```js
// cuando seleccionamos:
typeof vm.selected // => 'object'
vm.selected.number // => 123
```


## 1.3. Modificadores


* `.number`

Si necesitamos que un valor d eun modelo sea de tipo `Number` en vez de string, podemos hacer una conversión instantanea:

```html
<input v-model.number="age" type="number">
```

* `.trim`

Lo mismo si necesitas quitar espacios de la cadena que indica el usuario. Tendríamos un `trim` automático:

```html
<input v-model.trim="msg">
```

# 2. Comunicación entre componentes

Hemos hablado mucho sobre cómo enlazar y obtener datos de la interfaz. Hemos tambien habla de cómo crear componentes simples y de su ciclo de vida, pero no hemos hablado de cómo podemos comunicarnos entre componentes.

La comuncación entre componentes es la siguiente:

![Comunicacion de componentes](imgs/comunicacion.png)

Hablemos del paso de propiedades y de la emisión de eventos:

## 2.1. Props

Las props de un componente son los parámetros de entrada que un componente permite para configurar su comportamiento por defecto. Un componente padre puede instanciar y configurar un componente visual por medio de props.

Vamos a ver cómo se usan y todas sus peculiaridades:

### 2.1.1. Nomenclatura de las props

Para indicar las propiedades de un componente simplemente tenemos que indicar un array con las propiedades en `props` de un componente. Podemos nombrar las propiedades como camelCase y kebab-case:

```js
Vue.component('blog-post', {
  // camelCase in JavaScript
  props: ['postTitle'],
  template: '<h3>{{ postTitle }}</h3>'
})
```

Cuando queramos usar un componente, simplemente tendremos que hacer uso de él e indicar las propiedades requeridas. En el HTML las props tienen que estar escritas en kebab-case siempre:

```js
<!-- kebab-case en HTML -->
<blog-post post-title="hello!"></blog-post>
```

### 2.1.2. Tipado de las props

Las props son tipadas. Esto quiere decir que nosotros tenemos que indicar al usuario de nuestro componente qué tipo de datos se puede incluir en una prop. 

Si indicamos las props como un array de props, automáticamente son de tipo `string`.

```js
props: ['title', 'likes', 'isPublished', 'commentIds', 'author']
```

Por tanto, la mejor forma de declarar props en un componente es por medio de un objeto donde indiquemos en tipo de cada prop:

```js
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object
}
```

Esto es útil, porque en tiempo de desarrollo, las developer tools nos indicarán si estamos haciendo un mal uso de estas propiedades. Nos ayuda a crear mejores componentes.

### 2.1.3. Pasando props estáticas o dinámicas

Nosotros podemos pasar datos estáticos o dinamicos a nuestras propiedades.

Por ejemplo, si no indico un `v-bind:` o un `:`, directamente, el compilador entenderá que le estamos pasando un valor 'hardcodeado':

```html
<blog-post title="My journey with Vue"></blog-post>
```

Pero puedo indicar variables, para que los componentes reaccionen a cambios del padre:

```html
<!-- Asignamos dinámicamente el valor de la variable -->
<blog-post v-bind:title="post.title"></blog-post>

<!-- Asignamos dinámicamente el valor de una expresión compleja -->
<blog-post v-bind:title="post.title + ' by ' + post.author.name"></blog-post>
```

#### 2.1.3.1. Pasando un número

Cuidado cuando tengamos pasar un número a un componente y lo queramos hacer de manera estática ya que el compilador lo interpretará como un string, por eso, en estos casos, siempre lo indicaremos con `v-bind` o su shorthand (`:`):

```html
<!-- Aunque `42` es estático, necesitamos v-bind para indicarle el valor a Vuet -->
<blog-post v-bind:likes="42"></blog-post>

<!-- Asignamos dinámicamente el valor de la variable -->
<blog-post v-bind:likes="post.likes"></blog-post>
```

#### 2.1.3.2. Pasando un booleano

Cuando indicamos un valor booleano, siemplemente podemos marcar la propiedad. Esto siempr eindicará un `true`:

```html
<blog-post is-published></blog-post>
```
Si queremos pasar un `false`, lo recomendable es poner un valor por defecto a la prop, o si no, indicarlo dinámicamente:

```html
<blog-post v-bind:is-published="false"></blog-post>

<blog-post v-bind:is-published="post.isPublished"></blog-post>
```

#### 2.1.3.3. Pasando un array

Podemos pasar un array de manera dinámica tambien a un componente:

```html
<blog-post v-bind:comment-ids="[234, 266, 273]"></blog-post>

<blog-post v-bind:comment-ids="post.commentIds"></blog-post>
```

#### 2.1.3.4. Pasando un objeto

O los datos de un objeto:

```html
<blog-post v-bind:author="{ name: 'Veronica', company: 'Veridian Dynamics' }"></blog-post>

<blog-post v-bind:author="post.author"></blog-post>
```

#### 2.1.3.5. Pasando las propiedades de un objeto

En vez de ir pasando una a una las props a un componente, podemos pasarle todos los datos a la vez. Teniendo este objeto que lo queremos pasar a un componente:

```js
post: {
  id: 1,
  title: 'My Journey with Vue'
}
```

Esta forma de instanciarlo:

```html
<blog-post v-bind="post"></blog-post>
```

Y esta:

```html
<blog-post
  v-bind:id="post.id"
  v-bind:title="post.title"
></blog-post>
```

Hacen lo mismo

### 2.1.4. Flujos en una única dirección

Tenemos que recordar que las props solo pueden ser actualizadas en una única dirección, es decir, solo el padre puede actualizar las props de un componente. Un hijo no puede alterar el valor de las props para que su padre haga cosas.

Esto tambien nos está indicando que hay que tener cuidado si internamente de un componente intentamos mutar una prop. Si solo los padres tienen la prioridad en la mutación de una prop (es decir, que el ultimo valor indicado por un componente padre es el que predomina como valor en las props pasadas a los hijos), las mutaciones que los hijos hayan hecho, desaparecerán:

Además, habrá dos casos donde querremos poder mutar props dentro de un componente hijo:

1. **Cuando una prop es usada como valor inicial y queremos luego poder manipular ese dato**. En ese caso, es mejor usar la prop para iniciar el valor del data de la siguiente manera:

```js
props: ['initialCounter'],
data: function () {
  return {
    counter: this.initialCounter
  }
}
```

2. **Cuando internamente, necesitamos transformar el valor dentro del componente**. Lo haríamos con una computada (explicaremos en un rato):

```js
props: ['size'],
computed: {
  normalizedSize: function () {
    return this.size.trim().toLowerCase()
  }
}
```

### 2.1.5. Validación de props

Por otro lado, tenemos diferentes maneras de validar que los datos que nos pasan son correctos:

```js
Vue.component('my-component', {
  props: {
    // Comprobación de tipos básica (`null` relaciona con cualquier tipo)
    propA: Number,

    // Posibilidad de multiples tipos
    propB: [String, Number],

    // Prop obligatoria
    propC: {
      type: String,
      required: true
    },

    // Number con un valor por defecto
    propD: {
      type: Number,
      default: 100
    },

    // Objecto con valor por defecto
    propE: {
      type: Object,
      // Los objecto y array tiene que setear por defecto con una función factoría
      default: function () {
        return { message: 'hello' }
      }
    },

    // Validador personalizado
    propF: {
      validator: function (value) {
        // The value must match one of these strings
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})
```

> TIP: recuerda que las props serán validades antes de instanciar el componente. Esto quiere decir que en las validaciones no tendrás acceso ni a `data` ni a `computed`.

**Comprobación de tipos**

Los tipos por defecto validos, corresponden con todos los tipos de JavaScript:

* String
* Number
* Boolean
* Array
* Object
* Date
* Function
* Symbol

Podemos crear tipos nuevos:

```js
function Person (firstName, lastName) {
  this.firstName = firstName
  this.lastName = lastName
}
```

Y lo podemos usar así:

```js
Vue.component('blog-post', {
  props: {
    author: Person
  }
})
```

### 2.1.6. Atributos que no son props

Puede que algunos componentes necesiten atribútos que no tienen un mapeo con una propiedad. ALgunas librerías de terceros así lo podrían pedir. Estos atributos se añaden a su nodo raíz.

Por ejemplo:

```html
<bootstrap-date-input data-date-picker="activated"></bootstrap-date-input>
```

## 2.2. Evento personalizados

### 2.2.1. Nomenclatura de los eventos personalizados

```js
this.$emit('myEvent')
```

```html
<my-component v-on:my-event="doSomething"></my-component>
```

### 2.2.2. Enlazando eventos nativos a un componente

```html
<base-input v-on:focus.native="onFocus"></base-input>
```


### 2.2.3. El modificador `.sync`

```js
this.$emit('update:title', newTitle)
```

```html
<text-document
  v-bind:title="doc.title"
  v-on:update:title="doc.title = $event"
></text-document>
```

```html
<text-document v-bind.sync="doc"></text-document>
```

# 3. Propiedades computadas y watchers

```html
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```

## 3.1. Ejemplo básico

```html
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```

```js
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // a computed getter
    reversedMessage: function () {
      // `this` points to the vm instance
      return this.message.split('').reverse().join('')
    }
  }
})
```

```js
console.log(vm.reversedMessage) // => 'olleH'
vm.message = 'Goodbye'
console.log(vm.reversedMessage) // => 'eybdooG'
```

## 3.2. Propiedades computadas vs Métodos

```html
<p>Reversed message: "{{ reverseMessage() }}"</p>
```
```js
// en el componente
methods: {
  reverseMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```

```js
computed: {
  now: function () {
    return Date.now()
  }
}
```

## 3.3. Propiedades computadas vs propiedades observadas

```html
<div id="demo">{{ fullName }}</div>
```
```js
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
```

```js
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```

Mucho mejor, ¿no? :)

## 3.4. Asignando valores a propiedades computadas

```js
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```

# 4. Ejercicio "Registro de usuarios"

* Hagmos un formulario de usuarios
* Tenemos que guardar:
    * el nombre (máx 30), 
    * seleccionar foto desde el ordenador
    * los apellidos (máx 50), 
    * edad,
    * páis (se tiene que cargar por API - restcountries.eu) 
    * redes sociales que tiene
    * tiene que aceptar las condiciones de uso
* Hay que mostrar los caracteres que me quedan disponibles en los que tengan máximo
* Tenemos que hacer validaciones
* Tenemos que mostrar los errores de validación
* Cuando el usuario pulse Aceptar, los datos nos tiene que salir convertidos en una carta mostrando todo

# 5. Conceptos avanzados de Vue

## 5.1. Slots

### 5.1.1. Slot Content

```html
<navigation-link url="/profile">
  Your Profile
</navigation-link>
```

```html
<a
  v-bind:href="url"
  class="nav-link"
>
  <slot></slot>
</a>
```

```html
<navigation-link url="/profile">
  <!-- Add a Font Awesome icon -->
  <span class="fa fa-user"></span>
  Your Profile
</navigation-link>
```

```html
<navigation-link url="/profile">
  <!-- Use a component to add an icon -->
  <font-awesome-icon name="user"></font-awesome-icon>
  Your Profile
</navigation-link>
```

### 5.1.2. Nombrado de slots

```html
<div class="container">
  <header>
    <!-- We want header content here -->
  </header>
  <main>
    <!-- We want main content here -->
  </main>
  <footer>
    <!-- We want footer content here -->
  </footer>
</div>
```

```html
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

```html
<base-layout>
  <template slot="header">
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template slot="footer">
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

```html
<base-layout>
  <h1 slot="header">Here might be a page title</h1>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <p slot="footer">Here's some contact info</p>
</base-layout>
```

```html
<div class="container">
  <header>
    <h1>Here might be a page title</h1>
  </header>
  <main>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </main>
  <footer>
    <p>Here's some contact info</p>
  </footer>
</div>
```

### 5.1.3. Slot por defecto

```html
<button type="submit">
  <slot>Submit</slot>
</button>
```

### 5.1.4. Scope de compilación

```html
<navigation-link url="/profile">
  Logged in as {{ user.name }}
</navigation-link>
```

## 5.2. Filtros

```html
<!-- in mustaches -->
{{ message | capitalize }}

<!-- in v-bind -->
<div v-bind:id="rawId | formatId"></div>
```

```js
filters: {
  capitalize: function (value) {
    if (!value) return ''
    value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1)
  }
}
```

```js
Vue.filter('capitalize', function (value) {
  if (!value) return ''
  value = value.toString()
  return value.charAt(0).toUpperCase() + value.slice(1)
})

new Vue({
  // ...
})
```

```html
{{ message | filterA | filterB }}
```

```html
{{ message | filterA('arg1', arg2) }}
```

## 5.3. Directivas

```js
// Register a global custom directive called `v-focus`
Vue.directive('focus', {
  // When the bound element is inserted into the DOM...
  inserted: function (el) {
    // Focus the element
    el.focus()
  }
})
```

```js
directives: {
  focus: {
    // directive definition
    inserted: function (el) {
      el.focus()
    }
  }
}
```

```html
<input v-focus>
```

### 5.3.1. Ciclo de vida de una directiva y los hooks

### 5.3.2. Parámetros de los hooks

### 5.3.3. Shorthand

```js
Vue.directive('color-swatch', function (el, binding) {
  el.style.backgroundColor = binding.value
})
```

### 5.3.4. Objeto como parámetro de la directiva

```html
<div v-demo="{ color: 'white', text: 'hello!' }"></div>
```

```js
Vue.directive('demo', function (el, binding) {
  console.log(binding.value.color) // => "white"
  console.log(binding.value.text)  // => "hello!"
})
```

## 5.4. Mixins

```js
// define a mixin object
var myMixin = {
  created: function () {
    this.hello()
  },
  methods: {
    hello: function () {
      console.log('hello from mixin!')
    }
  }
}

// define a component that uses this mixin
var Component = Vue.extend({
  mixins: [myMixin]
})

var component = new Component() // => "hello from mixin!"
```

### 5.4.1. Cómo mezcla los objetos

```js
var mixin = {
  data: function () {
    return {
      message: 'hello',
      foo: 'abc'
    }
  }
}

new Vue({
  mixins: [mixin],
  data: function () {
    return {
      message: 'goodbye',
      bar: 'def'
    }
  },
  created: function () {
    console.log(this.$data)
    // => { message: "goodbye", foo: "abc", bar: "def" }
  }
})
```

```js
var mixin = {
  created: function () {
    console.log('mixin hook called')
  }
}

new Vue({
  mixins: [mixin],
  created: function () {
    console.log('component hook called')
  }
})

// => "mixin hook called"
// => "component hook called"
```

```js
var mixin = {
  methods: {
    foo: function () {
      console.log('foo')
    },
    conflicting: function () {
      console.log('from mixin')
    }
  }
}

var vm = new Vue({
  mixins: [mixin],
  methods: {
    bar: function () {
      console.log('bar')
    },
    conflicting: function () {
      console.log('from self')
    }
  }
})

vm.foo() // => "foo"
vm.bar() // => "bar"
vm.conflicting() // => "from self"
```

### 5.4.2. Mixins globales

```js
// inject a handler for `myOption` custom option
Vue.mixin({
  created: function () {
    var myOption = this.$options.myOption
    if (myOption) {
      console.log(myOption)
    }
  }
})

new Vue({
  myOption: 'hello!'
})
// => "hello!"
```
