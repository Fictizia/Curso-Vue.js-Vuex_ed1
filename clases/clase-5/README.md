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

En aplicaciones Web, particularmente en SPAs, la gestión de las diferentes vistas se viene realizando por medio de rutas (urls) locales que nos permitan saber en qué parte de la aplicación nos encontramos y qué componente raíz debemos renderizar.

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

Lo primero que hacemos es definir los componentes de tipo vista que queremos que se rendericen en una determinada ruta dada. Por ejemplo, aquí tenemos 3 componentes tipo vista típicos en una aplicación: `NotFound`, `Home` y `About`:

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

`vue-roter` es otra de las librerías más utilizadas en el ecosistema vue. Es una librería que nos permite:

* Mapear vistas o rutas anidadas
* Modular la configuración de rutas basada en componentes
* Crear rutas dinámicas con parametros, querys o wildcards
* Mayor control de la navegación
* Nos incluye clases a los links que han sido activados
* Modo HTML5 history o modo hash con auto-fallback en IE9
* Comportamiento del scroll personalizado

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

Podemos crear rutas dinámicas. Esto quiere decir que dentro de nuestra ruta, podemos hacer que varias rutas diferentes rendericen el mismo componente. Esto nos puede ser muy útil para crear rutas que muestran el detalle de productos o el perfil publico de un usuario. 

Por ejemplo, tenemos el siguiente ejemplo:

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

Como vemos la ruta `/user/1` y la ruta `/user/2` renderizarán el mismo componente. Para crear partes dinámicas, usamos la expresión regular: dos puntos (:) + nombre del parámetro. En este caso anterior la ruta se queda como `/user/:id` donde `user es una ruta estática y `:id` es un parámetro dinamico con nombre `id`.

Gracias a vue-router, esos parámetros son accesibles en los componentes por medio de `$route`. Podemos acceder a todos los parámetros dinámicos accediendo a `$route.params`:

```js
const User = {
    template: '<div>User {{ $route.params.id }}</div>'
}
```

Esto son otros ejemplos de rutas dinámica y de cómo `vue-router` trabajaría con ellas:


| patrón	                    | url relacionada     | $route.params                      |
|-------------------------------|---------------------|------------------------------------|
| /user/:username               | /user/evan          | { username: 'evan' }               |
| /user/:username/post/:post_id | /user/evan/post/123 | { username: 'evan', post_id: 123 } |


Debido a cómo funciona `vue-router` y `vue`, cuando el usuario solo cambia un parámetro de la url, no se realiza el `update` para el renderizado de la vista. Es por eso que tenemos que incluir reactividad para que nuestro componente cambie cuando el valor de un parámetro ha cambiado.

Tenemos dos maneras:

O pormedio de incluir un `watch` a `$route`:

```js
const User = {
  template: '...',
  watch: {
    '$route' (to, from) {
      // reactiona a cambios en la ruta
    }
  }
}
```

O por medio del Navigation Guard llamado `beforeRouteUpdate` que veremos en otra sección:

```js
const User = {
  template: '...',
  beforeRouteUpdate (to, from, next) {
    // reacciona a cambios en la ruta
    // no olvides llamar a next()
  }
}
```

### 2.3.2. Configuraciones avanzadas

Los patrones que podemos usar para el mapeo de rutas es bastante avanzado. Hemos visto los ejemplos que casi siempre usaremos. 

Pero si necesitas un control más exacto de expresiones regulares, recuerda que `vue-router` usa la librería `path-to-regexp`. Échale un vistazo de vez en cuando para ver que patrones avanzados puedes crear.


### 2.3.3. Prioridad en las relaciones

Como hemos visto, las rutas en `vue-router` se configuran en un array. Ten cuidado con el orden en que pongas las rutas porque en cuanto `vue-router` encuentre una ruta que coincida, la dará por valida. Para crear prioridades en tu configuración, ordéna correctamente tu array.

## 2.4. Rutas anidadas

Una de las ventajas de usar `vue-router` es que podemos crear rutas con niveles de profundidad. Esto nos va a permitir crear layouts que generen una sensación de modularidad y reutilización de código, además de una homogeinidad de nuestras interfaces.

Vale, imaginemos que nos encontramos en la sección del usuario donde podemos ver el perfil y los posts de dicho usuario. Me gustaría que al encontrarnos en una sección muy marcada como el usuario, aparecieran ciertos textos o navegaciones en la parte superior y que solo fuese cambiando el contenido interno en relación de la subruta en la que me encuentre. 

Quiero algo como esto:

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

Como vemos, la ruta estática `user` pintará un comoponente layout llamado `User` y que según si estoy en la subruta `profile` o `post` se renderizrá el componente `Profile` o `Post`. 

Pues bien, esto en  `vue-router` es tan fácil como hacer esto en nuestra configuración.

Dado este ejemplo, donde teníamos una aplicación que muestra datos del usuario:

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

Vamos a modificar el componente `User` para que nos deje una parte dinámica donde pintar otras vistas:

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

El `router-view` nos ayuda en esto. Le estamos diciendo a `vue-router` que tiene una zona donde renderizar otros componentes de manera dinámica. Esto no deja de ser un `slot` que vimos en lecciones anteriores.

Ahora en nuestra configuración, lo que hacemos es definir una serie de rutas hijas. Es otro subnivel donde indicamos otro arbol de rutas:

```js
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User,
      children: [
        {
          // UserProfile será renderizado dentro del <router-view> de User
          // cuando se relacione la ruta con /user/:id/profile
          path: 'profile',
          component: UserProfile
        },
        {
          // UserPosts será renderizado dentro del <router-view> de User
          // cuando se relacione la ruta con /user/:id/posts
          path: 'posts',
          component: UserPosts
        }
      ]
    }
  ]
})
```

Bastante sencillo de hacer vistas muy reutilizables.

Ten cuidado, eso sí, porque con la configuración anterior, si alguien escribiese `/user/1` simplemente, `vue-router` no renderizaría nada dentro de `User`. Que ojo, este puede ser el comportamiento por defecto que queramos.

En caso de querer renderizar algo por defecto. Dentríamos que indicar una ruta hija cuyo `path` sea vacío:

```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:id', component: User,
      children: [
        // UserHome será renderizado dentro del <router-view> de User
        // cuando se relacione la ruta con /user/:id
        { path: '', component: UserHome },

        // ...otras sub rutas
      ]
    }
  ]
})
```

## 2.5. Programando navegaciones

Una vez que tenemos rutas configurables. Necesitamos un sistema fácil con el que el usuario pueda navegar por nuestra aplicación. `vue-router` nos da dos formas de realizar navegaciones.

1. Un método declarativo por medio del componente de vue `router-link` que no es más que una abstración del tag `a` de html con un par de funcionalidades extra
2. Un método programático en JS donde por medio de la instancia de `router`que se inyecta a los componentes, podemos realizar diferentes acciones.

Podemos hacer 3 acciones con estas dos formas:


1. **Ir a una ruta en concreto**

Esto nos permite indicar la ruta exacta a la que nos tenemos que mover:

| Forma declarativa         | Forma programática   |
|---------------------------|----------------------|
| `<router-link :to="...">` | `router.push(...)`   |

`push` es un método de `router` muy versátil que nos permite indicar todo lo necesario:

```js
// literal string path
router.push('home')

// objeto
router.push({ path: 'home' })

// nombre de la ruta + parámetro
router.push({ name: 'user', params: { userId: 123 }})

// indicando queryString quedaría así: /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
```

```js
const userId = 123
router.push({ name: 'user', params: { userId }}) // -> /user/123
router.push({ path: `/user/${userId}` }) // -> /user/123
// cuidado porque la mezcla de los anteriores no funcionaría
router.push({ path: '/user', params: { userId }}) // -> /user
```

2. **Reemplazar ruta**

El caso anterior, guarda la ruta anterior visitada en el histórico de navegación. Puede darse el caso en el que no queramos guardar esa ruta, si no que queremos reemplazarla. Para eso usarémos `replace`.

Uso en sus dos vertientes:


| Forma declarativa                 | Forma programática      |
|-----------------------------------|-------------------------|
| `<router-link :to="..." replace>` | `router.replace(...)`   |

3. Navegar en el histórico de ruta

Podemos tambien movernos por el histórico por medio del método `go`. En este caso no hay forma declarativa ya que esto tiene más que ver con un sistema programñatico.

Veamos diferentes usas:

```js
// Voy hacia delante en el histórico, lo mismo que history.forward()
router.go(1)

// Vuelvo a un registro anterior, lo mismo que history.back()
router.go(-1)

// Adelanto 3 registros en el histórico
router.go(3)

// No hace nada si no existe el salto al que intentamos ir
router.go(-100)
router.go(100)
```

### 2.5.1. Manipulando el histórico de navegación

Si has trabajado con History API antes, habrás visto que `vue-router` lo único que está haciendo es crear un envoltorio sobre esa api para que funcione bien con `vue`. Por tanto `router.push`, `router.replace` y `router.go` dondejan de ser `window.history.pushState`, `window.history.replaceState` y `window.history.go`.

## 2.6. Nombrado 

### 2.6.1. Nombrado de rutas

Puede darse el caso en que nuestras rutas sean tan largas que hacer referencia a ellas en la navegación sea bastante dificil y tedioso. Para ello, podemos incluir un nombre en las rutas para referirnos a ellas.

Simplemente indica un `name`:

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
Y usalo en las navegaciones tanto declarativas:

```html
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
```

Como en las programáticas:

```js
router.push({ name: 'user', params: { userId: 123 }})
```

### 2.6.2. Nombrado de vistas

Por otro lado, dependiendo de la complejidad de nuestros componentes, puede darse el caso en que tengamos que indicar distintas vistas al mismo tiempo en un anidamiento. 

Podemos tener esto:

```html
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>
```

Donde tenemos un `router-view` con nombre `a` y otra con nombre `b`. Cuando un `router-view` no tiene nombre indica que es el `router-view` por defecto.

Ahora bien, en la configuración, tendremos que indicar tres componentes. Para hacerlo, hacemos que la configuración de `components` sea un objeto donde s eindica el nombre de la vista y el componente a renderizar:

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

Nos puede pasar lo mismo con las vistas anidadas y sus componentes hijos. (Hay que pensar en `vue-router` como otro arbol, en este caso de rutas):
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


`UserSettings` tiene varias vistas:

```html
<!-- UserSettings.vue -->
<div>
  <h1>User Settings</h1>
  <NavBar/>
  <router-view/>
  <router-view name="helper"/>
</div>
```

En la configuración quedaría así:

```js
{
  path: '/settings',
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

Puede darse caso en las que tengamos que hacer redirecciones entre rutas.

Es decir que dada la ruta `/a`, nos redirecione a la ruta `/b`:

```js
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' }
  ]
})
```

Podemos indicar en la redirección un nombre de ruta para que quede más simple:

```js
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: { name: 'foo' }}
  ]
})
```

Incluso podemos indicar una función para relaizar una redirección lógica:

```js
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: to => {
      // lógica
    }}
  ]
})
```

### 2.7.2. Alias

Podemos tambien indicar un alias. 

La diferencia con una redirección es que una redirección cuando el usuario vista `/a` la url será reemplazada por `/b`.

Por otro lado, un alias de `/a` como `/b` indica que cuando el usuario visita `/b`, en la url aparece `/b`, pero internamente se relacionará con `/a`. Es como si el usuario hubiera visitado `/a` pero seguimos mostrándole `/b`.

Para realizar esto, lo haríamos así:

```js
const router = new VueRouter({
  routes: [
    { path: '/a', component: A, alias: '/b' }
  ]
})
```

La ventaja de esto es que somos libres de mapear estructuras a urls arbitrarias.

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

```js
const router = new VueRouter({
  mode: 'history',
  routes: [...]
})
```

```apache
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteBase /
  RewriteRule ^index\.html$ - [L]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule . /index.html [L]
</IfModule>
```

```nginx
location / {
  try_files $uri $uri/ /index.html;
}
```

```js
const http = require('http')
const fs = require('fs')
const httpPort = 80

http.createServer((req, res) => {
  fs.readFile('index.htm', 'utf-8', (err, content) => {
    if (err) {
      console.log('We cannot open "index.htm" file.')
    }

    res.writeHead(200, {
      'Content-Type': 'text/html; charset=utf-8'
    })

    res.end(content)
  })
}).listen(httpPort, () => {
  console.log('Server listening on: http://localhost:%s', httpPort)
})
```

A tener en cuenta

```js
const router = new VueRouter({
  mode: 'history',
  routes: [
    { path: '*', component: NotFoundComponent }
  ]
})
```

## 2.10. Navigation Guard

### 2.10.1. Global Guards

```js
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  // ...
})
```

### 2.10.2. Global Resolve Guards

### 2.10.3. Global After Hooks

```js
router.afterEach((to, from) => {
  // ...
})
```

### 2.10.4. Per-Route Guard

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```

### 2.10.5. In-Component Guards

```js
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // called before the route that renders this component is confirmed.
    // does NOT have access to `this` component instance,
    // because it has not been created yet when this guard is called!
  },
  beforeRouteUpdate (to, from, next) {
    // called when the route that renders this component has changed,
    // but this component is reused in the new route.
    // For example, for a route with dynamic params `/foo/:id`, when we
    // navigate between `/foo/1` and `/foo/2`, the same `Foo` component instance
    // will be reused, and this hook will be called when that happens.
    // has access to `this` component instance.
  },
  beforeRouteLeave (to, from, next) {
    // called when the route that renders this component is about to
    // be navigated away from.
    // has access to `this` component instance.
  }
}
```

```js
beforeRouteEnter (to, from, next) {
  next(vm => {
    // access to component instance via `vm`
  })
}
```

```js
beforeRouteUpdate (to, from, next) {
  // just use `this`
  this.name = to.params.name
  next()
}
```

```js
beforeRouteLeave (to, from, next) {
  const answer = window.confirm('Do you really want to leave? you have unsaved changes!')
  if (answer) {
    next()
  } else {
    next(false)
  }
}
```

### 2.10.6. The Full Navigation Resolution Flow

## 2.11. Route Meta Fields

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      children: [
        {
          path: 'bar',
          component: Bar,
          // a meta field
          meta: { requiresAuth: true }
        }
      ]
    }
  ]
})
```

```js
router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {
    // this route requires auth, check if logged in
    // if not, redirect to login page.
    if (!auth.loggedIn()) {
      next({
        path: '/login',
        query: { redirect: to.fullPath }
      })
    } else {
      next()
    }
  } else {
    next() // make sure to always call next()!
  }
}
```

## 2.12. Data Fetching

### 2.12.1. Fetching After Navigation

```html
<template>
  <div class="post">
    <div class="loading" v-if="loading">
      Loading...
    </div>

    <div v-if="error" class="error">
      {{ error }}
    </div>

    <div v-if="post" class="content">
      <h2>{{ post.title }}</h2>
      <p>{{ post.body }}</p>
    </div>
  </div>
</template>
```

```js
export default {
  data () {
    return {
      loading: false,
      post: null,
      error: null
    }
  },
  created () {
    // fetch the data when the view is created and the data is
    // already being observed
    this.fetchData()
  },
  watch: {
    // call again the method if the route changes
    '$route': 'fetchData'
  },
  methods: {
    fetchData () {
      this.error = this.post = null
      this.loading = true
      // replace `getPost` with your data fetching util / API wrapper
      getPost(this.$route.params.id, (err, post) => {
        this.loading = false
        if (err) {
          this.error = err.toString()
        } else {
          this.post = post
        }
      })
    }
  }
}
```

### 2.12.2. Fetching Before Navigation

```js
export default {
  data () {
    return {
      post: null,
      error: null
    }
  },
  beforeRouteEnter (to, from, next) {
    getPost(to.params.id, (err, post) => {
      next(vm => vm.setData(err, post))
    })
  },
  // when route changes and this component is already rendered,
  // the logic will be slightly different.
  beforeRouteUpdate (to, from, next) {
    this.post = null
    getPost(to.params.id, (err, post) => {
      this.setData(err, post)
      next()
    })
  },
  methods: {
    setData (err, post) {
      if (err) {
        this.error = err.toString()
      } else {
        this.post = post
      }
    }
  }
}
```

## 2.13. Lazy Loading Routes

```js
const Foo = () => Promise.resolve({ /* component definition */ })
```

```js
import('./Foo.vue') // returns a Promise
```

```js
const Foo = () => import('./Foo.vue')
```

```js
const router = new VueRouter({
  routes: [
    { path: '/foo', component: Foo }
  ]
})
```

### 2.13.1. Grouping Components in the Same Chunk

```js
const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
```




