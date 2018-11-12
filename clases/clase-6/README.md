# Clase 6

## Planning del día 

1. Repaso clase anterior
2. Creando un gestor de estados simple
3. vuex
4. Ejercicio 

## Índice

- [1. Introducción](#1-introducción)
    - [1.1. ¿Qué es el estado de una aplicación?](#11-¿qué-es-el-estado-de-una-aplicación)
    - [1.2. Formas de gestionar el estado](#12-formas-de-gestionar-el-estado)
    - [1.3. Problemática a solucionar](#13-problemática-a-solucionar)
- [2. Creando un gestor de estados simple](#2-creando-un-gestor-de-estados-simple)
- [3. `vuex`](#3-vuex)
    - [3.1. Introducción](#31-introducción)
    - [3.2. ¿Cómo empezar?](#32-¿cómo-empezar)
    - [3.3. `State`](#33-state)
        - [3.3.1. Árbol simple de estados](#331-árbol-simple-de-estados)
        - [3.3.2. Obteniendo el estado en un componente](#332-obteniendo-el-estado-en-un-componente)
        - [3.3.3. `mapState`](#333-mapstate)
        - [3.3.4. Spread Operator](#334-spread-operator)
        - [3.3.5. El componente aun puede tener estado local](#335-el-componente-aun-puede-tener-estado-local)
    - [3.4. `Getters`](#34-getters)
        - [3.4.1. Acceso como propiedades](#341-acceso-como-propiedades)
        - [3.5.2. Acceso como métodos](#352-acceso-como-métodos)
        - [3.5.3. `mapGetters`](#353-mapgetters)
    - [3.6. `Mutations`](#36-mutations)
        - [3.6.1. Commit con payload](#361-commit-con-payload)
        - [3.6.2. Mutación como objeto](#362-mutación-como-objeto)
        - [3.6.3. Las mutaciones siguen las reglas de reactividad](#363-las-mutaciones-siguen-las-reglas-de-reactividad)
        - [3.6.4. Usando constantes para los tipos de mutaciones](#364-usando-constantes-para-los-tipos-de-mutaciones)
        - [3.6.5. Las mutaciones deben ser sincronas](#365-las-mutaciones-deben-ser-sincronas)
        - [3.6.6. `mapMutations`](#366-mapmutations)
    - [3.7. `Actions`](#37-actions)
        - [3.7.1. Ejecutando acciones](#371-ejecutando-acciones)
        - [3.7.2. `mapActions`](#372-mapactions)
        - [3.7.3. Componiendo acciones](#373-componiendo-acciones)
    - [3.8. Módulos](#38-módulos)
        - [3.8.1. Estado en un módulo](#381-estado-en-un-módulo)
        - [3.8.2. Espacio de nombres](#382-espacio-de-nombres)
    - [3.9. Estructura de aplicación](#39-estructura-de-aplicación)
    - [3.10. Modo estricto](#310-modo-estricto)
- [3.11. Manejar formularios](#311-manejar-formularios)
- [4. Ejercicio](#4-ejercicio)


# 1. Introducción

## 1.1. ¿Qué es el estado de una aplicación?

## 1.2. Formas de gestionar el estado

## 1.3. Problemática a solucionar

# 2. Creando un gestor de estados simple

# 3. `vuex`

## 3.1. Introducción

```js
new Vue({
  // state
  data () {
    return {
      count: 0
    }
  },

  // view
  template: `
    <div>{{ count }}</div>
  `,

  // actions
  methods: {
    increment () {
      this.count++
    }
  }
})
```

![flow](imgs/flow.png)

![vuex](imgs/vuex.png)

## 3.2. ¿Cómo empezar?

```js
const store = new Vuex.Store({
  state: {
    count: 0
  },

  mutations: {
    increment (state) {
      state.count++
    }
  }
})
```

```js
store.commit('increment')

console.log(store.state.count) // -> 1
```

## 3.3. `State`

### 3.3.1. Árbol simple de estados

Vuex usa un arbol simple de estados. Esto significa que en único objeto, gestionamos todos los datos a nivel de plaicación. Nos ayuda a tener un úncio origen de la verdad dentro de nuestra aplicación. 

Esto significa que generalmente solo tendremos un unico store por aplicación. Tener un sistema así, nos va a permitir realizar fotografías de un momento determinado de la aplicación lo que nos permitirá depurar mejor nuestras aplicaciones.

### 3.3.2. Obteniendo el estado en un componente

Como indicábamos anteriormente, para crear un contenedor de datos en Vuex o Store, haremos lo siguiente:

```js
const store = new Vuex.Store({
  state: {
    count: 0
  }
})
```
De esta manera, ya tenemos un objeto controlado, desde donde podemos acceder desde los componentes.

Para hacer un acceso desde un componente, es tan fácil como crear una computada con el estado que necesitemos. Hay que tener en cuenta que `state` es reactivo por lo que se provocarán cambios en los componentes si hay cambios en el estado.

```js
// creamos un componente Counter
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return store.state.count
    }
  }
}
```

El sistema anterior nos puede traer problemas porque tenemos que importar el `store` en cada uno de los componentes en el que lo necesitemos acloplándolo demasiado.

Para solucionar esto, al igual que hacíamos con `vue-router`, podemos inyectar el store en la instancia padre y que se propague en el árbol:

```js
const app = new Vue({
  el: '#app',
  // proporcionamos el store usando la opción "store".
  // Esto inyectará la instancia del store a todos los componentes hijos.
  store,
  components: { Counter },
  template: `
    <div class="app">
      <counter></counter>
    </div>
  `
})
```

Gracias a esto, ya podemos usar la instancia del store inyectada en el árbol de componente. De esta manera podremos desacoplar y mockear mejor en tiempo de test:

```js
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return this.$store.state.count
    }
  }
}
```

### 3.3.3. `mapState`

Tenemos todavía una mejor forma de inyectar el estado de nuestro estor gracías a una utilidad de vuex llamada `mapState` que lo que nos permite es mapear estados como computadas. En muchos casos no necesitaremos crear computadas que repiten código. Esta librería ya lo hace por nosotros:

```js
// En la propia librería se expone esta utilidad como Vuex.mapState
import { mapState } from 'vuex'

export default {
  // ...
  computed: mapState({
    // Nos permite mapear estados de mucha maneras:
    // con una arrow functions podemos acceder una manera muy simple
    count: state => state.count,

    // podemos pasar un valor de tipo string 'count' que sería igual que indicar `state => state.count`
    countAlias: 'count',

    // también podemos mezclar estado local con una función de esta manera
    countPlusLocalState (state) {
      return state.count + this.localCount
    }
  })
}
```

### 3.3.4. Spread Operator

¿Qué ocurre si dentro de nuestro componente tenemos computed y queremos mapear otros del estado? Podemos usar el Spread Operator en esta ocasión:

### 3.3.5. El componente aun puede tener estado local

Que una aplicación haga uso de `vuex` no significa que ya no tengamos que tener estado local en un componente.

`vuex` nos ayuda a guardar estados a nivel de aplicación. Estados compartidos entre diferentes partes y módulos. Eso no significa que los componentes no tengan que encapsular estados internos para que su funcionamiento sea el correcto.

Ten en cuenta que un uso correcto con `vuex` siempre va a suponer un buen trato de los datos locales.

## 3.4. `Getters`

Con `state` podemos acceder directamente a los datos de un `store`. Ahora bien, dentro de `vuex` podemos crear vistas para obtener datos de una manera más simple y reutilizable. Esto es lo que se llama getters. Son propiedades que realizan queris o transformaciones sobre los `states`. 

Hay que verlo como las propiedades computadas de `vuex`. Por tanto, son reactivas tambien y su acceso es por medio de propiedades aunque se defina como funciones factoría.

¿En qué nos son útiles? Imaginemos que tenemos la siguiente computed en un componente:

```js
computed: {
  doneTodosCount () {
    return this.$store.state.todos.filter(todo => todo.done).length
  }
}
```

Es bastante fastidio tener que estar calculando por toda la aplicación el numero de todos que ya están hechos. Estaría muy bien contar con un algo que nos calculase esto en nuestro `store`. De esto se encargan los `getters`.

Tendríamos algo así:

```js
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }
  }
})
```

Ahora lo único que tenemos que hacer es acceder a esta propiedad en los componentes.

### 3.4.1. Acceso como propiedades

Como decíamos, aunque se definan como funciones. A los datos de un `getter`se accede como si fuera una propiedad:

```js
store.getters.doneTodos // -> [{ id: 1, text: '...', done: true }]
```

Los `getters` se pueden componer a partir de otros.

Por tanto, en este caso tendríamos dos: uno que me da los todos hechos y otro que me hace el recuento.

```js
getters: {
  // ...
  doneTodosCount: (state, getters) => {
    return getters.doneTodos.length
  }
}
```

El acceso es igual que el anterior:

```js
store.getters.doneTodosCount // -> 1
```

### 3.5.2. Acceso como métodos

Como hemos dicho, los `getters` nos permiten hacer queries sobre el estado. Esto quiere decir que permiten parámetros para que podamos realizar cosas con ellos. EN este caso buscamos un todo por un id determinado:

```js
getters: {
  // ...
  getTodoById: (state) => (id) => {
    return state.todos.find(todo => todo.id === id)
  }
}
```

Ahora sí, el acceso a estos datos es por medio de la ejecución del método:

```js
store.getters.getTodoById(2) // -> { id: 2, text: '...', done: false }
```

### 3.5.3. `mapGetters`

Los `getters` también cuenta con una utilidad para mapearlos como computadas en los componentes:

```js
import { mapGetters } from 'vuex'

export default {
  // ...
  computed: {
    // Con indicar su nombre podemos mapear los que necesitemos
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }
}
```
```js
...mapGetters({
  // Podemos indicar el nombre de una computed nueva para que no esté tan acoplado
  doneCount: 'doneTodosCount'
})
```

## 3.6. `Mutations`

```js
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment (state) {
      // mutate state
      state.count++
    }
  }
})
```

```js
store.commit('increment')
```

### 3.6.1. Commit con payload

```js
// ...
mutations: {
  increment (state, n) {
    state.count += n
  }
}
```
```js
store.commit('increment', 10)
```

```js
// ...
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
```
```js
store.commit('increment', {
  amount: 10
})
```

### 3.6.2. Mutación como objeto

```js
store.commit({
  type: 'increment',
  amount: 10
})
```

```js
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
```

### 3.6.3. Las mutaciones siguen las reglas de reactividad



### 3.6.4. Usando constantes para los tipos de mutaciones

```js
// mutation-types.js
export const SOME_MUTATION = 'SOME_MUTATION'
```

```js
// store.js
import Vuex from 'vuex'
import { SOME_MUTATION } from './mutation-types'

const store = new Vuex.Store({
  state: { ... },
  mutations: {
    // we can use the ES2015 computed property name feature
    // to use a constant as the function name
    [SOME_MUTATION] (state) {
      // mutate state
    }
  }
})
```

### 3.6.5. Las mutaciones deben ser sincronas

```js
mutations: {
  someMutation (state) {
    api.callAsyncMethod(() => {
      state.count++
    })
  }
}
```

### 3.6.6. `mapMutations` 

```js
import { mapMutations } from 'vuex'

export default {
  // ...
  methods: {
    ...mapMutations([
      'increment', // map `this.increment()` to `this.$store.commit('increment')`

      // `mapMutations` also supports payloads:
      'incrementBy' // map `this.incrementBy(amount)` to `this.$store.commit('incrementBy', amount)`
    ]),
    ...mapMutations({
      add: 'increment' // map `this.add()` to `this.$store.commit('increment')`
    })
  }
}
```

## 3.7. `Actions`

```js
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment (context) {
      context.commit('increment')
    }
  }
})
```

```js
actions: {
  increment ({ commit }) {
    commit('increment')
  }
}
```

### 3.7.1. Ejecutando acciones

```js
store.dispatch('increment')
```

```js
actions: {
  incrementAsync ({ commit }) {
    setTimeout(() => {
      commit('increment')
    }, 1000)
  }
}
```

```js
// dispatch with a payload
store.dispatch('incrementAsync', {
  amount: 10
})

// dispatch with an object
store.dispatch({
  type: 'incrementAsync',
  amount: 10
})
```

```js
actions: {
  checkout ({ commit, state }, products) {
    // save the items currently in the cart
    const savedCartItems = [...state.cart.added]
    // send out checkout request, and optimistically
    // clear the cart
    commit(types.CHECKOUT_REQUEST)
    // the shop API accepts a success callback and a failure callback
    shop.buyProducts(
      products,
      // handle success
      () => commit(types.CHECKOUT_SUCCESS),
      // handle failure
      () => commit(types.CHECKOUT_FAILURE, savedCartItems)
    )
  }
}
```

### 3.7.2. `mapActions`

```js
import { mapActions } from 'vuex'

export default {
  // ...
  methods: {
    ...mapActions([
      'increment', // map `this.increment()` to `this.$store.dispatch('increment')`

      // `mapActions` also supports payloads:
      'incrementBy' // map `this.incrementBy(amount)` to `this.$store.dispatch('incrementBy', amount)`
    ]),
    ...mapActions({
      add: 'increment' // map `this.add()` to `this.$store.dispatch('increment')`
    })
  }
}
```

### 3.7.3. Componiendo acciones

```js
actions: {
  actionA ({ commit }) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        commit('someMutation')
        resolve()
      }, 1000)
    })
  }
}
```

```js
store.dispatch('actionA').then(() => {
  // ...
})
```

```js
actions: {
  // ...
  actionB ({ dispatch, commit }) {
    return dispatch('actionA').then(() => {
      commit('someOtherMutation')
    })
  }
}
```

```js
// assuming `getData()` and `getOtherData()` return Promises

actions: {
  async actionA ({ commit }) {
    commit('gotData', await getData())
  },
  async actionB ({ dispatch, commit }) {
    await dispatch('actionA') // wait for `actionA` to finish
    commit('gotOtherData', await getOtherData())
  }
}
```

## 3.8. Módulos

```js
const moduleA = {
  state: { ... },
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: { ... },
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> `moduleA`'s state
store.state.b // -> `moduleB`'s state
```

### 3.8.1. Estado en un módulo

```js
const moduleA = {
  state: { count: 0 },
  mutations: {
    increment (state) {
      // `state` is the local module state
      state.count++
    }
  },

  getters: {
    doubleCount (state) {
      return state.count * 2
    }
  }
}
```

```js
const moduleA = {
  // ...
  actions: {
    incrementIfOddOnRootSum ({ state, commit, rootState }) {
      if ((state.count + rootState.count) % 2 === 1) {
        commit('increment')
      }
    }
  }
}
```

```js
const moduleA = {
  // ...
  getters: {
    sumWithRootCount (state, getters, rootState) {
      return state.count + rootState.count
    }
  }
}
```

### 3.8.2. Espacio de nombres

```js
const store = new Vuex.Store({
  modules: {
    account: {
      namespaced: true,

      // module assets
      state: { ... }, // module state is already nested and not affected by namespace option
      getters: {
        isAdmin () { ... } // -> getters['account/isAdmin']
      },
      actions: {
        login () { ... } // -> dispatch('account/login')
      },
      mutations: {
        login () { ... } // -> commit('account/login')
      },

      // nested modules
      modules: {
        // inherits the namespace from parent module
        myPage: {
          state: { ... },
          getters: {
            profile () { ... } // -> getters['account/profile']
          }
        },

        // further nest the namespace
        posts: {
          namespaced: true,

          state: { ... },
          getters: {
            popular () { ... } // -> getters['account/posts/popular']
          }
        }
      }
    }
  }
})
```

```js
modules: {
  foo: {
    namespaced: true,

    getters: {
      // `getters` is localized to this module's getters
      // you can use rootGetters via 4th argument of getters
      someGetter (state, getters, rootState, rootGetters) {
        getters.someOtherGetter // -> 'foo/someOtherGetter'
        rootGetters.someOtherGetter // -> 'someOtherGetter'
      },
      someOtherGetter: state => { ... }
    },

    actions: {
      // dispatch and commit are also localized for this module
      // they will accept `root` option for the root dispatch/commit
      someAction ({ dispatch, commit, getters, rootGetters }) {
        getters.someGetter // -> 'foo/someGetter'
        rootGetters.someGetter // -> 'someGetter'

        dispatch('someOtherAction') // -> 'foo/someOtherAction'
        dispatch('someOtherAction', null, { root: true }) // -> 'someOtherAction'

        commit('someMutation') // -> 'foo/someMutation'
        commit('someMutation', null, { root: true }) // -> 'someMutation'
      },
      someOtherAction (ctx, payload) { ... }
    }
  }
}
```

```js
{
  actions: {
    someOtherAction ({dispatch}) {
      dispatch('someAction')
    }
  },
  modules: {
    foo: {
      namespaced: true,

      actions: {
        someAction: {
          root: true,
          handler (namespacedContext, payload) { ... } // -> 'someAction'
        }
      }
    }
  }
}
```

```js
computed: {
  ...mapState({
    a: state => state.some.nested.module.a,
    b: state => state.some.nested.module.b
  })
},
methods: {
  ...mapActions([
    'some/nested/module/foo', // -> this['some/nested/module/foo']()
    'some/nested/module/bar' // -> this['some/nested/module/bar']()
  ])
}
```

```js
computed: {
  ...mapState('some/nested/module', {
    a: state => state.a,
    b: state => state.b
  })
},
methods: {
  ...mapActions('some/nested/module', [
    'foo', // -> this.foo()
    'bar' // -> this.bar()
  ])
}
```

```js
import { createNamespacedHelpers } from 'vuex'

const { mapState, mapActions } = createNamespacedHelpers('some/nested/module')

export default {
  computed: {
    // look up in `some/nested/module`
    ...mapState({
      a: state => state.a,
      b: state => state.b
    })
  },
  methods: {
    // look up in `some/nested/module`
    ...mapActions([
      'foo',
      'bar'
    ])
  }
}
```

## 3.9. Estructura de aplicación

```markdown
├── index.html
├── main.js
├── api
│   └── ... # abstractions for making API requests
├── components
│   ├── App.vue
│   └── ...
└── store
    ├── index.js          # where we assemble modules and export the store
    ├── actions.js        # root actions
    ├── mutations.js      # root mutations
    └── modules
        ├── cart.js       # cart module
        └── products.js   # products module
```

## 3.10. Modo estricto

```js
const store = new Vuex.Store({
  // ...
  strict: true
})
```

```js
const store = new Vuex.Store({
  // ...
  strict: process.env.NODE_ENV !== 'production'
})
```
# 3.11. Manejar formularios

```html
<input v-model="obj.message">
```

```html
<input :value="message" @input="updateMessage">
```
```js
// ...
computed: {
  ...mapState({
    message: state => state.obj.message
  })
},
methods: {
  updateMessage (e) {
    this.$store.commit('updateMessage', e.target.value)
  }
}
```

```js
<input v-model="message">
```
```js
// ...
computed: {
  message: {
    get () {
      return this.$store.state.obj.message
    },
    set (value) {
      this.$store.commit('updateMessage', value)
    }
  }
}
```

# 4. Ejercicio 