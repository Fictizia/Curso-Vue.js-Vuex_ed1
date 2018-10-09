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

### 1.1.1. Texto

```html
<input v-model="message" placeholder="Edítame">
<p>El mensaje es: {{ message }}</p>
```

### 1.1.2. Texto multilínea

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

Si queremos guardar el valor de varias opciones:

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

### 1.1.4. Radio

Si tenemos que elegir entre una de las opciones, usaremos un radiobutton:

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

```html
<!-- `picked` is a string "a" when checked -->
<input type="radio" v-model="picked" value="a">

<!-- `toggle` is either true or false -->
<input type="checkbox" v-model="toggle">

<!-- `selected` is a string "abc" when the first option is selected -->
<select v-model="selected">
  <option value="abc">ABC</option>
</select>
```

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
// when checked:
vm.toggle === 'yes'
// when unchecked:
vm.toggle === 'no'
```

### 1.2.2. Radio

```html
<input type="radio" v-model="pick" v-bind:value="a">
```
```js
// when checked:
vm.pick === vm.a
```

### 1.2.3. Select

```html
<select v-model="selected">
  <!-- inline object literal -->
  <option v-bind:value="{ number: 123 }">123</option>
</select>
```
```js
// when selected:
typeof vm.selected // => 'object'
vm.selected.number // => 123
```


## 1.3. Modificadores

* `.lazy`

```html
<!-- synced after "change" instead of "input" -->
<input v-model.lazy="msg" >
```

* `.number`

```html
<input v-model.number="age" type="number">
```

* `.trim`

```html
<input v-model.trim="msg">
```

# 2. Comunicación entre componentes

## 2.1. Props

### 2.1.1. Nomenclatura de las props

```js
Vue.component('blog-post', {
  // camelCase in JavaScript
  props: ['postTitle'],
  template: '<h3>{{ postTitle }}</h3>'
})
```
```js
<!-- kebab-case en HTML -->
<blog-post post-title="hello!"></blog-post>
```

### 2.1.2. Tipado de las props

```js
props: ['title', 'likes', 'isPublished', 'commentIds', 'author']
```

```js
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object
}
```

### 2.1.3. Pasando props estáticas o dinámicas

```html
<blog-post title="My journey with Vue"></blog-post>
```

```html
<!-- Dynamically assign the value of a variable -->
<blog-post v-bind:title="post.title"></blog-post>

<!-- Dynamically assign the value of a complex expression -->
<blog-post v-bind:title="post.title + ' by ' + post.author.name"></blog-post>
```

#### 2.1.3.1. Pasando un número

```html
<!-- Even though `42` is static, we need v-bind to tell Vue that -->
<!-- this is a JavaScript expression rather than a string.       -->
<blog-post v-bind:likes="42"></blog-post>

<!-- Dynamically assign to the value of a variable. -->
<blog-post v-bind:likes="post.likes"></blog-post>
```

#### 2.1.3.2. Pasando un booleano

```html
<!-- Including the prop with no value will imply `true`. -->
<blog-post is-published></blog-post>

<!-- Even though `false` is static, we need v-bind to tell Vue that -->
<!-- this is a JavaScript expression rather than a string.          -->
<blog-post v-bind:is-published="false"></blog-post>

<!-- Dynamically assign to the value of a variable. -->
<blog-post v-bind:is-published="post.isPublished"></blog-post>
```

#### 2.1.3.3. Pasando un array

```html
<!-- Even though the array is static, we need v-bind to tell Vue that -->
<!-- this is a JavaScript expression rather than a string.            -->
<blog-post v-bind:comment-ids="[234, 266, 273]"></blog-post>

<!-- Dynamically assign to the value of a variable. -->
<blog-post v-bind:comment-ids="post.commentIds"></blog-post>
```

#### 2.1.3.4. Pasando un objeto

```html
<!-- Even though the object is static, we need v-bind to tell Vue that -->
<!-- this is a JavaScript expression rather than a string.             -->
<blog-post v-bind:author="{ name: 'Veronica', company: 'Veridian Dynamics' }"></blog-post>

<!-- Dynamically assign to the value of a variable. -->
<blog-post v-bind:author="post.author"></blog-post>
```

#### 2.1.3.5. Pasando las propiedades de un objeto

```js
post: {
  id: 1,
  title: 'My Journey with Vue'
}
```

```html
<blog-post v-bind="post"></blog-post>
```

```html
<blog-post
  v-bind:id="post.id"
  v-bind:title="post.title"
></blog-post>
```

### 2.1.4. Flujos en una única dirección

```js
props: ['initialCounter'],
data: function () {
  return {
    counter: this.initialCounter
  }
}
```

```js
props: ['size'],
computed: {
  normalizedSize: function () {
    return this.size.trim().toLowerCase()
  }
}
```

### 2.1.5. Validación de props

```js
Vue.component('my-component', {
  props: {
    // Basic type check (`null` matches any type)
    propA: Number,
    // Multiple possible types
    propB: [String, Number],
    // Required string
    propC: {
      type: String,
      required: true
    },
    // Number with a default value
    propD: {
      type: Number,
      default: 100
    },
    // Object with a default value
    propE: {
      type: Object,
      // Object or array defaults must be returned from
      // a factory function
      default: function () {
        return { message: 'hello' }
      }
    },
    // Custom validator function
    propF: {
      validator: function (value) {
        // The value must match one of these strings
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})
```
**Comprobación de tipos**

* String
* Number
* Boolean
* Array
* Object
* Date
* Function
* Symbol

```js
function Person (firstName, lastName) {
  this.firstName = firstName
  this.lastName = lastName
}
```

Y lo podemos usar:

```js
Vue.component('blog-post', {
  props: {
    author: Person
  }
})
```


### 2.1.6. Atributos que no son props

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

### 5.1.2. Nombrado de slots

### 5.1.3. Slot por defecto

### 5.1.4. Scope de compilación

## 5.2. Filtros

## 5.3. Directivas

### 5.3.1. Ciclo de vida de una directiva y los hooks

### 5.3.2. Parámetros de los hooks

### 5.3.3. Shorthand

### 5.3.4. Objeto como parámetro de la directiva

## 5.4. Mixins

### 5.4.1. Cómo mezcla los objetos

### 5.4.2. Mixins globales


