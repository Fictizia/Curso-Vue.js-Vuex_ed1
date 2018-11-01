# Clase 5

## Planning del día 

1. Repaso clase anterior
2. Creando un enrutador simple
3. vue-router
4. Ejercicio 'Creando las rutas de mi webapp'

## Índice

- [1. Creando un un enrutador simple](#1-creando-un-un-enrutador-simple)
- [2. `vue-router`](#2-vue-router)
    - [2.1. Introducción](#21-introducción)
    - [2.2. ¿Cómo empezar?](#22-¿cómo-empezar)
    - [2.3. Configurando rutas](#23-configurando-rutas)
        - [2.3.1. Reacciona a cambios en los parámetros](#231-reacciona-a-cambios-en-los-parámetros)
        - [2.3.2. Configuraciones avanzadas](#232-configuraciones-avanzadas)
        - [2.3.3. Prioridad en las relaciones](#233-prioridad-en-las-relaciones)
    - [2.4. Rutas anidadas](#24-rutas-anidadas)
    - [2.5. Programando navegaciones](#25-programando-navegaciones)
        - [2.5.1. Manipulando el histórico de navegación](#251-manipulando-el-histórico-de-navegación)
    - [2.6. Nombrado](#26-nombrado)
        - [2.6.1. Nombrado de rutas](#261-nombrado-de-rutas)
        - [2.6.2. Nombrado de vistas](#262-nombrado-de-vistas)
        - [2.6.3. Nombrado de vistas anidadas](#263-nombrado-de-vistas-anidadas)
    - [2.7. Redirecciones y alias](#27-redirecciones-y-alias)
        - [2.7.1. Redirect](#271-redirect)
        - [2.7.2. Alias](#272-alias)
    - [2.8. Pasando propiedades a un componente de vista](#28-pasando-propiedades-a-un-componente-de-vista)
    - [2.9. Modo histórico de HTML5](#29-modo-histórico-de-html5)
    - [2.10. Navigation Guard](#210-navigation-guard)
        - [2.10.1. Global Guards](#2101-global-guards)
        - [2.10.2. Global Resolve Guards](#2102-global-resolve-guards)
        - [2.10.3. Global After Hooks](#2103-global-after-hooks)
        - [2.10.4. Per-Route Guard](#2104-per-route-guard)
        - [2.10.5. In-Component Guards](#2105-in-component-guards)
        - [2.10.6. The Full Navigation Resolution Flow](#2106-the-full-navigation-resolution-flow)
    - [2.11. Route Meta Fields](#211-route-meta-fields)
    - [2.12. Data Fetching](#212-data-fetching)
        - [2.12.1. Fetching After Navigation](#2121-fetching-after-navigation)
        - [2.12.2. Fetching Before Navigation](#2122-fetching-before-navigation)
    - [2.13. Lazy Loading Routes](#213-lazy-loading-routes)
        - [2.13.1. Grouping Components in the Same Chunk](#2131-grouping-components-in-the-same-chunk)


# 1. Creando un un enrutador simple

Cuando una aplicación empieza a crecer, acaba necesitando la manera de poder gestionar las diferentes pantallas, páginas o vistas que tiene. 

En aplicaciones Web, particularmente en SPAs, la gestión de las diferentes vistas se realizando por medio de rutas (urls) locales que nos permitan saber en qué parte de la aplicación nos encontramos y qué componente raíz debemos renderizar.

Si la aplicación que estamos desarrollando es pequeña, podemos crear una gestion de la ruta por nuestra cuenta. Podemos hacer un pequeño enrutador que indique qué componente renderizar.

Un ejemplo podría ser este:

```js
const NotFound = { template: '<p>Page not found</p>' }
const Home = { template: '<p>home page</p>' }
const About = { template: '<p>about page</p>' }

const routes = {
  '/': Home,
  '/about': About
}

new Vue({
  el: '#app',
  data: {
    currentRoute: window.location.pathname
  },
  computed: {
    ViewComponent () {
      return routes[this.currentRoute] || NotFound
    }
  },
  render (h) { return h(this.ViewComponent) }
})
```

Vayamos por partes:

1. Lo primero que hacemos es definir los componentes de tipo vista que queremos que se rendericen en una determinada ruta dada. Por ejemplo, aquí tenemos 3 componentes tipo vista típicos en una aplicación: `NotFound`, `Home` y `About`:

```js
const NotFound = { template: '<p>Page not found</p>' }
const Home = { template: '<p>home page</p>' }
const About = { template: '<p>about page</p>' }
```

Estos componentes se encontrarán en su respectivo SFC y se importarían luego en el gestor de rutas, pero para que veamos el ejemplo más claro, se incluye todo de seguido.

---

Por otra parte, tenemos la configuración de rutas. Esto no es más que un objeto JSON que contiene la ruta que queremos escuchar y el componentes que tenemos que renderizar.

En este caso lo que indicamos es que cuando el usuario indique en la url la raiz (`/`), se renderizará el componente `Home` y que cuando el usuario indique la ruta `/about`, renderizará el componente `About`:

```js
const routes = {
  '/': Home,
  '/about': About
}
```

---

Lo siguiente es crear nuestra instancia de Vue y manejar de manera dinámica la ruta:

```js
new Vue({
  el: '#app',
  data: {
    currentRoute: window.location.pathname
  },
  computed: {
    ViewComponent () {
      return routes[this.currentRoute] || NotFound
    }
  },
  render (h) { return h(this.ViewComponent) }
})
```

De aquí, detengámonos en 3 puntos. Primero el `data`:

```js
data: {
    currentRoute: window.location.pathname
}
```

Lo que en `data` hacemos es iniciar con la url actual, la variable `currentRouter`.

Después, creamos una propiedad computada que se encargará de cambiar el componente a renderizar a partir de la ruta escrita en `currentRouter`. Si no hay una coincidencia con las rutas configuradas, se renderiza el componente `NotFound`:

```js
computed: {
    ViewComponent () {
        return routes[this.currentRoute] || NotFound
    }
},
```

por último s euna una función de renderizado que se encarga de incluir en el Virtual DOM el componente indicado:

```js
render (h) { return h(this.ViewComponent) }
```

Y ya está. COn ir añadiendo rutas en el fichero de configuración lo tendríamos.

Ahora bien, esto se nos queda algo limitado. ¿Qué ocurre si queremos generar rutas dinámicas (que varios patrones rendericen el mismo componente)? ¿Cómo gestionamos las vistas anidadas o en profundidad)? ¿Cómo puedo pasar parámetros a los componentes de vista?

Es aquí, donde esta solución se nos queda corta y necesitamos una herramienta que nos ayude a hacer más cosas de manera sencilla. Es hora de introducir `vue-router`.

# 2. `vue-router`

## 2.1. Introducción

## 2.2. ¿Cómo empezar?

Podemos usar `vue-router`de dos maneras dependiendo de cómo esté creada nuestra aplicación. Si no estamos usando `vue-cli` para crear nuestro scaffolding, lo haremos importando la librería después de Vue en nuestro HTML:

```html
<script src="/path/to/vue.js"></script>
<script src="/path/to/vue-router.js"></script>
```

Si estamos usando `vue-cli` para construir nuestra aplicación. Tendremos que instalarla via NPM:

```sh
$ npm install vue-router
```

Y después extender Vue por medio de `Vue.use` en nuestro código:

```js
// router/index.js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)
```

A partir de aquí, podemos empezar a configurar rutas:

```js
// continúa router/index.js
export default new VueRouter({
    // configuración de rutas
})
```

Tenemos que incluir las rutas en nuestra instancia de vue:

```js
// main.js
import router from './router

new Vue({
    el: '#app',
    router
})
```

Y en el HTML, indicar dónde se encontrará la parte dinámica en donde vue-router tiene que renderizar el componente:

```html
<div id="app">
  <h1>Hello App!</h1>
  <p>
    <router-link to="/foo">Go to Foo</router-link>
    <router-link to="/bar">Go to Bar</router-link>
  </p>

  <router-view></router-view>
</div>
```

Ahora solo tenemos que empezar a configurar rutas.

## 2.3. Configurando rutas

Para configurar rutas, es tan fácil como hacíamos en nuestro ejemplo. Indicamos un array con todas las rutas que queramos gestionar:

```js
// router/index.js
import Foo from '@/components/Foo'
import Bar from '@/components/Bar'

export default new VueRouter({
    routes: [
        { 
            path: '/foo', 
            component: Foo 
        },
        { 
            path: '/bar', 
            component: Bar
        }
    ]
})
```

Indicamos la ruta a gestionar (`path`) y el componente a renderizar (`component`)


### 2.3.1. Reacciona a cambios en los parámetros

```js
const User = {
  template: '<div>User</div>'
}

const router = new VueRouter({
  routes: [
    // las partes dinamicas se indican por medio de dos puntos
    { path: '/user/:id', component: User }
  ]
})
```

```js
const User = {
    template: '<div>User {{ $route.params.id }}</div>'
}
```


| patrón	                    | url relacionada     | $route.params                      |
|-------------------------------|---------------------|------------------------------------|
| /user/:username               | /user/evan          | { username: 'evan' }               |
| /user/:username/post/:post_id | /user/evan/post/123 | { username: 'evan', post_id: 123 } |


```
const User = {
  template: '...',
  watch: {
    '$route' (to, from) {
      // react to route changes...
    }
  }
}
```

### 2.3.2. Configuraciones avanzadas


### 2.3.3. Prioridad en las relaciones

## 2.4. Rutas anidadas

```
/user/foo/profile                     /user/foo/posts
+------------------+                  +-----------------+
| User             |                  | User            |
| +--------------+ |                  | +-------------+ |
| | Profile      | |  +------------>  | | Posts       | |
| |              | |                  | |             | |
| +--------------+ |                  | +-------------+ |
+------------------+                  +-----------------+
```

```html
<div id="app">
  <router-view></router-view>
</div>
```

```js
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}

const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User }
  ]
})
```

```js
const User = {
  template: `
    <div class="user">
      <h2>User {{ $route.params.id }}</h2>
      <router-view></router-view>
    </div>
  `
}
```

```js
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User,
      children: [
        {
          // UserProfile will be rendered inside User's <router-view>
          // when /user/:id/profile is matched
          path: 'profile',
          component: UserProfile
        },
        {
          // UserPosts will be rendered inside User's <router-view>
          // when /user/:id/posts is matched
          path: 'posts',
          component: UserPosts
        }
      ]
    }
  ]
})
```

```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:id', component: User,
      children: [
        // UserHome will be rendered inside User's <router-view>
        // when /user/:id is matched
        { path: '', component: UserHome },

        // ...other sub routes
      ]
    }
  ]
})
```

## 2.5. Programando navegaciones

1. **Ir a una ruta en concreto**

| Forma declarativa         | Forma programática   |
|---------------------------|----------------------|
| `<router-link :to="...">` | `router.push(...)`   |

```js
// literal string path
router.push('home')

// object
router.push({ path: 'home' })

// named route
router.push({ name: 'user', params: { userId: 123 }})

// with query, resulting in /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
```

```js
const userId = 123
router.push({ name: 'user', params: { userId }}) // -> /user/123
router.push({ path: `/user/${userId}` }) // -> /user/123
// This will NOT work
router.push({ path: '/user', params: { userId }}) // -> /user
```

2. **Reemplazar ruta**

| Forma declarativa                 | Forma programática      |
|-----------------------------------|-------------------------|
| `<router-link :to="..." replace>` | `router.replace(...)`   |

3. Navegar en el histórico de ruta

```js
// go forward by one record, the same as history.forward()
router.go(1)

// go back by one record, the same as history.back()
router.go(-1)

// go forward by 3 records
router.go(3)

// fails silently if there aren't that many records.
router.go(-100)
router.go(100)
```

### 2.5.1. Manipulando el histórico de navegación

## 2.6. Nombrado 

### 2.6.1. Nombrado de rutas

```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
```

```html
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
```

```js
router.push({ name: 'user', params: { userId: 123 }})
```

### 2.6.2. Nombrado de vistas

```html
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>
```

```js
const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Foo,
        a: Bar,
        b: Baz
      }
    }
  ]
})
```
### 2.6.3. Nombrado de vistas anidadas


```
/settings/emails                                       /settings/profile
+-----------------------------------+                  +------------------------------+
| UserSettings                      |                  | UserSettings                 |
| +-----+-------------------------+ |                  | +-----+--------------------+ |
| | Nav | UserEmailsSubscriptions | |  +------------>  | | Nav | UserProfile        | |
| |     +-------------------------+ |                  | |     +--------------------+ |
| |     |                         | |                  | |     | UserProfilePreview | |
| +-----+-------------------------+ |                  | +-----+--------------------+ |
+-----------------------------------+                  +------------------------------+
```

```html
<!-- UserSettings.vue -->
<div>
  <h1>User Settings</h1>
  <NavBar/>
  <router-view/>
  <router-view name="helper"/>
</div>
```

```js
{
  path: '/settings',
  // You could also have named views at the top
  component: UserSettings,
  children: [{
    path: 'emails',
    component: UserEmailsSubscriptions
  }, {
    path: 'profile',
    components: {
      default: UserProfile,
      helper: UserProfilePreview
    }
  }]
}
```

## 2.7. Redirecciones y alias

### 2.7.1. Redirect

```js
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' }
  ]
})
```

```js
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: { name: 'foo' }}
  ]
})
```

```js
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: to => {
      // the function receives the target route as the argument
      // return redirect path/location here.
    }}
  ]
})
```

### 2.7.2. Alias

```js
const router = new VueRouter({
  routes: [
    { path: '/a', component: A, alias: '/b' }
  ]
})
```

## 2.8. Pasando propiedades a un componente de vista

```js
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User }
  ]
})
```

```js
const User = {
  props: ['id'],
  template: '<div>User {{ id }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User, props: true },

    // for routes with named views, you have to define the `props` option for each named view:
    {
      path: '/user/:id',
      components: { default: User, sidebar: Sidebar },
      props: { default: true, sidebar: false }
    }
  ]
})
```

Esta configuración admite tres tipos distintos:

1. Boolean

2. Object

```js
const router = new VueRouter({
  routes: [
    { path: '/promotion/from-newsletter', component: Promotion, props: { newsletterPopup: false } }
  ]
})
```

3. Function

```js
const router = new VueRouter({
  routes: [
    { path: '/search', component: SearchUser, props: (route) => ({ query: route.query.q }) }
  ]
})
```

## 2.9. Modo histórico de HTML5

## 2.10. Navigation Guard

### 2.10.1. Global Guards

### 2.10.2. Global Resolve Guards

### 2.10.3. Global After Hooks

### 2.10.4. Per-Route Guard

### 2.10.5. In-Component Guards

### 2.10.6. The Full Navigation Resolution Flow

## 2.11. Route Meta Fields

## 2.12. Data Fetching

### 2.12.1. Fetching After Navigation

### 2.12.2. Fetching Before Navigation

## 2.13. Lazy Loading Routes

### 2.13.1. Grouping Components in the Same Chunk






