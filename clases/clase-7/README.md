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

### 1.2. Para qué son útiles

### 1.3. Tipos de test

### 1.4. Test unitarios

### 1.5. Partes de un test unitario

## 2. Jest

### 2.1. Qué es

### 2.2. Funcionalidades principales

### 2.3. Cómo se instala

### 2.4. Partes de un test unitario en Jest

### 2.5. Nuestro primer test

### 2.6. ¿Cómo integro Jest en mi proyecto de Vue?

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

## Ejercicio (I): arregla los tests

### 3.8. Testeando vue-router

### 3.9. Testeando vuex

#### 3.9.1. Mutaciones

#### 3.9.2. Acciones

#### 3.9.3. Getters

## Ejercicio (II): inserta los tests