---
title: Migration from Vue 1.x
type: guide
order: 24
---

## FAQ

> 哇 - 这个页面真是太长了！这是否意味着2.0和之前的版本完全不同，我需要从基础从新学起，而低版本的迁移也会变得很困难吗？

我很高兴你提了这个问题！答案是不。大约90%的API都和过去一样，而且核心概念都未曾变动。这页面之所以长，是因为我们提供了非常详细的解释，包括很多实例。放心，__你无需从头至尾地看一遍！__

> 迁移的步骤是？

1. 首先，在你的项目中运行[migration helper](https://github.com/vuejs/vue-migration-helper)。这个命令行界面工具包含了一个Vue的精简压缩的开发版本。当它检测到一个被废弃的模式时，会提醒你，给出建议，并会提供查看更多信息的一些链接。

2. 之后，在侧边栏中浏览一下这个页面的大致内容。如果发现某个地方的改动可能会影响到你，但migration helper并没有提醒你，那么请指出。

3. 如果你之前的项目有测试，运行看看有哪些还是测试未通过的。如果没有测试，就在浏览器中打开应用，关注有哪些warning和error.

4. 至此，你的应用迁移就完成了。如果还想了解更多，可以读这篇剩下的部分 - 或者就从[the beginning](index.html)开始看吧，这是一个改进版的入门指引。很多内容都可以快速浏览，因为你已经对核心概念比较熟悉了。

> 从Vue 1.x应用迁移到2.0需要多长时间？

取决于两个因素：

- 应用的规模（小到中型的应用应该在一天之内可以完成）

- 你有多少次在玩一个很酷的新特性时抓狂过。不单是你，我们在开发2.0时也这样！

- 你使用了哪些被废弃的特性。大多数都可以通过「查找-替换」升级完成，但是小部分需要一些时间。如果你没有使用推荐的最佳实践，Vue 2.0也会强制让你按这么去做。长远来看这是件好事，但是也可能意味着这是一个标志性（可能有点过了）的重构。

> 如果升级到了Vue 2, 也需要同时升级Vuex和Vue-Router吗？

只有Vue-Router 2和Vue 2是兼容的，所以你也得按[Vue-Router升级路线](migration-vue-router.html) 中的内容来操作。幸运的是，大多数应用在router这块都没有太多代码，所以这部分应该不到一小时就可以搞定。

Vuex方面，甚至0.8版本都可以和Vue 2兼容，所以升级并非强制性的。唯一可能的升级理由是享受Vuex 2带来的好处和新特性，如模块化和更精简的样板。

## 模板

### Fragment实例 <sup>已废弃</sup>

每个组件现在都需要有一个明确的根元素。碎片(Fragments)实例已经不再支持。如果你有个template像这样：

``` html
<p>foo</p>
<p>bar</p>
```

推荐把所有内容包在一个新元素内，如下：

``` html
<div>
  <p>foo</p>
  <p>bar</p>
</div>
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>运行你的端到端测试套件或者应用，找关于「multiple root elements in a template」的console warnings。</p>
</div>
{% endraw %}

## 生命周期钩子

### `beforeCompile` <sup>已废弃</sup>

使用`created`来替代。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到所有这个钩子。</p>
</div>
{% endraw %}

### `compiled` <sup>已废弃</sup>

使用新的钩子`mounted`来替代。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到所有这个钩子。</p>
</div>
{% endraw %}

### `attached` <sup>已废弃</sup>

在其他钩子中进行「in-dom check」来替代。举例，替换如下的内容：

``` js
attached: function () {
  doSomething()
}
```

你可以使用：

``` js
mounted: function () {
  this.$nextTick(function () {
    doSomething()
  })
}
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到所有这个钩子。</p>
</div>
{% endraw %}

### `detached` <sup>已废弃</sup>

在其他钩子中进行「in-dom check」来替代。举例，替换如下的内容：

``` js
detached: function () {
  doSomething()
}
```

你可以使用：

``` js
destroyed: function () {
  this.$nextTick(function () {
    doSomething()
  })
}
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到所有这个钩子。</p>
</div>
{% endraw %}

### `init` <sup>已废弃</sup>

使用新的钩子 `beforeCreate` 来替代。这实质上是同一个东西，只是换了名称，是为了和其他生命周期方法保持一致。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到所有这个钩子。</p>
</div>
{% endraw %}

### `ready` <sup>已废弃</sup>

用新的钩子 `mounted` 来替代。 需要注意的是，在`mounted`时，不能保证已经「document ready」。因此需要加入`Vue.nextTick`/`vm.$nextTick`。举例：

``` js
mounted: function () {
  this.$nextTick(function () {
    // code that assumes this.$el is in-document
  })
}
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到所有这个钩子。</p>
</div>
{% endraw %}

## `v-for`

### `v-for` 使用数组时的参数顺序

当在`v-for`中使用数组`index`时，以往的参数顺序是`(index, value)`。现在是`(value, index)`，保持和Javascript原生数组方法的一致性，如`forEach` 和 `map`。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到deprecated argument order。注意，如果你把参数名改成类似<code>position</code> 或 <code>num</code>这样的名字，那么helper将不会提醒。</p>
</div>
{% endraw %}

### `v-for` 使用Object时的参数顺序

当在`v-for`中使用Object的`key`时，以往的参数顺序是`(key, value)`。现在是`(value, key)`，保持和大多数库中object迭代器的一致性，如`lodash`。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到deprecated argument order。注意，如果你把参数名key改成类似<code>name</code> 或 <code>property</code>这样的名字，那么helper将不会提醒。</p>
</div>
{% endraw %}

### `$index` and `$key` <sup>已废弃</sup>

隐式声明的两个变量 `$index` 和 `$key` 已经被废弃，因为在 `v-for` 里已经显式声明了它们。这可以让不熟悉Vue的开发者更加易懂，在做嵌套循环的时候也更加清晰。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到这些已废弃的变量。如果没有处理完，会看到一个像这样的<strong>console errors</strong>：<code>Uncaught ReferenceError: $index is not defined</code></p>
</div>
{% endraw %}

### `track-by` <sup>已废弃</sup>

`track-by` 现在已经被 `key` 替代，工作方式和其他属性的方式一样： 如果没有 `v-bind:` 或 `:` 前缀，就当作一个字符串来处理。大多数情况下，你可能需要一个带完整表达式的动态绑定，而不是一个key名。替换举例：

``` html
<div v-for="item in items" track-by="id">
```

现在这么写：

``` html
<div v-for="item in items" v-bind:key="item.id">
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到 <code>track-by</code> 的例子。</p>
</div>
{% endraw %}

### `v-for` 的值范围

之前，`v-for="number in 10"` 会使 `number` 的值范围从0至9。 现在的值范围是1至10。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下找这个正则匹配  <code>/\w+ in \d+/</code> 。如果找到的结果在一个 <code>v-for</code>中，观察是否受到了影响。</p>
</div>
{% endraw %}

## 属性

### `coerce` Prop Option <sup>已废弃</sup>

如果需要对一个属性值做转换（在设置值之前转换值），用一个computed的值来代替它。举例，如下的写法

``` js
props: {
  username: {
    type: String,
    coerce: function (value) {
      return value
        .toLowerCase()
        .replace(/\s+/, '-')
    }
  }
}
```

可以写成：

``` js
props: {
  username: String,
},
computed: {
  normalizedUsername: function () {
    return this.username
      .toLowerCase()
      .replace(/\s+/, '-')
  }
}
```

这样有几个好处：

- 仍然可以获取prop的原始值。
- 强制要求给转换后的值取一个与value值不一样的属性名称。可以让代码的目的性更加明确。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到 <code>coerce</code> option。</p>
</div>
{% endraw %}

### `twoWay` Prop Option <sup>已废弃</sup>

Props现在始终是单向流动的。要使组件的变动影响它的parent scope, 就需要在它自身emit事件来触发，而不是依赖于一个隐式的绑定。看以下这些链接了解更多：

- [自定义组件事件](components.html#Custom-Events)
- [自定义input输入框事件](components.html#Form-Input-Components-using-Custom-Events) (using component events)
- [全局状态管理](state-management.html)

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到 <code>twoWay</code> option。</p>
</div>
{% endraw %}

### `v-bind` with `.once` and `.sync` Modifiers <sup>deprecated</sup>

Props现在始终是单向流动的。要使组件的变动影响它的parent scope, 就需要在它自身emit事件来触发，而不是依赖于一个隐式的绑定。看以下这些链接了解更多：

- [自定义组件事件](components.html#Custom-Events)
- [自定义input输入框事件](components.html#Form-Input-Components-using-Custom-Events) (using component events)
- [全局状态管理](state-management.html)

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到 <code>.once</code> 和 <code>.sync</code> 这些modifiers。</p>
</div>
{% endraw %}

### Prop Mutation <sup>已废弃</sup>

在本地改变一个prop值 (mutation) 现在被认为是一个「反模式」，举例：声明一个prop, 然后在component中设置`this.myProp = 'someOtherValue'` 。在新的渲染机制下，当父component重新渲染时，子component的本地修改会被覆盖。

大多数prop值的改变可以替换为以下形式：

- data property, prop用于设置其默认值
- computed property

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到关于prop mutations的<strong>console warnings</strong></p>
</div>
{% endraw %}

### Props on a Root Instance <sup>已废弃</sup>

Vue根实例（Root Instance）上的 `props` 现在需要用 `propsData` 来替换。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>运行你的端到端测试（如果有的话）。<strong>failed tests</strong> 会提醒你在根实例上传入的props已经无法工作了。</p>
</div>
{% endraw %}

## 内置指令

### `v-bind`的真假值

在使用 `v-bind` 时，当前版本的假值有这些： `null`, `undefined` 和 `false` 。也就是说 `0` 和空字符串现在会被处理成真值。举个例子，`v-bind:draggable="''"` 现在会被渲染为 `draggable="true"`。

对于枚举属性，除了上述提到的，`"false"` 这个字符串也会被渲染为  `attr="false"` 。

<p class="tip">注意，对于其他的指令 (如 `v-if` 和 `v-show`, 上述写法还是会被处理成真值。)</p>

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>运行你的端到端测试（如果有的话）。<strong>failed tests</strong> 会提醒你的应用由于这个改动带来的影响。</p>
</div>
{% endraw %}

### 在组件上使用 `v-on` 来监听原生事件

当使用组件时， `v-on` 现在只对自身 `$emit` 的自定义事件起作用。 如果需要在一个根元素上监听原生DOM事件，可以使用 `.native` 这个修饰器。举例：

``` html
<my-component v-on:click.native="doSomething"></my-component>
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>运行你的端到端测试（如果有的话）。<strong>failed tests</strong> 会提醒你的应用由于这个改动带来的影响。</p>
</div>
{% endraw %}

### `v-model` 和 `debounce` 结合使用 <sup>已废弃</sup>

Debouncing现在被用于限制发送ajax请求，或其他费时操作的频率。 Vue中使用 `v-model` 的 `debounce` 属性适用于一些非常简单的例子， 但是实际上在运行中__状态变更__ 也延迟执行了，这个时间甚至会比那此费时操作更长。这个差别很小，但是随着应用规模的增长，会带来一定的限制。

这些限制在设计一个实时搜索框 (search indicator) 时会变得很明显，举例：

{% raw %}
<script src="https://cdn.jsdelivr.net/lodash/4.13.1/lodash.js"></script>
<div id="debounce-search-demo" class="demo">
  <input v-model="searchQuery" placeholder="Type something">
  <strong>{{ searchIndicator }}</strong>
</div>
<script>
new Vue({
  el: '#debounce-search-demo',
  data: {
    searchQuery: '',
    searchQueryIsDirty: false,
    isCalculating: false
  },
  computed: {
    searchIndicator: function () {
      if (this.isCalculating) {
        return '⟳ Fetching new results'
      } else if (this.searchQueryIsDirty) {
        return '... Typing'
      } else {
        return '✓ Done'
      }
    }
  },
  watch: {
    searchQuery: function () {
      this.searchQueryIsDirty = true
      this.expensiveOperation()
    }
  },
  methods: {
    expensiveOperation: _.debounce(function () {
      this.isCalculating = true
      setTimeout(function () {
        this.isCalculating = false
        this.searchQueryIsDirty = false
      }.bind(this), 1000)
    }, 500)
  }
})
</script>
{% endraw %}

如果使用 `debounce` 这个属性，就没有办法去检测 "Typing" 这个状态，因为这个input的实时状态我们无法
得知。为了让Vue和这个debounce function解耦，我们可以只对需要的操作做限制，移除这个在特性上的限制：

``` html
<!--
通过使用lodash或其他专门 的工具库中的debounce函数，下面这个使用debounce实践是最佳的 - 同时，我们可以在任何地方使用，而不仅是在模板中。
-->
<script src="https://cdn.jsdelivr.net/lodash/4.13.1/lodash.js"></script>
<div id="debounce-search-demo">
  <input v-model="searchQuery" placeholder="Type something">
  <strong>{{ searchIndicator }}</strong>
</div>
```

``` js
new Vue({
  el: '#debounce-search-demo',
  data: {
    searchQuery: '',
    searchQueryIsDirty: false,
    isCalculating: false
  },
  computed: {
    searchIndicator: function () {
      if (this.isCalculating) {
        return '⟳ Fetching new results'
      } else if (this.searchQueryIsDirty) {
        return '... Typing'
      } else {
        return '✓ Done'
      }
    }
  },
  watch: {
    searchQuery: function () {
      this.searchQueryIsDirty = true
      this.expensiveOperation()
    }
  },
  methods: {
    // 这里是debounce实际生效的地方
    expensiveOperation: _.debounce(function () {
      this.isCalculating = true
      setTimeout(function () {
        this.isCalculating = false
        this.searchQueryIsDirty = false
      }.bind(this), 1000)
    }, 500)
  }
})
```

另外一个好处是，当debounce函数不是包裹函数时，还会耗费时间。举例，当命中搜索建议时，用户已经停止输入了还需要等待一段时间才能得到搜索结果的体验不太理想。这时可能需要一个 __throttling__ 函数来代替它。如果你已经使用了类似lodash这样的工具库，换成使用 `throttle` 函数，很快就能搞定。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到 <code>debounce</code> 的例子。</p>
</div>
{% endraw %}

### `v-model` 和 `lazy` 或 `number` 这样的参数特性结合使用 <sup>已废弃</sup>

The `lazy` and `number` param attributes are now modifiers, to make it more clear what That means instead of:

`lazy` 和 `number` 这些参数特性现在是修饰器，以使代码更加清晰，也就是说替换以下例子：

``` html
<input v-model="name" lazy>
<input v-model="age" type="number" number>
```

你可以这么写：

``` html
<input v-model.lazy="name">
<input v-model.number="age" type="number">
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到这些被废弃的参数特性的例子。</p>
</div>
{% endraw %}

### `v-model` 和行内 `value` 结合使用 <sup>已废弃</sup>

`v-model` no longer cares about the initial value of an inline `value` attribute. For predictability, it will instead always treat the Vue instance data as the source of truth.

`v-model` 现在不再关心行内 `value` 属性的初始值。现在仅会根据Vue实例中的数据来处理。

也就是说这个元素：

``` html
<input v-model="text" value="foo">
```

在data中的数据是这样的：

``` js
data: {
  text: 'bar'
}
```

会渲染成"bar"而不是"foo"。对于 `<textarea>` 也会做同样处理。如下：

``` html
<textarea v-model="text">
  hello world
</textarea>
```

需要保证你的 `text` 初始化值是"hello world"。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>升级后运行你的端到端测试（如果有的话）。找到<strong>console warnings</strong> 中关于 inline value attributes with <code>v-model</code>的部分。</p>
</div>
{% endraw %}

### 使用`v-model` 和 `v-for` 进行原始值迭代 <sup>已废弃</sup>

这样的例子将不会工作：

``` html
<input v-for="str in strings" v-model="str">
```

因为这样的话，相当于 `<input>` 会被编译成如下的Javascript:

``` js
strings.map(function (str) {
  return createElement('input', ...)
})
```

如你所见，`v-model` 的双向绑定在这里行不通。在迭代函数中设置 `str`为其他的值将没有作用，因为它在函数作用域中只是一个本地变量。 

你可以使用一个 __objects__ 数组来替代，以使  `v-model` 可以在这个对象上进行更新。举例：

``` html
<input v-for="obj in objects" v-model="obj.str">
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>运行你的测试（如果有的话）。<strong>failed tests</strong> 会提醒你的应用由于这个改动带来的影响。</p>
</div>
{% endraw %}

### 在`v-bind:style`表达式中同时使用Object声明和 `!important` <sup>已废弃</sup>

以下例子将不再工作：

``` html
<p v-bind:style="{ color: myColor + ' !important' }">hello</p>
```

如果你确实需要覆盖另外一个 `!important`，需要使用字符串拼接：

``` html
<p v-bind:style="'color: ' + myColor + ' !important'">hello</p>
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有<code>!important</code>的style绑定的例子。</p>
</div>
{% endraw %}

### `v-el` 和 `v-ref` <sup>已废弃</sup>

简单说，`v-el` 和 `v-ref` 已经被合并为 `ref`属性，同时在组件实例中可以通过 `$refs` 来访问它。也就是说 `v-el:my-element` 会变成 `ref="myElement"` ，`v-ref:my-component` 会变成 `ref="myComponent"` 。当在一个普通html元素上使用时， `ref`会是这个DOM元素，当在一个组件上使用时，`ref`将会是这个组件的实例。

虽然 `v-ref` 已经不再是一个内置指令而是个特殊属性，同样可以动态声明它。当在和 `v-for` 一起使用时会变得更实用。举例。

``` html
<p v-for="item in items" v-bind:ref="'item' + item.id"></p>
```

在之前版本中，`v-el`/`v-ref` 和 `v-for` 一起使用时，会产生一个 html元素/组件的数组，因为没有别的方式可以给每个item定义唯一的名称。同时你也可以通过给每个item同样的`ref`来实现这个功能：

``` html
<p v-for="item in items" ref="items"></p>
```

不像1.x版本， 这些 `$refs` 不是响应式的，因为在渲染过程中已经注册或更新过它们了。让它们变为响应式需要对每个改动做重复的渲染工作。

另一方面， `$refs` 主要是被设计成在Javascript程序中访问的 - 所以并不推荐在模板中去依赖它们，因为指向的是不属于该实例本身的状态。这会违反Vue的数据驱动视图模型。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有 <code>v-el</code> 和 <code>v-ref</code>的例子。</p>
</div>
{% endraw %}

### `v-else` 和 `v-show` 结合使用 <sup>已废弃</sup>

`v-else` 和 `v-show` 将不能在一起结合使用。 使用 `v-if`和一个否定表达式来替代。举例，要替换：

``` html
<p v-if="foo">Foo</p>
<p v-else v-show="bar">Not foo, but bar</p>
```

可以写成：

``` html
<p v-if="foo">Foo</p>
<p v-if="!foo && bar">Not foo, but bar</p>
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有 <code>v-else</code> 和 <code>v-show</code>结合使用的例子。</p>
</div>
{% endraw %}

## 自定义指令

指令（Directives）的职责范围被大大缩小了：它们现在只能用于执行一些低级的直接DOM操作。大多数情况下你更应该去使用组件(components)来进行代码复用。

一些最需要说明的不同点包括：

- Directives将不再会有实例。也就是说在directive的钩子函数中将不存在`this`。在指令中所有需要的东西都将使用参数传入。如果你需要在钩子中做状态持久化，可以在`el`中处理。
- 像 `acceptStatement`, `deep`, `priority` 等选项已经全部被废弃。替代双向绑定的directives，可见[这个例子](#Two-Way-Filters-deprecated)。
- 现有的directive钩子将会有不同的行为，同时会有一些新钩子。

幸运的是，新的directives变得简单多了，掌握起来也更为简单。可以看新的[自定义指令指南](custom-directive.html) 了解更多。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到定义directive的例子。它们会被helper全部标记，在大多数情况下你需要将它重构为一个component。</p>
</div>
{% endraw %}

### 指令的 `.literal` 修饰器 <sup>已废弃</sup>

`.literal`这个修饰器已经被移除，因为可以直接通过提供一个字符串字面量来实现。

举例：

``` js
<p v-my-directive.literal="foo bar baz"></p>
```

可以直接改为：

``` html
<p v-my-directive="'foo bar baz'"></p>
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有在directive上定义 `.literal`修饰器的例子。</p>
</div>
{% endraw %}

## 过渡（Transitions）

### `transition` 属性 <sup>已废弃</sup>

Vue的过渡系统已经进行了大幅度改动，现在需要使用`<transition>` 和 `<transition-group>` 元素进行包裹，而不是使用`transition` 属性。推荐阅读新的[过渡指南](transitions.html) 来了解更多。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有<code>transition</code>属性的例子。</p>
</div>
{% endraw %}

### 使用`Vue.transition` 创建复用的过渡 <sup>已废弃</sup>

在新的过渡系统下，你可以直接 [使用组件实现复用过渡](http://rc.vuejs.org/guide/transitions.html#Reusable-Transitions)。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有<code>Vue.transition</code>属性的例子。</p>
</div>
{% endraw %}

### 过渡的 `stagger` 属性 <sup>已废弃</sup>

如果你要为一个列表创建一个渐近过渡，可以通过设置和访问 `data-index` （或类似的属性）来控制过渡时间。看[这个例子](transitions.html#Staggering-List-Transitions)。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有 <code>transition</code>属性的例子。在升级过程中，你也可以“过渡”到新的渐近过渡策略上来。</p>
</div>
{% endraw %}

## 事件

### `events` 选项 <sup>已废弃</sup>

`events`选项已经被废弃。在`created`钩子中会注册事件处理器。请查看[`$dispatch` 和 `$broadcast` 迁移指南](#dispatch-and-broadcast-deprecated)这个详细示例。

### `Vue.directive('on').keyCodes` <sup>已废弃</sup>

新的更简洁的方式来设置 `keyCodes` 是通过`Vue.config.keyCodes`。举例：

``` js
// 启用v-on:keyup.f1
Vue.config.keyCodes.f1 = 112
```
{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有使用老的<code>keyCode</code>方式进行设置的例子。</p>
</div>
{% endraw %}

### `$dispatch` and `$broadcast` <sup>已废弃</sup>

`$dispatch` 和 `$broadcast` 被废弃的原因是：现在有更加清晰的组件间通信和可维护性更好状态管理方案，如 [Vuex](https://github.com/vuejs/vuex)。

问题在于，组件树变得越来越大时，基于组件树结构的事件流将很难跟踪和维护，可扩展性也不好，我们不想让你后续再痛苦下去。`$dispatch` 和 `$broadcast` 也不能很好地解决兄弟组件的通信问题。

最常使用这些方法的是在实现父组件和下一级子组件通信的时候。在这些场景下，你实际上可以可以[使用 `v-on` 监听来自子组件的 `$emit`](http://vuejs.org/guide/components.html#Form-Input-Components-using-Custom-Events)。

但是，当在和跨多层级的子孙组件/祖先组件进行通信时，`$emit` 也起不了作用。最简单的可能升级方式是使用一个「中央事件管理枢纽」。这样做的好处是：你可以在组件树中 -- 甚至是在兄弟组件间通信！因为Vue的实例实现了一个事件发射器，你可以用一个空的Vue实例来实现它。

举例，比如说我们有一个todo的应用，像是如下的结构：

```
Todos
|-- NewTodoInput
|-- Todo
    |-- DeleteTodoButton
```

我们可以在这个单事件枢纽中来管理组件间通信：

``` js
// 我们在每个组件中都要使用这个「事件枢纽」来进行它们之间的通信。
var eventHub = new Vue()
```

然后在组件中，我们可以使用 `$emit`, `$on`, `$off` 来派发事件，监听事件，清除事件监听器，分别是：

``` js
// 创建一个新Todo的input
// ...
methods: {
  addTodo: function () {
    eventHub.$emit('add-todo', { text: this.newTodoText })
    this.newTodoText = ''
  }
}
```

``` js
// 删除todo的按钮
// ...
methods: {
  deleteTodo: function (id) {
    eventHub.$emit('delete-todo', id)
  }
}
```

``` js
// ...
created: function () {
  eventHub.$on('add-todo', this.addTodo)
  eventHub.$on('delete-todo', this.deleteTodo)
},
// 在组件被销毁时清空事件监听器是很好的实践
beforeDestroy: function () {
  eventHub.$off('add-todo', this.addTodo)
  eventHub.$off('delete-todo', this.deleteTodo)
},
methods: {
  addTodo: function (newTodo) {
    this.todos.push(newTodo)
  },
  deleteTodo: function (todoId) {
    this.todos = this.todos.filter(function (todo) {
      return todo.id !== todoId
    })
  }
}
```

这种模式在简单场景中可以替代`$dispatch` 和 `$broadcast`；但在更复杂场景中，推荐使用专用的状态管理层，如[Vuex](https://github.com/vuejs/vuex)。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有 <code>$dispatch</code>和<code>$broadcast</code>的例子。</p>
</div>
{% endraw %}

## 过滤器

### 在文本插值外使用过滤器 <sup>已废弃</sup>

过滤器现在只能在文本插值 (`{% raw %}{{ }}{% endraw %}` 标记)之内使用。过去我们发现在内置指令如 `v-model` , `v-on` 等中使用filter时，复杂度要大于带来的便利。如在列表中使用`v-for` 并进行过滤，更好的方式是把这部分逻辑移动到Javascript中使用computed property来实现，这样在组件中也可以复用过滤后的数据。

In general, whenever something can be achieved in plain JavaScript, we want to avoid introducing a special syntax like filters to take care of the same concern. Here's how you can replace Vue's built-in directive filters:

总之，在简单的Javascript可以实现的功能，我们都避免去引入一个特殊的语法，如filter去实现同样简单的功能。Vue的内置指令filter的替换可以参考以下例子：

#### 替换`debounce`过滤器

替换如下例子：

``` html
<input v-on:keyup="doStuff | debounce 500">
```

``` js
methods: {
  doStuff: function () {
    // ...
  }
}
```

可以使用 [lodash的 `debounce`](https://lodash.com/docs/4.15.0#debounce) （或可能需要使用 [`throttle`](https://lodash.com/docs/4.15.0#throttle)）来直接限制费时操作的调用。可以像这样达成同样效果：

``` html
<input v-on:keyup="doStuff">
```

``` js
methods: {
  doStuff: _.debounce(function () {
    // ...
  }, 500)
}
```

这个策略带来的好处，看[`v-model`的这个例子](#v-model-with-debounce-deprecated)了解更多。

#### 替换 `limitBy` 过滤器

替换如下例子：

``` html
<p v-for="item in items | limitBy 10">{{ item }}</p>
```

可以在一个计算属性中使用Javascript原生的[`.slice` 方法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice#Examples)：

``` html
<p v-for="item in filteredItems">{{ item }}</p>
```

``` js
computed: {
  filteredItems: function () {
    return this.items.slice(0, 10)
  }
}
```

#### 替换 `filterBy` 过滤器

替换如下例子：

``` html
<p v-for="user in users | filterBy searchQuery in 'name'">{{ user.name }}</p>
```

可以在一个计算属性中使用Javascript原生的[`.filter` 方法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice#Examples)：

``` html
<p v-for="user in filteredUsers">{{ user.name }}</p>
```

``` js
computed: {
  filteredUsers: function () {
    return this.users.filter(function (user) {
      return user.name.indexOf(this.searchQuery)
    })
  }
}
```

JavaScript的原生 `.filter` 可以处理更加复杂的操作，因为在computed property中你可以使用完整的Javascript。举个例子，如果需要找出所有的 active users，同时name和email都是不区分大小写的：

``` js
this.users.filter(function (user) {
  var searchRegex = new RegExp(this.searchQuery, 'i')
  return user.isActive && (
    searchRegex.test(user.name) ||
    searchRegex.test(user.email)
  )
})
```

#### 替换 `orderBy` 过滤器

替换如下例子：

``` html
<p v-for="user in users | orderBy 'name'">{{ user.name }}</p>
```

在一个计算属性中使用[lodash的 `orderBy`](https://lodash.com/docs/4.15.0#orderBy) (或者可能使用 [`sortBy`](https://lodash.com/docs/4.15.0#sortBy))：

``` html
<p v-for="user in orderedUsers">{{ user.name }}</p>
```

``` js
computed: {
  orderedUsers: function () {
    return _.orderBy(this.users, 'name')
  }
}
```

你甚至可以进行多列排序：

``` js
_.orderBy(this.users, ['name', 'last_login'], ['asc', 'desc'])
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有在内置指令上使用filters的例子。如果没有被完全替换，会看到 <strong>console errors</strong>。</p>
</div>
{% endraw %}

### 过滤器参数语法

过滤器参数的语法现在和Javascript的函数调用相同。因此，如下这样空格分隔的参数：

``` html
<p>{{ date | formatDate 'YY-MM-DD' timeZone }}</p>
```

用括号包裹，逗号分隔：

``` html
<p>{{ date | formatDate('YY-MM-DD', timeZone) }}</p>
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有在内置指令上使用filters的例子。如果没有被完全替换，会看到 <strong>console errors</strong>。</p>
</div>
{% endraw %}

### 内置文本过滤器 <sup>已废弃</sup>

尽管仍允许在文本插值中使用过滤器，所有内置过滤器都已经被移除了。推荐使用其他专门处理这些领域问题的库 (如 [`date-fns`](https://date-fns.org/) 用于格式化日期 ， [`accounting`](http://openexchangerates.github.io/accounting.js/) 用于格式化货币)。

对于Vue内置过滤器, 我们将在下面例子中说明如何替代他们。这些示例代码可以被放在自定义的帮助函数，方法，或计算属性中。

#### 替换 `json` 过滤器

实际上并不需要任何的调试，因为Vue为自动格式化输出，不管是字符串，数字，数组或简单对象。如果你需要和Javascript的 `JSON.stringify` 方法实现同样的功能，可以将它放在方法或计算属性中。

#### 替换  `capitalize` 过滤器

``` js
text[0].toUpperCase() + text.slice(1)
```

#### 替换  `uppercase` 过滤器

``` js
text.toUpperCase()
```

#### 替换  `lowercase` 过滤器


``` js
text.toLowerCase()
```

#### 替换  `pluralize` 过滤器

[pluralize](https://www.npmjs.com/package/pluralize) 这个npm包可以很好实现这个功能，但是如果你只是需要一个特殊单词的复数形式，或特殊的输出格式如`0`, 那么可以定义你自己的复数函数。举例：

``` js
function pluralizeKnife (count) {
  if (count === 0) {
    return 'no knives'
  } else if (count === 1) {
    return '1 knife'
  } else {
    return count + 'knives'
  }
}
```

#### 替换  `currency` 过滤器

一个非常简单的实现，可以直接这么做：

``` js
'$' + price.toFixed(2)
```

但在很多场景中，你会遇到一些奇怪的行为 （如`0.035.toFixed(2)` 会四舍五入成 `0.4`, but `0.045` 会四舍五入成 `0.4`)。要处理这些问题，可以使用[`accounting`](http://openexchangerates.github.io/accounting.js/) 库来进行更可靠的货币格式化。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有使用这些已废弃的text filters的例子。如果没有被完全替换，会看到 <strong>console errors</strong>。</p>
</div>
{% endraw %}

### 双向过滤器 <sup>已废弃</sup>

一些用户乐于结合使用双向过滤器和 `v-model` ，使用很少的代码就能写出有趣的input输入框。这虽然__看起来__简单了，实际上双向过滤器隐藏了大量的复杂度 - 甚至状态的延迟更新带来了糟糕的用户体验。因此，在新版本中推荐使用组件包裹一个input，这样对于自定义输入框更加清晰，也可加入更多功能。

举例，我们现在来迁移一个货币双向过滤器：

<iframe width="100%" height="300" src="https://jsfiddle.net/chrisvfritz/6744xnjk/embedded/js,html,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

大多数情况下看起来不错，但是延迟状态更新会带来一些奇怪的行为。举个例子，切换到result这个tab, 在任意一个输入框中输入 `9.999` 。当input失去焦点时，它的值会更新至  `$10.00` 。但是你会发现计算的总数是在data中储存的 `9.999` 。 这说明用户看到的现实版本并未同步。

让我们使用Vue 2.0将其升级到一个可靠的解决方案。先将这个过滤器包裹到一个新的 `<currency-input>` 组件中：

<iframe width="100%" height="300" src="https://jsfiddle.net/chrisvfritz/943zfbsh/embedded/js,html,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

这样的做法允许我们加入一些过滤器自身无法封装进去的行为，如在一个input输入框获得焦点时选中其中的文字。现在下一步是将过滤器的业务逻辑进行抽象。如下，我们将所有逻辑都放到了一个额外的[`货币验证器` 对象](https://gist.github.com/chrisvfritz/5f0a639590d6e648933416f90ba7ae4e)中:

<iframe width="100%" height="300" src="https://jsfiddle.net/chrisvfritz/9c32kev2/embedded/js,html,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

这个额外的模块不仅让迁移到Vue 2更加简单，同时也让货币转换和格式化可以

- 脱离你的Vue代码进行单元测试
- 在应用的其他部分可以进行利用，如对API加载完成时的验证

把这个验证器抽象出来后，可以很方便地将它加入到一个更可靠的解决方案中。那些奇怪的状态都被清除掉，同时用户也不可能进行任何的错误输入，就像浏览器原生的数字输入框(number input)的行为那样。

现在还有一些限制，还在使用过滤器和Vue 1.0, 所以让我们完成升级到Vue 2.0:

<iframe width="100%" height="300" src="https://jsfiddle.net/chrisvfritz/1oqjojjx/embedded/js,html,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

You may notice that:

可以注意到：

- input的各方面现在都更加清晰，使用了生命周期钩子和DOM事件，替换了双向过滤器的隐式行为。
- 现在能在自定义文本输入框中直接使用 `v-model` ，不仅对正常的输入进行持久化，同时也意味着我们的组件对Vuex更加友好。
- 由于我们不再使用过滤器选项（需要一个返回值），对货币相关的处理现在实际上可以是异步的。也就是说如果我们有很多应用需要货币处理，我们可以很方便地把这个逻辑重构成一个共享的小服务。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有在指令(如<code>v-model</code>)中使用过滤器的例子。如果迁移没有完成，会出现<strong>console errors</strong>。</p>
</div>
{% endraw %}

## Slots

### 名称重复的Slot <sup>已废弃</sup>

新版本将不再支持在同一个模板中同样名字的多个 `<slot>` 。当一个slot被渲染后，就不能在同一个渲染树中被再次渲染。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>运行你的端到端测试套件或者应用，找关于<code>v-model</code>中重复slots的<strong>console warnings</strong>。</p>
</div>
{% endraw %}

### `slot` 的样式属性 <sup>已废弃</sup>

通过名为 `<slot>` 插入的内容现在不提供 `slot` 属性。用一个元素包裹来写入样式，或者在一些高阶的使用场景中，使用 [渲染函数](render-function.html) 来修改插入的内容。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到具名slots上存在css选择器的例子（如<code>[slot="my-slot-name"]</code>）。</p>
</div>
{% endraw %}

## 特殊属性

### `keep-alive` 属性 <sup>已废弃</sup>

`keep-alive` is no longer a special attribute, but rather a wrapper component, similar to `<transition>`. For example:

`keep-alive` 现在是一个包裹组件，而不再是一个特殊属性，类似 `<transition>` 。举例：

``` html
<keep-alive>
  <component v-bind:is="view"></component>
</keep-alive>
```

这样可以让 `<keep-alive>`使用在多个条件性子组件上：

``` html
<keep-alive>
  <todo-list v-if="todos.length > 0"></todo-list>
  <no-todos-gif v-else></no-todos-gif>
</keep-alive>
```

<p class="tip">当 `<keep-alive>` 有多个子组件时，最后会被解析成单个子组件。除了第一个外的其他子组件会被简单地忽略掉。</p>

当和 `<transition>` 在一起使用时，需要把它放在`<transition>` 的内部：

``` html
<transition>
  <keep-alive>
    <component v-bind:is="view"></component>
  </keep-alive>
</transition>
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到<code>keep-alive</code> 属性）。</p>
</div>
{% endraw %}

## 插值

### 结合属性名的插值 <sup>已废弃</sup>

结合属性名的插值将不再起作用。举例：

``` html
<button class="btn btn-{{ size }}"></button>
```

可以使用一个行内表达式来替代：

``` html
<button v-bind:class="'btn btn-' + size"></button>
```

或者一个data/计算属性：

``` html
<button v-bind:class="buttonClasses"></button>
```

``` js
computed: {
  buttonClasses: function () {
    return 'btn btn-' + size
  }
}
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到插值和属性结合使用的例子。</p>
</div>
{% endraw %}

### HTML 插值 <sup>已废弃</sup>

HTML插值（如`{% raw %}{{{ foo }}}{% endraw %}`）已经被废弃，因为现在有[`v-html` 指令](/api/#v-html)来替代它。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到HTML插值的例子。</p>
</div>
{% endraw %}

### 单次插值绑定  <sup>已废弃</sup>

单次绑定 (如`{% raw %}{{* foo }}{% endraw %}`) 已经被废弃，因为现在有新的[`v-once` 指令](/api/#v-once) 来替代它。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到单次插值绑定的例子。</p>
</div>
{% endraw %}

## 响应式相关

### `vm.$watch`

通过 `vm.$watch` 创建的监听器现在会在相关组件进行重新渲染时触发事件。这种方式可以在组件重新渲染时进行状态的再更新，避免不必要的更新。举个例子，可以监听一个组件的属性,当属性变化时来更新组件本身的数据。

如果你之前依赖于通过 `vm.$watch` 在组件更新后进行一些DOM操作，可以在 `updated` 生命周期钩子中来实现。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>运行你的端到端测试（如果有的话）。<strong>failed tests</strong> 会提醒你watcher依赖于旧版本的行为。</p>
</div>
{% endraw %}

### `vm.$set`

之前版本的 `vm.$set` 行为已经被废弃，现在它只是一个[`Vue.set`](/api/#Vue-set)的别名。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到vm.$set的例子。</p>
</div>
{% endraw %}

### `vm.$delete`

之前版本的 `vm.$delete` 行为已经被废弃，现在它只是一个[`Vue.delete`](/api/#Vue-set)的别名。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到vm.$delete的例子。</p>
</div>
{% endraw %}

### `Array.prototype.$set`  <sup>已废弃</sup>

使用 Vue.set 来替代。

(console error, migration helper)

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到在数组上使用$set的例子。如果没有全部迁移完成，会看到missing method的 <strong>console errors</strong>。</p>
</div>
{% endraw %}

### `Array.prototype.$remove` <sup>已废弃</sup>

使用 `Array.prototype.splice` 来替代。举例：

``` js
methods: {
  removeTodo: function (todo) {
    var index = this.todos.indexOf(todo)
    this.todos.splice(index, 1)
  }
}
```

或者更好的方法是把index传入这个删除函数：

``` js
methods: {
  removeTodo: function (index) {
    this.todos.splice(index, 1)
  }
}
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到在数组上使用$remove的例子。如果没有全部迁移完成，会看到missing method的 <strong>console errors</strong>。</p>
</div>
{% endraw %}

### `Vue.set` 和 `Vue.delete` on Vue instances <sup>已废弃</sup>

Vue.set 和 Vue.delete在Vue实例上不再可用。现在需要强制在data选项中声明所有的响应式属性。如果你需要删除一个Vue实例上的属性或 `$data`, 设置其为null即可。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到在Vue实例上使用Vue.set或Vue.delete的例子。如果没有全部迁移完成，会看到
  <strong>console errors</strong>。</p>
</div>
{% endraw %}

### 替换 `vm.$data` <sup>已废弃</sup>

现在不允许替换一个组件实例的根`$data`。这可以避免在响应式系统中的一些边际情况，也可以让组件状态变得更加可预期（尤其是类型检测系统）。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到覆写 <code>vm.$data</code>的例子。如果没有全部迁移完成，会看到
  <strong>console warnings</strong>。</p>
</div>
{% endraw %}

### `vm.$get` <sup>已废弃</sup>

直接从响应式的data中取值即可。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到使用 <code>vm.$get</code>的例子。如果没有全部迁移完成，会看到
  <strong>console errors</strong>。</p>
</div>
{% endraw %}

## DOM相关的实例方法

### `vm.$appendTo` <sup>deprecated</sup>

使用原生的DOM API：

``` js
myElement.appendChild(vm.$el)
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到使用 <code>vm.$appendTo</code>的例子。如果没有全部迁移完成，会看到
  <strong>console errors</strong>。</p>
</div>
{% endraw %}

### `vm.$before` <sup>已废弃</sup>

使用原生的DOM API：

``` js
myElement.parentNode.insertBefore(vm.$el, myElement)
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到使用 <code>vm.$before</code>的例子。如果没有全部迁移完成，会看到
  <strong>console errors</strong>。</p>
</div>
{% endraw %}

### `vm.$after` <sup>已废弃</sup>

使用原生的DOM API：

``` js
myElement.parentNode.insertBefore(vm.$el, myElement.nextSibling)
```

或者如果 `myElement` 是最后一个子元素：

``` js
myElement.parentNode.appendChild(vm.$el)
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到使用 <code>vm.$after</code>的例子。如果没有全部迁移完成，会看到
  <strong>console errors</strong>。</p>
</div>
{% endraw %}

### `vm.$remove` <sup>已废弃</sup>

使用原生的DOM API：

``` js
vm.$el.remove()
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到使用 <code>vm.$remove</code>的例子。如果没有全部迁移完成，会看到
  <strong>console errors</strong>。
</div>
{% endraw %}

## 实例方法

### `vm.$eval` <sup>已废弃</sup>

并没有什么实际用途。如果你正好在代码中使用了这个特性，而且不知道怎么迁移到新版本，在我们的[论坛](http://forum.vuejs.org/)寻求帮助。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到使用 <code>vm.$eval</code>的例子。如果没有全部迁移完成，会看到
  <strong>console errors</strong>。
</div>
{% endraw %}

### `vm.$interpolate` <sup>已废弃</sup>

并没有什么实际用途。如果你正好在代码中使用了这个特性，而且不知道怎么迁移到新版本，在我们的[论坛](http://forum.vuejs.org/)寻求帮助。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到使用 <code>vm.$interpolate</code>的例子。如果没有全部迁移完成，会看到
  <strong>console errors</strong>。
</div>
{% endraw %}

### `vm.$log` <sup>已废弃</sup>

使用[Vue Devtools](https://github.com/vuejs/vue-devtools) 来获得更好的调试体验。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到使用 <code>vm.$log</code>的例子。如果没有全部迁移完成，会看到
  <strong>console errors</strong>。
</div>
{% endraw %}

## 实例中的DOM选项

### `replace: false` <sup>已废弃</sup>

组件现在会始终替换其绑定的元素。如果需要模拟 `replace: false` 的行为，可以使用一个元素包裹根组件。举例：

``` js
new Vue({
  el: '#app',
  template: '<div id="app"> ... </div>'
})
```

或者使用一个渲染函数：

``` js
new Vue({
  el: '#app',
  render: function (h) {
    h('div', {
      attrs: {
        id: 'app',
      }
    }, /* ... */)
  }
})
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到使用 <code>replace: false</code>的例子。</strong>。
</div>
{% endraw %}

## 全局设置

### `Vue.config.debug` <sup>已废弃</sup>

因为警告现在和默认和堆栈跟踪一起打印出来，所以此设置已经没必要了。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到使用 <code>Vue.config.debug</code>的例子。</strong>。
</div>
{% endraw %}

### `Vue.config.async` <sup>已废弃</sup>

从渲染性能考虑，异步模式是默认选项。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到使用 <code>Vue.config.async</code>的例子。</strong>。
</div>
{% endraw %}

### `Vue.config.delimiters` <sup>已废弃</sup>

这个配置项现在是 [组件级的选项](/api/#delimiters)。这允许你在应用中使用其他的定界符，而不会对第三方组件的使用产生破坏性影响。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到使用 <code>Vue.config.delimiters</code>的例子。</strong>。
</div>
{% endraw %}

### `Vue.config.unsafeDelimiters` <sup>已废弃</sup>

HTML插值由于有[`v-html`](#HTML-Interpolation-deprecated)而被废弃了。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到使用 <code>Vue.config.unsafeDelimiters</code>的例子。之后，helper也会找到HTML插值的例子，将它们替换为 `v-html`。</p>
</div>
{% endraw %}

## 全局API

### `Vue.extend` 和 `el` 结合使用 <sup>已废弃deprecated</sup>

el这个选项现在不再能和 `Vue.extend` 结合使用，只在一个实例被创建时才是一个有效的选项。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到<code>Vue.extend</code> <code>el</code>选项结合使用的例子。</p>
</div>
{% endraw %}

### `Vue.elementDirective` <sup>已废弃</sup>

使用组件来替代。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到使用 <code>Vue.elementDirective</code> <code>el</code>的例子。</p>
</div>
{% endraw %}

### `Vue.partial` <sup>已废弃</sup>

Partials已经被废弃，因为组件间的数据流有更明确的解决方法 - 使用属性。除非你在应用中的关键性能部分使用了partial，那么推荐使用一个[普通组件](components.html)来替代。如果你希望动态绑定一个partial的名称，可以使用一个[动态组件](http://vuejs.org/guide/components.html#Dynamic-Components)。

如果你正好在应用的关键性能部分使用了partial, 那么你可以升级到 [函数式组件]。这是一个普通的 JS/JSX 文件（而不是一个 `.vue` 文件），同时它不具有状态和实例，就和partial一样。这可以让渲染非常快。

由函数式组件代替partial的一个好处时，它们变得更加自由，因为可以使用所有Javascript来定义它。但是这也是代价的，如果你从未使用一个基于渲染函数的组件框架，需要一些时间来学习。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ，找到使用 <code>Vue.partial</code>的例子。</p>
</div>
{% endraw %}
