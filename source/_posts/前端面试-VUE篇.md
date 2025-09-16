---
title: 前端面试-VUE篇
date: 2025-09-03 23:04:13
tags:
---
## 1.虚拟dom渲染到页面上时，框架做了什么？

虚拟DOM（Virtual DOM）是一种编程概念和内存中的数据结构，它使用JavaScript对象来表示和模拟真实DOM树的结构和状态。
创建虚拟 DOM 目的就是为了更好将虚拟的节点渲染到页面视图中，虚拟 DOM 对象的节点与真实 DOM 的属性一一照应

虚拟DOM 渲染到页面时，前端框架会先创建或更新虚拟DOM 树，然后利用 Diff 算法 找出新旧虚拟DOM 树的差异，最后根据差异生成最小的真实DOM 操作指令，批量执行这些操作，并处理事件绑定和生命周期钩子，以高效更新页面并保持与数据的同步。
核心处理流程如下：

1. 虚拟DOM 创建/更新：
   当应用数据发生变化时，框架会生成一个新的虚拟DOM 树。
2. 差异比较(Diff)：
   框架使用Diff 算法比较新的虚拟DOM 树和上一次渲染的虚拟DOM 树，找出两者的不同之处。
3. 生成真实DOM 操作：
   根据比较结果，框架会生成最小化的真实DOM 操作指令，例如创建新节点、更新属性、删除节点等。
4. 批量执行DOM 操作：
   框架会将这些DOM 操作批量地应用到真实的DOM 上，以减少对真实DOM 的直接改动，从而提高性能。
5. 事件处理：
   框架会为新生成的或更新的DOM 节点绑定事件监听器，以便响应用户的交互行为。
6. 生命周期钩子触发：
   在DOM 更新的过程中，框架还会根据组件的生命周期触发相应的钩子函数，为开发者提供扩展和定制的能力。

## 2. Vue有了数据响应式，为什么还需要diff算法？

**数据响应式**
Vue的数据响应式系统通过Object.defineProperty()（Vue2） 或者ES6的Proxy （Vue3）来实现，主要解决以下问题：

1. 数据绑定：保证视图与数据的同步更新，当数据发生变化时，视图会自动更新，避免手动操作DOM。
2. 依赖追踪：Vue能够追踪每个数据的依赖关系，即哪些组件或者计算属性依赖于某个数据。当数据变化时，自动更新依赖的组件或者计算属性。

**虚拟DOM和Diff算法**
虚拟DOM是一种内存中的表示结构，它是对真实DOM的抽象。Diff算法是一种高效的更新DOM的策略，它通过比较新旧虚拟DOM树的差异，最小化更新操作，提高页面的渲染效率。

**为什么需要Diff算法？**

1. 性能优化：直接操作真实DOM是非常昂贵的，而虚拟DOM可以在内存中快速比较和计算差异。Diff算法帮助减少更新操作的次数和返回，从而提高页面的渲染效率。
2. 批量更新：Diff算法能够将多次DOM更新操作合并为一次，避免频繁的DOM操作，减少了浏览器的重排和重绘。
3. 跨平台兼容：虚拟DOM和Diff算法使得Vue可以在不同的平台（如浏览器、小程序、Node.js）上运行，并保持一致的渲染结果。
4. 更新效率：虽然响应式系统可以自动更更新视图，但是如果每次数据变化都直接操作真实DMO，可能会带来性能问题。Diff算法可以智能的比较新旧DOM树的变化，只更新必要部分，从而提高了更新效率。

**综合作用**

1. 数据响应式：保证了数据和视图的同步更新，提供了便捷的开发方式。
2. 虚拟DOM+Diff算法：提高页面渲染的效率和性能，减少了不必要的DOM操作，确保了页面的流畅性和响应性。

## 3. Vue2和Vue3的差异

* 源码层面

  | 版本 | 区别                                                      |
  | ---- | --------------------------------------------------------- |
  | vue2 | javascript 使用flow进行类型检测                           |
  | vue3 | 源码使用typescript进行重构，vue对typescript支持更加友好了 |
* 性能层面

  | 版本 | 区别                                                                                                |
  | ---- | --------------------------------------------------------------------------------------------------- |
  | vue2 | 使用object.defineProperty来劫持数据的setter和getter方法，对象改变需要借助api去深度监听              |
  | vue3 | 使用Proxy来实现数据劫持，删除了一些api($on,$once,$off) fiter等，优化了Block tree,solt,diff 算法等 |
* api方面

  | 版本 | 区别                                                                                               |
  | ---- | -------------------------------------------------------------------------------------------------- |
  | vue2 | OptionsAPI 在options里写data,methods,created等描述组件对象，多个逻辑可能在不不同地方，代码内聚性低 |
  | vue3 | CompositionAPI 将模块相关代码统一放在一个地方处理，不需要在多个options里查找                       |
* hook函数增加代码复用性

  * vue2使用mixins进行代码逻辑共享，mixins也是由一大堆options组成，如果有多个mixins则可能造成命名冲突等问题。
  * vue3可以通过hook函数 将一部分独立的逻辑抽离出去，并且也是响应式的
* 代码写法方面

  * vue3支持在template中写多个根，vue2只能有一个
  * 当内部有异步函数，需要使用到await的时候，可以直接使用，不需要在setup前面加async
* 生命周期

  | vue2            | vue3              | 生命周期                    |
  | --------------- | ----------------- | --------------------------- |
  | beforeCreate()  | setup()           | 组件开始创建数据实例之前    |
  | created()       | setup()           | 组件数据创建数据实例完成    |
  | beforeMount()   | onBeforeMount()   | DOM挂载之前                 |
  | mounted()       | onMounted()       | DOM挂载完成（页面完成渲染） |
  | beforeUpdate()  | onBeforeUpdate()  | 组件数据更新之前            |
  | updated()       | onUpdated()       | 组件数据更新之后            |
  | beforeDestroy() | onBeforeUnmount() | 组件销毁之前                |
  | destroyed()     | onUnmounted()     | 组件销毁之后                |

## 4.在Vue中父组件怎么监听到子组件的生命周期？

1. 通过事件通信：子组件在生命周期钩子中触发自定义事件，父组件监听该事件，并执行相应的逻辑。
2. 使用ref:父组件访问子组件的实例，并在父组件的生命周期钩子中调用子组件的方法。
3. 在vue2中有使用 @hook 修饰符（主要用于Vue 2.6+）,vue3没有

   ```javascript
    <template>
      <ChildComponent @hook:mounted="handleChildMountedHook" />
    </template>

    <script>
    export default {
      methods: {
        handleChildMountedHook() {
          console.log('父组件通过 @hook:mounted 监听到子组件挂载');
        }
      }
    }
    </script>
   ```

## 5. Vue响应式原理？

[参考链接](https://juejin.cn/post/7516369217768898600)

1. 何为响应式：
   所谓数据响应式就是建立响应式数据与依赖（调用了响应式数据的操作）之间的关系，当响应式数据发生变化时，可以通知那些使用了这些响应式数据的依赖操作进行相关更新操作，可以是DOM更新，也可以是执行一些回调函数。
2. vue2和vue3区别：

   * Vue2响应式：利用 Object.defineProperty() 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。也就是对属性的读取、修改进行拦截（数据劫持）。
   * Vue3响应式：Proxy 对象用于创建一个对象的代理，从而实现基本操作的拦截和自定义（如属性查找、赋值、枚举、函数调用等）。也就是拦截对象中任意属性的变化，包括：属性值的读写、属性的添加、属性的删除等。直接监听对象而非属性。
3. 为什么Vue3不延续Vue2的方案？

   * 在 Vue2 中，对于一个深层属性嵌套的对象，要劫持它内部深层次的变化，就需要递归遍历这个对象，执行 Object.defineProperty 把每一层对象数据都变成响应式的，
   * defineProperty定义对象不能监听添加额外属性或修改额外添加的属性的变化；
   * defineProperty定义对象不能监听根据自身数组下标修改数组元素的变化。
   * Proxy 返回的是一个新对象，而 Object.defineProperty 只能遍历对象属性直接修改。
4. vue3响应式手写

   * reactive
     ![ ](5-1.png)

   ```javascript
   const reactive =  <T extends object>(target: T) => {
       // 限制reactive只能传递引用类型，如果传递的不是引用类型，则出警告并将原始值直接返回
       if (typeof target !== 'object' || target === null) {
           console.warn('Reactive can only be applied to objects');
           return target
       }

       // 返回原始值的代理对象
       return new Proxy(target, {
           get(target, key, receiver) {
               const value = Reflect.get(target, key, receiver);
               // 这里需要收集依赖（后面实现）
               track(target, key);
               // 如果值是对象，则递归调用reactive
               if (typeof value === 'object' && value !== null) {
                   return reactive(value); 
               }

               return value;
           },
           set(target, key, value, receiver) {
               const result = Reflect.set(target, key, value, receiver);

               // 这里需要触发更新（后面实现）
               trigger(target, key)
               return result;
           },
       })
   }

   export default reactive;
   ```

   * track（依赖收集），该方法的主要作用就是收集依赖，这里可以使用Map去进行存储依赖关系，Map的key就是我们的代理对象，而value还是一个嵌套的map，存储代理对象的每个key以及对应的依赖函数数组，因为每个key都可以有多个依赖。
     ![ ](5-2.png)

   ```javascript
   const targetMap = new WeakMap()
   export const track = (target: object, key: PropertyKey) => {

       // 先找到target对应的依赖
       let depsMap = targetMap.get(target)

       if(!depsMap) {
           // 如果没找到，则说明是第一次收集，需要初始化
           depsMap = new Map()
           targetMap.set(target, depsMap)
       }
       // 接着需要对代理对象的属性进行依赖收集
       let deps = depsMap.get(key)
       if(!deps) {
           deps = new Set()
       }
       if (!deps.has(activeEffect) && activeEffect) { 
           // 防止重复注册 
           deps.add(activeEffect) 

       }
       depsMap.set(key, deps)
       console.log(`Tracking ${String(key)} on`, target);
   };
   ```

   * trigger（更新触发），该方法的主要作用就是从 targetMap 中，根据 target 和 key 找到对应的依赖函数集合 deps，然后遍历 deps 执行依赖函数

   ```javascript
   export const trigger = (target: object, key: PropertyKey) => {
       // 先找到target对应的依赖map
       // console.log('----',targetMap)
       const depsMap = targetMap.get(target)
       if(!depsMap) return
       // 再找到对应属性的依赖
       const deps = depsMap.get(key)
       // 如果没有依赖可执行，则返回
       if(!deps) return
       // 最后遍历整个依赖set分别执行
       console.log('--deps', deps)
       deps.forEach(effect => {
           effect?.()
       })
   };
   ```

   * effect（副作用管理）），该副作用函数主要是在依赖更新的时候调用，它接受一个函数，在被调用的时候执行这个函数.在 effectFn 函数内部，把函数赋值给全局变量 activeEffect；然后执行 fn() 的时候，就会触发响应式对象的 get 函数，get 函数内部就会把 activeEffect 存储到依赖地图中，完成依赖的收集

   ```javascript
   let activeEffect
   export const effect = (fn: () => void) => {
       const effectFn = () => {
           activeEffect = effectFn
           fn()
       }

       effectFn()
   }

   ```

## 6. Vue中，created和mounted两个钩子之间调用时间差值受什么影响？

created 和 mounted 这两个生命周期钩子，分别在实例创建和挂载的不同阶段被调用。
它们之间的时间差值主要受以下几个因素的影响：

1. 模板编译时间：

   * 当实例被创建时，Vue 会编译模板（或将模板转换为渲染函数），这个过程在 created 钩子之前完成。如果模板非常复杂或包含大量指令、组件，这个过程会更耗时，从而延长 created 和 mounted 之间的时间差。
2. 虚拟 DOM 渲染时间：

   * 在 mounted 钩子调用之前，Vue 会将虚拟 DOM 渲染为实际的 DOM 元素。渲染复杂的组件树或处理大量数据绑定会增加这段时间。
3. 异步操作：

   * 如果在 created 钩子中发起了异步操作（如 API 请求），这些操作本身不会直接影响 created 和 mounted 的时间差，但如果这些操作涉及数据更新，可能会间接增加挂载时间。
4. 浏览器性能：

   * 浏览器的性能和设备的硬件配置也会影响模板编译和 DOM 渲染的速度，从而影响这两个钩子之间的时间差。
5. 其他钩子执行时间：

   * 在 beforeCreate、created、beforeMount 等钩子中执行的代码也会影响到 mounted 钩子的触发时间。如果这些钩子中有大量计算或耗时操作，也会增加时间差。

## 7. Vue中推荐在哪个生命周期发起请求？

在 Vue 中，推荐在 mounted 生命周期发起请求。 mounted 生命周期是在组件挂载完成后调用的，此时 DOM 已经渲染完成，因此可以确保请求数据时，DOM 已经存在，不会出现数据请求失败或者数据请求晚于 DOM 的情况。
特定场景：

* SSR（服务器端渲染）:在服务器端渲染中，Vue实例的mounted钩子不会被调用，因为DOM并不会被真正挂载，这种情况需要在created钩子中发起请求。
* 依赖数据初始化：如果组件在挂载之前就需要某些数据来初始化，可以在created钩子中发起请求，以确保数据在组件挂载时已经可用。

## 8. Vue渲染流程？

Vue的渲染流程大致分为解析编译、挂载渲染、更新渲染三个阶段。

1. 解析编译：
   Vue将模板（或组件的render函数）编译成一个渲染函数，该函数用于生成虚拟DOM树。
2. 挂载渲染：
   运行时渲染器调用渲染函数，生成虚拟DOM树。然后，它会遍历虚拟DOM树，创建真实的DOM节点并将其插入到页面中，完成首次渲染。
3. 更新渲染：
   当组件的数据发生变化时，Vue会重新执行渲染函数，生成新的虚拟DOM树。渲染器随后会比较新旧两棵虚拟DOM树的差异，并只将需要更改的部分应用到真实DOM上。

详细步骤：

* 初始化：
  用户在main.js中通过 `new Vue()或createApp()`创建Vue实例。
* 编译模板：
  Vue通过解析模板生成抽象语法树（AST），再根据AST生成渲染函数。
* 挂载（初次渲染）：
  * 渲染函数被执行，生成虚拟DOM（Virtual DOM，简称VNode）树。
  * 渲染器调用 `patch()`方法，根据虚拟DOM创建真实的DOM节点。
  * 真实DOM树最终被插入到页面中的指定挂载点（如 `<div id="app"></div>`）。
* 更新：
  * 当响应式数据发生变化时，相关的组件会被重新渲染。
  * 渲染函数再次执行，生成新的虚拟DOM树。
  * `patch()`方法会对比新旧虚拟DOM树的差异（使用Diff算法）。
  * 只有差异部分会被更新到真实DOM上，避免不必要的重绘和重排，从而提高性能。

核心概念：

* 虚拟DOM (Virtual DOM)：:Vue使用虚拟DOM作为真实DOM的中间层，将DOM操作抽象为JavaScript对象。
* Diff算法：:用于比较新旧两棵虚拟DOM树，找出需要更新的节点。
* 渲染器：:负责调用渲染函数、生成虚拟DOM，并根据虚拟DOM操作真实DOM。

## 8. vue中computed和watch的区别？

* computed 是计算属性，有缓存功能。它的底层会通过 dirty 变量来判断是否重新计算。只有在依赖数据发生变化时才会重新计算，性能会更好。
* 而 watch 没缓存，但 watch 能执行异步和比较复杂的逻辑操作。
* computed 适用于多对一，也就是这个缓存属性受多个属性影响，例如购物车商品结算；
* 而 watch 适用于一对多，也就是监听的属性影响多个属性，例如搜索框搜索。

## 9. 组合式API与选项式API区别？

* 在逻辑组织和逻辑复用方面，组合式优于选项式。
* 组合式API几乎是函数，会有更好的类型推断。
* 组合式API 对tree-shaking友好，代码也更容易压缩
* 组合式API中见不到this的使用，减少了this指向不明的情况

## 10. vue3相比于vue2的性能提升体现在哪里？

1. **编译阶段**
   * **diff算法优化**：vue3在diff算法中相比vue2添加了静态标记，为会发生变化的地方添加了一个flag，下次发生变化的时候直接找该地方进行比较。
   * **静态提升**：vue中对不参与更新的元素会做静态提升，只会被创建一次，在有其他变化导致的重新渲染时直接复用。
   * **事件监听缓存**：默认情况下绑定事件行为会被视为动态绑定，所以每次都会追踪它的变化。
   * **SSR优化**：当静态内容达到一定量级，会用createStaticVNode方法在客户端去生成一个static node，这些静态node，会被直接innerHtml,就不需要创建对象，然后根据对象渲染。
2. **源码体积**
   vue3移除了不常用API，并引入tree shanking,任何一个函数仅仅只在其被用到的时才打包。
3. **响应式系统**
   vue2采用object.defineProperty,来劫持对象，然后深度遍历所有属性。而vue3采用proxy，可以对整个对象监听，不需要深度遍历
   * 可以监听动态属性的添加
   * 可以监听到数组的索引和数组length属性
   * 可以监听删除属性

## 11. 说说对vue中keep-alive的理解

1. 含义：keep-alive是vue内置组件，能在组件切换过程中将状态保留在内存中，防止重复渲染DOM。keep-alive包裹动态组件时，会缓存不活动的组件实例，而不是销毁他们。

   * keep-alive组件的属性有：

     * include：指定需要缓存的组件名称
     * exclude：指定不需要缓存的组件名称
     * ax：指定缓存组件的最大数量
   * 设置keep-alive的组件会多出两个生命周期钩子（onActivated与onDeactivated）

     ```javascript
     <script setup>
     import { onActivated, onDeactivated } from 'vue'

     onActivated(() => {
       // 调用时机为首次挂载
       // 以及每次从缓存中被重新插入时
     })

     onDeactivated(() => {
       // 在从 DOM 上移除、进入缓存
       // 以及组件卸载时调用
     })
     </script>
     ```
2. 缓存后如何获取数据

* **beforeRouteEnter**
  每次组件渲染的时候都会执行beforeRouterEnter

  ```javascript
  beforeRouteEnter(to, from, next) {
    next(vm => {
      vm.getData()
    })
  }
  ```
* **activated**
  在keep-alive缓存中激活时执行

  ```javascript
  activated() {
    this.getData()
  }
  ```

## 12. nextTick()有什么用？

等待下一次 DOM 更新刷新的工具方法。

* 当你在 Vue 中更改响应式状态时，最终的 DOM 更新并不是同步生效的，而是由 Vue 将它们缓存在一个队列中，直到下一个“tick”才一起执行。这样是为了确保每个组件无论发生多少状态改变，都仅执行一次更新。
* nextTick() 可以在状态改变后立即使用，以等待 DOM 更新完成。你可以传递一个回调函数作为参数，或者 await 返回的 Promise。

## 13. vue3 组件通信方案

1. **Props （父传子）**

```html
  // Parent.vue
  <template>
    <!-- 使用子组件 -->
    <Child :msg="message" />
  </template>

  // Child.vue
  <script setup>
    const props = defineProps({
      msg: {
        type: String,
        default: ''
      }
    })
    console.log(props.msg) 
  </script>

  <template>
    <div>
      {{ msg }}
    </div>
  </template>
```

2. **emits (子组件通知父组件触发一个事件，并且可以传值给父组件)**

```html
  // Parent.vue
  <template>
    <div>父组件：{{ message }}</div>
    <!-- 自定义 changeMsg 事件 -->
    <Child @changeMsg="changeMessage" />
  </template>

  // Child.vue
  <script setup>

  // 注册一个自定义事件名，向上传递时告诉父组件要触发的事件。
  const emit = defineEmits(['changeMsg'])

  function handleClick() {
    // 参数1：事件名
    // 参数2：传给父组件的值
    emit('changeMsg', '扶苏')
  }
  </script>  
  <template>
    <div>
      子组件：<button @click="handleClick">子组件的按钮</button>
    </div>
  </template>
```

3. **ref 与 $parent**

* ref 可以获取元素的 DOM 或者获取子组件实例的 VC。既然可以在父组件内部通过 ref 获取子组件实例 VC，那么子组件内部的方法与响应式数据父组件也是可以使用的。
* $parent 可以获取某一个组件的父组件实例 VC，因此可以使用父组件内部的数据与方法。必须子组件内部拥有一个按钮点击时候获取父组件实例，当然父组件的数据与方法需要通过 defineExpose 方法对外暴露。

```html
  // Parent.vue
  <script setup>
  let money = ref(10000);
  let children1 = ref(null);
  let children2 = ref(null);
  
  const handler = () => {
    money.value += 100;
    children1.value.money -= 100;
  };
  </script>
  <template>
    <h1>我是父组件:ref parent</h1>
    <h2>父组件拥有财产:{{ money }}</h2>
    <button @click="handler">向子组件1拿100元</button>
    <Children1 ref="children1"></Children1>
    <Children2 ref="children2"></Children2>
  </template>

  // Child1.vue
  <template>
    <div class="children1">
      <h2>我是子组件1</h2>
      <h3>子组件1拥有财产:{{ money }}</h3>
    </div>
  </template>

  <script setup>
  let money = ref(9999);
  defineExpose({
    money,
  });
  </script>

  // Child2.vue
  <template>
    <div class="children2">
      <h2>我是子组件2</h2>
      <h3>子组件2拥有财产:{{ money }}</h3>
      <button @click="handler($parent)">向父组件拿300元</button>
    </div>
  </template>

  <script setup>
  let money = ref(9999);
  const handler = ($parent) => {
    money.value += 300;
    console.log($parent);
    $parent.money -= 300;
  };
  </script>
```

4. **useAttrs**
5. **v-model**
   组件上的 v-model 使用 modelValue 作为 prop 和 update:modelValue 作为事件。

```html
  // Parent.vue
  <script setup>
  import { ref } from 'vue'
  import Child from './components/Child.vue'
  const message1 = ref('1')
  const message2 = ref('2')
  </script>
  <template>
  <Child v-model:msg1="message1" v-model:msg2="message2" />
  </template>

  // Child.vue
  <script setup>
  import { ref } from 'vue'
  // 接收
  const props = defineProps({
    msg1: String,
    msg2: String
  })

  const emit = defineEmits(['update:msg1', 'update:msg2'])
  function changeMsg1() {
    emit('update:msg1', '鲨鱼辣椒')
  }
  function changeMsg2() {
    emit('update:msg2', '蝎子莱莱')
  }
  </script>  

  <template>
    <div><button @click="changeMsg1">修改msg1</button> {{msg1}}</div>
    <div><button @click="changeMsg2">修改msg2</button> {{msg2}}</div>
  </template>
```

6. **作用域插槽 slot**

```html
  // Parent.vue
  <MyComponent>
    <template #header="headerProps">
      {{ headerProps }}
    </template>
    <template #default="defaultProps">
      {{ defaultProps }}
    </template>
    <template #footer="footerProps">
      {{ footerProps }}
    </template>
  </MyComponent>

  // Child.vue
  <slot name="header" message="hello"></slot>
  //headerProps 的结果是 { message: 'hello' }。
```

7. **provide / inject**

* 遇到多层传值时，使用 props 和 emit 的方式会显得比较笨拙。这时就可以用 provide 和 inject 了。
* provide 是在父组件里使用的，可以往下传值。
* inject 是在子(后代)组件里使用的，可以网上取值。
* 无论组件层次结构有多深，父组件都可以作为其所有子组件的依赖提供者。

  ```html
  // Parent.vue
  <script setup>
  import { ref, provide, readonly } from 'vue'
  import Child from './components/Child.vue'

  const name = ref('1')
  const msg = ref('2')

  // 使用readonly可以让子组件无法直接修改，需要调用provide往下传的方法来修改
  provide('name', readonly(name))
  provide('msg', msg)
  provide('changeName', (value) => {
    name.value = value
  })
  </script>
  <template>
    <Child></Child>
  </template>

  // Child.vue
  <template>
    <div>
      <div>msg: {{ msg }}</div>
      <div>name: {{name}}</div>
      <button @click="handleClick">修改</button>
    </div>
  </template>

  <script setup>
  import { inject } from 'vue'
  const name = inject('name', 'hello') // 参数2为默认值
  const msg = inject('msg')
  const changeName = inject('changeName')

  function handleClick() {
    changeName('3')
    // 因为 msg 没被 readonly 过，所以可以直接修改值
    msg.value = '4'
  }
  </script>
  ```

8. **getCurrentInstance**
9. **mitt.js**
10. **Pinia**

## 14. v-model原理

v-model是一个指令用于双向绑定。
**原理**：

* 在模板中用v-model绑定一个变量到表单元素上
* vue在解析模板时，会将v-model指令转换为合适的属性和事件绑定。对于大多数表单元素，他会将value属性于输入框的当前值进行绑定，并监听inbput事件来实时更新绑定的数据。
* 当用户在输入框中键入或选择内容时，会触发Input事件。Vue会捕获该事件并更新绑定的数据，以及根据数据的变化重新渲染视图。
* 同样，如果在表单元素上使用v-model的lazy修饰符，vue会监听change事件，而不是input事件。
  总的来说，原理就是通过监听表单元素的输入事件，将用户的输入同步到vue实例中的属性，并在属性值变化时重新渲染视图。

## 15. 自定义指令是什么？有哪些应用场景？怎么写？

自定义指令是一种用于扩展Vue的模板语法的机制，通过自定义指令，你可以在DOM元素上添加自定义行为，并在元素插入、更新和移除时进行相应操作。

应用场景：

1. 操作DOM元素，比如添加样式、事件监听等。可以在指令的钩子函数中访问和操作DOM元素。
2. 表单验证：通过自定义指令，你可以监听输入框的值变化，并根据自定义的验证规则进行验证，以提供实时的反馈。
3. 权限控制：通过自定义指令，你可以根据当前用户的权限来控制DOM元素的显示和隐藏。
4. 第三方库集成：通过自定义指令，你可以集成第三方库，集成highlight.js来实现v-highlight指令高亮代码pre code。

用法：

* 全局指令

  ```javascript
  // main.js
  import { createApp } from 'vue'
  import App from './App.vue'

  const app = createApp(App)

  // 🌍 全局注册 v-focus 指令
  app.directive('focus', {
    mounted(el) {
      el.focus()
    }
  })

  app.mount('#app')
  ```
* 局部指令

  ```html
  <script setup>
  import { ref, onMounted } from 'vue'

  // 🔧 定义一个局部自定义指令：v-focus
  const vFocus = {
    mounted(el) {
      el.focus() // 自动聚焦
    }
  }

  const inputRef = ref('')
  </script>

  <template>
    <input v-focus v-model="inputRef" placeholder="我会自动获得焦点" />
  </template>
  ```
* 指令钩子函数

  ```javascript
  const myDirective = {
    // 在绑定元素的 attribute 前
    // 或事件监听器应用前调用
    created(el, binding, vnode) {
      // 下面会介绍各个参数的细节
    },
    // 在元素被插入到 DOM 前调用
    beforeMount(el, binding, vnode) {},
    // 在绑定元素的父组件
    // 及他自己的所有子节点都挂载完成后调用
    mounted(el, binding, vnode) {},
    // 绑定元素的父组件更新前调用
    beforeUpdate(el, binding, vnode, prevVnode) {},
    // 在绑定元素的父组件
    // 及他自己的所有子节点都更新后调用
    updated(el, binding, vnode, prevVnode) {},
    // 绑定元素的父组件卸载前调用
    beforeUnmount(el, binding, vnode) {},
    // 绑定元素的父组件卸载后调用
    unmounted(el, binding, vnode) {}
  }
  ```
* 钩子参数

  * el：指令绑定到的DOM元素，可以用于直接操作当前元素，默认传入钩子的就是el参数，例如我们开始实现的focus指令，就是直接操作的元素DOM
  * binding：这是一个对象，包含以下属性：
    * value：在元素上使用指令时，传递给指令的值。例如：`<div v-reverse="'hello'"></div>`，传递给reserve指令的值就是hello，我们可以拿到值并做相关处理
    * oldValue：之前的值，一般用于beforeUpate和updated钩子函数中，例如：beforeUpdate(el, {oldValue: ''})
    * arg：传递给指令的参数，非必需，例如 `<div v-reverse:foo="'hello'"></div>`，那么传递给指令的参数就是foo
    * modifiers：一个由修饰符构成的对象，例如 `<div v-reverse.foo.bar="'hello'"></div>`，那么该修饰符对象为{foo: true, bar: true}，我们经常使用到的事件修饰符，其实和这个差不多。
    * instance：使用该指令的组件实例，注意不是DOM
    * dir：指令的定义对象
  * vnode：绑定元素的地城VNode
  * preVnode：之前的渲染中代表指令所绑定的元素的VNode，一般用于beforeUpate和updated钩子函数中

## 16. Vue 中 $router 和 $route 的区别？

1. $router：Vue Router 提供的实例对象，用于进行路由操作。动态改变URL，从而实现页面间无刷新跳转。
2. $route：当前路由信息的对象，包括当前URL路径、查询参数、路径参数等。只读不可修改。

## 17. Vue中v-if和v-for为什么不建议一起用？

* 在Vue 2 中，v-for 的优先级高于 v-if，当二者同时作用于一个元素时，会先执行 v-for 遍历所有元素，然后在每个元素上都执行 v-if 判断，导致性能浪费，尤其是在数据量大时会影响性能，因此Vue 官方不推荐这样做。
* 在Vue 3 中，v-if 优先级高于 v-for，但仍然建议避免同时使用，可以通过将 v-if 移到外层 template 标签上或使用计算属性提前过滤数据来优化。

## 18. Vue 中hash模式和history模式的区别？

* **hash模式**：
  * 工作原理：
    * 利用 URL 中的 # 后面的部分（即 hash 值）来模拟路由变化。
    * 浏览器不会将 hash 部分发送给服务器，所以无论 # 后是什么，服务器都返回同一个 HTML 文件（通常是 index.html）。
    * Vue Router 监听 hashchange 事件，根据 hash 值渲染对应组件。
  * 优点：
    * 无需服务器配置：任何静态服务器都能运行。
    * 兼容性好：支持老浏览器（IE8+）。
    * 刷新不报错：即使路径不存在，也不会 404。
  * 缺点：
    * URL 不美观，带有 #。
    * 对 SEO 不够友好（虽然 Google 能索引，但不如 history 模式）。
    * `#` 后的内容不会发到服务器，限制了某些场景使用。
* **history 模式**：
  * 工作原理：
    * 利用 URL 中的 # 后面的部分（即 hash 值）来模拟路由变化。
    * 浏览器不会将 hash 部分发送给服务器，所以无论 # 后是什么，服务器都返回同一个 HTML 文件（通常是 index.html）。
    * Vue Router 监听 hashchange 事件，根据 hash 值渲染对应组件。
  * 优点：
    * 无需服务器配置：任何静态服务器都能运行。
    * 兼容性好：支持老浏览器（IE8+）。
    * 刷新不报错：即使路径不存在，也不会 404。
  * 缺点：
    * URL 不美观，带有 #。
    * 对 SEO 不够友好（虽然 Google 能索引，但不如 history 模式）。
    * `#` 后的内容不会发到服务器，限制了某些场景使用。

## 19. KeepAlive 是什么？

Vue内置组件，可以使被包含的组件保留状态或避免重新渲染，也就是租价缓存。能在组件切换过程中将状态保留在内存中，防止重复渲染DOM。
简而言之：它的功能是在多个组件间动态切换时缓存被移除的组件实例。

```html
<!-- 非活跃的组件将会被缓存！ -->
<KeepAlive>
  <component :is="activeComponent" />
</KeepAlive>
```

当使用keep-alive组件后，组件就会自动加上两个钩子：

* activated：组件被激活时调用,可以理解为进入这个页面时候触发。
* deactivated：组件被移除时调用,可以理解为离开这个页面时候触发。

