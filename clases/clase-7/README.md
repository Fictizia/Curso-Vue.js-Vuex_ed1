# Clase 6

## Planning del día

1. Repaso clase anterior
2. ¿Por qué necesitamos probar nuestros componentes?
3. ¿Qué es un test unitario?
4. Jest
5. Entendiendo las partes de vue-test-utils
6. Probando componentes visuales
7. Testeando partes asíncronas
8. Probando vue-router
9. Probando vuex

## Índice

- [1. Introducción al testing automático](#1-introducción-al-testing-automático)
  - [1.1. Qué es un test automático](#11-qué-es-un-test-automático)
  - [1.2. Para qué son útiles](#12-para-qué-son-útiles)
  - [1.3. Tipos de test](#13-tipos-de-test)
  - [1.4. Test unitarios](#14-test-unitarios)
  - [1.5. Partes de un test unitario](#15-partes-de-un-test-unitario)
- [2. Jest](#2-jest)
  - [2.1. Qué es](#21-qué-es)
  - [2.2. Funcionalidades principales](#22-funcionalidades-principales)
  - [2.3. Cómo se instala](#23-cómo-se-instala)
  - [2.4. Partes de un test unitario en Jest](#24-partes-de-un-test-unitario-en-jest)
  - [2.5. Nuestro primer test](#25-nuestro-primer-test)
  - [2.6. ¿Cómo integro Jest en mi proyecto de Vue?](#26-¿cómo-integro-jest-en-mi-proyecto-de-vue)
- [3. vue-test-utils](#3-vue-test-utils)
  - [3.1. Renderizando componentes](#31-renderizando-componentes)
  - [3.2. Encontrando elementos en el componente](#32-encontrando-elementos-en-el-componente)
  - [3.3. Testeando props](#33-testeando-props)
  - [3.4. Testeando propiedades computadas](#34-testeando-propiedades-computadas)
  - [3.5. Testeando formularios](#35-testeando-formularios)
  - [3.6. Testeando emisión de eventos](#36-testeando-emisión-de-eventos)
  - [3.7. Mockeando objetos globales](#37-mockeando-objetos-globales)
- [Ejercicio (I): arregla los tests](#ejercicio-i-arregla-los-tests)
  - [3.8. Testeando vue-router](#38-testeando-vue-router)
  - [3.9. Testeando vuex](#39-testeando-vuex)
    - [3.9.1. Mutaciones](#391-mutaciones)
    - [3.9.2. Acciones](#392-acciones)
    - [3.9.3. Getters](#393-getters)
- [Ejercicio (II): inserta los tests](#ejercicio-ii-inserta-los-tests)

## 1. Introducción al testing automático

### 1.1. Qué es un test automático

El testing de software es el arte de medir y mantener la calidad del software para asegurar que las expectativas y requerimientos del usuario, el valor de negocio, los requisitos no funcionales como la seguridad, confiabilidad y tolerancia a fallos, y las políticas operacionales se cumplan. El testing es un esfuerzo del equipo para conseguir un mínimo de calidad y tener una “definición de hecho” entendida y aceptada por todos.

> Definición de hecho – es una definición del equipo y un compromiso de calidad que se aplicará a la solución en cada iteración (también se puede definir en el nivel de Tareas o de Historias de Usuario). Ten en cuenta el diseño, las revisiones de código, refactorización y testing cuando discutáis y defináis vuestra “definición de hecho”.

### 1.2. Para qué son útiles

Cuando alguien pregunta por qué son importantes los tests, le solemos preguntar si creen que es importante que un avión comercial que usan habitualmente para cruzar océanos sea testeado meticulosamente antes de cada vuelo. ¿Es importante que el avión sea testeado? Si, ok, ahora, ¿es importante que el altímetro sea testeado aparte? Por supuesto. Eso es el test unitario… testear el altímetro. No garantiza que el avión vaya a volar, pero no puede volar de forma segura sin él.

La importancia de los test, empieza con el proyecto y continúa durante todo el ciclo de vida de la aplicación, también depende de si estamos buscando:

- Código de calidad desde el principio.
- Menos bugs.
- Código auto explicativo.
- Reducir el coste de corregir errores encontrándolos lo antes posible.
- Sentido de responsabilidad del código, estar orgullosos de nuestro código.

### 1.3. Tipos de test

- Test exploratorio: El tester imagina posibles escenarios que no se hayan cubierto por otros test. Es muy útil cuando se observa al usuario usando el sistema. NO hay test predefinidos.
- Test de integración: Testear diferentes componentes de la solución trabajando como si fueran uno.
- Test de carga: Testear cómo se comporta el sistema con cargas de trabajo en un entorno controlado.
- Test de Regresión: Asegura que el sistema mantiene los mínimos de calidad después de hacer cambios como corregir bugs. Usa una mezcla de tests unitarios y de sistema.
- Smoke Test: Se usa para testear una nueva característica o idea antes de subir al repositorio de código los cambios.
- Test de sistema: Testea el sistema completo con características fijas y comprueba los requerimientos no funcionales.
- Test unitario: Un test de la unidad más pequeña de código (método, clase, etc.) que puede ser testeada de manera aislada del sistema. Ver Verifying Code By Using Unit Test yLa delgada línea entre buen y mal test unitario en esta guía para más información.
- Test de aceptación de usuario: Cuando se acerca el final de los ciclos del producto, se invita a los usuarios a que realicen test de aceptación en escenarios reales, normalmente basados en casos de tests

![Tipos de test](imgs/tests.png)

### 1.4. Test unitarios

### 1.5. Partes de un test unitario

## 2. Jest

### 2.1. Qué es

### 2.2. Funcionalidades principales

### 2.3. Cómo se instala

### 2.4. Partes de un test unitario en Jest

### 2.5. Nuestro primer test

### 2.6. Cómo integro Jest en mi proyecto de Vue

## 3. vue-test-utils

### 3.1. Renderizando componentes

```js
const Child = Vue.component("Child", {
  name: "Child",

  template: "<div>Child component</div>"
})
```

```js
const Parent = Vue.component("Parent", {
  name: "Parent",

  template: "<div><child /></div>"
})
```

```js
const shallowWrapper = shallowMount(Child)
const mountWrapper = mount(Child)

console.log(shallowWrapper.html())
console.log(mountWrapper.html())
```

```html
<div>Child component</div>
```

```js
const shallowWrapper = shallowMount(Parent)
const mountWrapper = mount(Parent)

console.log(shallowWrapper.html())
console.log(mountWrapper.html())
mountWrapper.html() now yields:
```

```html
<div><div>Child component</div></div>
```

```html
<div><vuecomponent-stub></vuecomponent-stub></div>
```

### 3.2. Encontrando elementos en el componente

```html
<template>
  <div>Child</div>
</template>
```

```js
<script>
export default {
  name: "Child"
}
</script>
```

```html
<template>
  <div>
    <span v-show="showSpan">
      Parent Component
    </span>
    <Child v-if="showChild" />
  </div>
</template>
```

```js
<script>
import Child from "./Child.vue"

export default {
  name: "Parent",

  components: { Child },

  data() {
    return {
      showSpan: false,
      showChild: false
    }
  }
}
</script>
```

```js
import { mount, shallowMount } from "@vue/test-utils"
import Parent from "@/components/Parent.vue"

describe("Parent", () => {
  it("does not render a span", () => {
    const wrapper = shallowMount(Parent)

    expect(wrapper.find("span").isVisible()).toBe(false)
  })
})
```

```js
it("does render a span", () => {
  const wrapper = shallowMount(Parent, {
    data() {
      return { showSpan: true }
    }
  })

  expect(wrapper.find("span").isVisible()).toBe(true)
})
```

```js
import Child from "@/components/Child.vue"

it("does not render a Child component", () => {
  const wrapper = shallowMount(Parent)

  expect(wrapper.find(Child).exists()).toBe(false)
})
```

```js
it("renders a Child component", () => {
  const wrapper = shallowMount(Parent, {
    data() {
      return { showChild: true }
    }
  })

  expect(wrapper.find({ name: "Child" }).exists()).toBe(true)
})
```

```html
<template>
  <div>
    <Child v-for="id in [1, 2 ,3]" :key="id" />
  </div>
</template>
```

```js
<script>
import Child from "./Child.vue"

export default {
  name: "ParentWithManyChildren",

  components: { Child }
}
</script>
```

```js
it("renders many children", () => {
  const wrapper = shallowMount(ParentWithManyChildren)

  expect(wrapper.findAll(Child).length).toBe(3)
})
```

### 3.3. Testeando props

```html
<template>
  <div>
    <span v-if="isAdmin">Admin Privledges</span>
    <span v-else>Not Authorized</span>
    <button>
      {{ msg }}
    </button>
  </div>
</template>
```

```js
<script>
export default {
  name: "SubmitButton",

  props: {
    msg: {
      type: String,
      required: true
    },
    isAdmin: {
      type: Boolean,
      default: false
    }
  }
}
</script>
```

```js
import { shallowMount } from '@vue/test-utils'
import SubmitButton from '@/components/SubmitButton.vue'

describe('SubmitButton.vue', () => {
  it("displays a non authorized message", () => {
    const msg = "submit"
    const wrapper = shallowMount(SubmitButton,{
      propsData: {
        msg: msg
      }
    })

    console.log(wrapper.html())

    expect(wrapper.find("span").text()).toBe("Not Authorized")
    expect(wrapper.find("button").text()).toBe("submit")
  })
})
```

```js
import { shallowMount } from '@vue/test-utils'
import SubmitButton from '@/components/SubmitButton.vue'

describe('SubmitButton.vue', () => {
  it('displays a admin privledges message', () => {
    const msg = "submit"
    const isAdmin = true
    const wrapper = shallowMount(SubmitButton,{
      propsData: {
        msg,
        isAdmin
      }
    })

    expect(wrapper.find("span").text()).toBe("Admin Privledges")
    expect(wrapper.find("button").text()).toBe("submit")
  })
})
```

```js
const msg = "submit"
const factory = (propsData) => {
  return shallowMount(SubmitButton, {
    propsData: {
      msg,
      ...propsData
    }
  })
}
```

```js
describe("SubmitButton", () => {
  describe("has admin privledges", ()=> {
    it("renders a message", () => {
      const wrapper = factory()

      expect(wrapper.find("span").text()).toBe("Not Authorized")
      expect(wrapper.find("button").text()).toBe("submit")
    })
  })

  describe("does not have admin privledges", ()=> {
    it("renders a message", () => {
      const wrapper = factory({ isAdmin: true })

      expect(wrapper.find("span").text()).toBe("Admin Privledges")
      expect(wrapper.find("button").text()).toBe("submit")
    })
  })
})
```

### 3.4. Testeando propiedades computadas

```html
<template>
  <div>
    {{ numbers }}
  </div>
</template>
```

```js
<script>
export default {
  name: "NumberRenderer",

  props: {
    even: {
      type: Boolean,
      required: true
    }
  },

  computed: {
    numbers() {
        const evens = []
        const odds = []

        for (let i = 1; i < 10; i++) {
            if (i % 2 === 0) {
                evens.push(i)
            } else {
                odds.push(i)
            }
        }

        return this.even === true ? evens.join(", ") : odds.join(", ")
    }
  }
}
</script>
```

```js
import { shallowMount } from "@vue/test-utils"
import NumberRenderer from "@/components/NumberRenderer.vue"

describe("NumberRenderer", () => {
  it("renders even numbers", () => {
    const wrapper = shallowMount(NumberRenderer, {
      propsData: {
        even: true
      }
    })

    expect(wrapper.text()).toBe("2, 4, 6, 8")
  })
})
```

```js
it("renders odd numbers", () => {
  const localThis = { even: false }

  expect(NumberRenderer.computed.numbers.call(localThis)).toBe("1, 3, 5, 7, 9")
})

```

### 3.5. Testeando formularios

```html
<template>
  <div>
    <form @submit.prevent="handleSubmit">
      <input v-model="username" data-username>
      <input type="submit">
    </form>

    <div
      class="message"
      v-show="submitted"
    >
      Thank you for your submission, {{ username }}.
    </div>
  </div>
</template>
```

```js
<script>
  export default {
    name: "FormSubmitter",

    data() {
      return {
        username: '',
        submitted: false
      }
    },

    methods: {
      handleSubmit() {
        this.submitted = true
      }
    }
  }
</script>
```

```js
import { shallowMount } from "@vue/test-utils"
import FormSubmitter from "@/components/FormSubmitter.vue"

describe("FormSubmitter", () => {
  it("reveals a notification when submitted", () => {
    const wrapper = shallowMount(FormSubmitter)

    wrapper.find("[data-username]").setValue("alice")
    wrapper.find("form").trigger("submit.prevent")

    expect(wrapper.find(".message").text())
      .toBe("Thank you for your submission, alice.")
  })
})
```

### 3.6. Testeando emisión de eventos

```html
<template>
  <div>
  </div>
</template>
```

```js
<script>
  export default {
    name: "Emitter",

    methods: {
      emitEvent() {
        this.$emit("myEvent", "name", "password")
      }
    }
  }
</script>
```

```js
import Emitter from "@/components/Emitter.vue"
import { shallowMount } from "@vue/test-utils"

describe("Emitter", () => {
  it("emits an event with two arguments", () => {
    const wrapper = shallowMount(Emitter)

    wrapper.vm.emitEvent()

    console.log(wrapper.emitted())
  })
})
```

### 3.7. Mockeando objetos globales

```html
<template>
  <div class="hello">
    {{ $t("helloWorld") }}
  </div>
</template>
```

```js
<script>
  export default {
    name: "Bilingual"
  }
</script>
```

```js
import { shallowMount } from "@vue/test-utils"
import Bilingual from "@/components/Bilingual.vue"

describe("Bilingual", () => {
  it("renders successfully", () => {
    const wrapper = shallowMount(Bilingual)
  })
})
```

```js
import { shallowMount } from "@vue/test-utils"
import Bilingual from "@/components/Bilingual.vue"

describe("Bilingual", () => {
  it("renders successfully", () => {
    const wrapper = shallowMount(Bilingual, {
      mocks: {
        $t: (msg) => msg
      }
    })
  })
})
```

```js
import VueTestUtils from "@vue/test-utils"

VueTestUtils.config.mocks["mock"] = "Default Mock Value"
```

```js
import VueTestUtils from "@vue/test-utils"
import translations from "./src/translations.js"

const locale = "en"

VueTestUtils.config.mocks["$t"] = (msg) => translations[locale][msg]
```

```js
describe("Bilingual", () => {
  it("renders successfully", () => {
    const wrapper = shallowMount(Bilingual)

    console.log(wrapper.html())
  })
})
```

## Ejercicio (I): arregla los tests

### 3.8. Testeando vue-router

```html
<template>
  <div id="app">
    <router-view />
  </div>
</template>
```

```js
<script>
export default {
  name: 'app'
}
</script>
```

```html
<template>
  <div>Nested Route</div>
</template>
```

```js
<script>
export default {
  name: "NestedRoute"
}
</script>
```

```js
import NestedRoute from "@/components/NestedRoute.vue"

export default [
  { path: "/nested-route", component: NestedRoute }
]
```

```js
import Vue from "vue"
import VueRouter from "vue-router"
import routes from "./routes.js"

Vue.use(VueRouter)

export default new VueRouter({ routes })
```

```js
import { shallowMount, mount, createLocalVue } from "@vue/test-utils"
import App from "@/App.vue"
import VueRouter from "vue-router"
import NestedRoute from "@/components/NestedRoute.vue"
import routes from "@/routes.js"

const localVue = createLocalVue()
localVue.use(VueRouter)

describe("App", () => {
  it("renders a child component via routing", () => {
    const router = new VueRouter({ routes })
    const wrapper = mount(App, { localVue, router })

    router.push("/nested-route")

    expect(wrapper.find(NestedRoute).exists()).toBe(true)
  })
})
```

### 3.9. Testeando vuex

#### 3.9.1. Mutaciones

```js
export default {
  SET_POST(state, { post }) {

  }
}
```

```js
import mutations from "@/store/mutations.js"

describe("SET_POST", () => {
  it("adds a post to the state", () => {
    const post = { id: 1, title: "Post" }
    const state = {
      postIds: [],
      posts: {}
    }

    mutations.SET_POST(state, { post })

    expect(state).toEqual({
      postIds: [1],
      posts: { "1": post }
    })
  })
})
```

```markdown
FAIL  tests/unit/mutations.spec.js
● SET_POST › adds a post to the state

  expect(received).toEqual(expected)

  Expected value to equal:
    {"postIds": [1], "posts": {"1": {"id": 1, "title": "Post"}}}
  Received:
    {"postIds": [], "posts": {}}
```

```js
export default {
  SET_POST(state, { post }) {
    state.postIds.push(post.id)
  }
}
```

```markdown
Expected value to equal:
  {"postIds": [1], "posts": {"1": {"id": 1, "title": "Post"}}}
Received:
  {"postIds": [1], "posts": {}}
```

```js
export default {
  SET_POST(state, { post }) {
    state.postIds.push(post.id)
    state.posts = { ...state.posts, [post.id]: post }
  }
}
```

#### 3.9.2. Acciones

```js
import axios from "axios"

export default {
  async authenticate({ commit }, { username, password }) {
    const authenticated = await axios.post("/api/authenticate", {
      username, password
    })

    commit("set_authenticated", authenticated)
  }
}
```

```js
describe("authenticate", () => {
  it("authenticated a user", async () => {
    const commit = jest.fn()
    const username = "alice"
    const password = "password"

    await actions.authenticate({ commit }, { username, password })

    expect(url).toBe("/api/authenticate")
    expect(body).toEqual({ username, password })
    expect(commit).toHaveBeenCalledWith(
      "SET_AUTHENTICATED", true)
  })
})
```

```markdown
 FAIL  tests/unit/actions.spec.js
  ● authenticate › authenticated a user

    SyntaxError: The string did not match the expected pattern.

      at XMLHttpRequest.open (node_modules/jsdom/lib/jsdom/living/xmlhttprequest.js:482:15)
      at dispatchXhrRequest (node_modules/axios/lib/adapters/xhr.js:45:13)
      at xhrAdapter (node_modules/axios/lib/adapters/xhr.js:12:10)
      at dispatchRequest (node_modules/axios/lib/core/dispatchRequest.js:59:10)
```

```js
let url = ''
let body = {}

jest.mock("axios", () => ({
  post: (_url, _body) => {
    return new Promise((resolve) => {
      url = _url
      body = _body
      resolve(true)
    })
  }
}))
```

#### 3.9.3. Getters

```js
const state = {
  dogs: [
    { name: "lucky", breed: "poodle", age: 1 },
    { name: "pochy", breed: "dalmatian", age: 2 },
    { name: "blackie", breed: "poodle", age: 4 }
  ]
}
```

```js
export default {
  poodles: (state) => {
    return state.dogs.filter(dog => dog.breed === "poodle")
  },

  poodlesByAge: (state, getters) => (age) => {
    return getters.poodles.filter(dog => dog.age === age)
  }
}
```

```js
computed: {
  puppies() {
    return this.$store.getters.poodlesByAge(1)
  }
}
```

```js
import getters from "../../src/store/getters.js"

const dogs = [
  { name: "lucky", breed: "poodle", age: 1 },
  { name: "pochy", breed: "dalmatian", age: 2 },
  { name: "blackie", breed: "poodle", age: 4 }
]
const state = { dogs }

describe("poodles", () => {
  it("returns poodles", () => {
    const actual = getters.poodles(state)

    expect(actual).toEqual([ dogs[0], dogs[2] ])
  })
})
```

```js
describe("poodlesByAge", () => {
  it("returns poodles by age", () => {
    const poodles = [ dogs[0], dogs[2] ]
    const actual = getters.poodlesByAge(state, { poodles })(1)

    expect(actual).toEqual([ dogs[0] ])
  })
})
```

## Ejercicio (II): inserta los tests