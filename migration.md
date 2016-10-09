---
title: Migration from Vue 1.x
type: guide
order: 24
---

## FAQ

> Woah - this is a super long page! Does that mean 2.0 is completely different, I'll have to learn the basics all over again, and migrating will be practically impossible?

> 哇 - 这个页面真是太长了！这是否意味着2.0和之前的版本完全不同，我需要从基础从新学起，而低版本的迁移也会变得很困难吗？

I'm glad you asked! The answer is no. About 90% of the API is the same and the core concepts haven't changed. It's long because we like to offer very detailed explanations and include a lot of examples. Rest assured, __this is not something you have to read from top to bottom!__

我很高兴你提了这个问题！答案是不。大约90%的API都和过去一样，而且核心概念都未曾变动。这页面之所以长，是因为我们提供了非常详细的解释，包括很多实例。放心，__你无需从头至尾地看一遍！__

> Where should I start in a migration?

> 迁移的步骤是？

1. Start by running the [migration helper](https://github.com/vuejs/vue-migration-helper) on a current project. We've carefully minified and compressed a senior Vue dev into a simple command line interface. Whenever they recognize a deprecated pattern, they'll let you know, offer suggestions, and provide links to more info.

1. 首先，在你的项目中运行[migration helper](https://github.com/vuejs/vue-migration-helper)。这个命令行界面工具包含了一个Vue的精简压缩的开发版本。当它检测到一个被废弃的模式时，会提醒你，给出建议，并会提供查看更多信息的一些链接。

2. After that, browse through the table of contents for this page in the sidebar. If you see a topic you may be affected by, but the migration helper didn't catch, check it out.

2. 之后，在侧边栏中浏览一下这个页面的大致内容。如果发现某个地方的改动可能会影响到你，但migration helper并没有提醒你，那么请指出。

3. If you have any tests, run them and see what still fails. If you don't have tests, just open the app in your browser and keep an eye out for warnings or errors as you navigate around.

3. 如果你之前的项目有测试，运行看看有哪些还是测试未通过的。如果没有测试，就在浏览器中打开应用，关注有哪些warning和error.

4. By now, your app should be fully migrated. If you're still hungry for more though, you can read the rest of this page - or just dive in to the new and improved guide from [the beginning](index.html). Many parts will be skimmable, since you're already familiar with the core concepts.

4. 至此，你的应用迁移就完成了。如果还想了解更多，可以读这篇剩下的部分 - 或者就从[the beginning](index.html)开始看吧，这是一个改进版的入门指引。很多内容都可以快速浏览，因为你已经对核心概念比较熟悉了。

> How long will it take to migrate a Vue 1.x app to 2.0?

> 从Vue 1.x应用迁移到2.0需要多长时间？

It depends on a few factors:

取决于两个因素：

- The size of your app (small to medium-sized apps will probably be less than a day)

- 应用的规模（小到中型的应用应该在一天之内可以完成）

- How many times you get distracted and start playing with a cool new feature. 😉 &nbsp;Not judging, it also happened to us while building 2.0!

- 你有多少次在玩一个很酷的新特性时抓狂过。不单是你，我们在开发2.0时也这样！

- Which deprecated features you're using. Most can be upgraded with find-and-replace, but others might take a few minutes. If you're not currently following best practices, Vue 2.0 will also try harder to force you to. This is a good thing in the long run, but could also mean a significant (though possibly overdue) refactor.

- 你使用了哪些被废弃的特性。大多数都可以通过「查找-替换」升级完成，但是小部分需要一些时间。如果你没有使用推荐的最佳初中，Vue 2.0也会强制让你按这么去做。长远来看这是件好事，但是也可能意味着这是一个标志性（可能有点过了）的重构。

> If I upgrade to Vue 2, will I also have to upgrade Vuex and Vue-Router?

> 如果升级到了Vue 2, 也需要同时升级Vuex和Vue-Router吗？

Only Vue-Router 2 is compatible with Vue 2, so yes, you'll have to follow the [migration path for Vue-Router](migration-vue-router.html) as well. Fortunately, most applications don't have a lot of router code, so this likely won't take more than an hour.

只有Vue-Router 2和Vue 2是兼容的，所以你也得按[migration path for Vue-Router](migration-vue-router.html) 中的内容来操作。幸运的是，大多数应用在router这块都没有太多代码，所以这部分应该不到一小时就可以搞定。

As for Vuex, even version 0.8 is compatible with Vue 2, so you're not forced to upgrade. The only reason you may want to upgrade immediately is to take advantage of the new features in Vuex 2, such as modules and reduced boilerplate.

Vuex方面，甚至0.8版本都可以和Vue 2兼容，所以升级并非强制性的。唯一可能的升级理由是享受Vuex 2带来的好处和新特性，如模块化和reduced boilerplate。

## Templates

### Fragment Instances <sup>deprecated</sup>

### Fragment实例 <sup>已废弃</sup>

Every component must have exactly one root element. Fragment instances are no longer allowed. If you have a template like this:

每个组件现在都需要有一个明确的root元素。Fragments实例已经不再支持。如果你有个template像这样：

``` html
<p>foo</p>
<p>bar</p>
```

It's recommended to simply wrap the entire contents in a new element, like this:

推荐把所有内容包在一个新元素内，如下：

``` html
<div>
  <p>foo</p>
  <p>bar</p>
</div>
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run your end-to-end test suite or app after upgrading and look for <strong>console warnings</strong> about multiple root elements in a template.</p>
  <h4>升级路线</h4>
  <p>运行你的端到端测试套件或者应用，找关于「multiple root elements in a template」的console warnings。</p>
</div>
{% endraw %}

## Lifecycle Hooks

### `beforeCompile` <sup>deprecated</sup>

### `beforeCompile` <sup>已废弃</sup>

Use the `created` hook instead.

使用`created`来替代。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find all examples of this hook.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到所有这个hook。</p>
</div>
{% endraw %}

### `compiled` <sup>deprecated</sup>

### `compiled` <sup>已废弃</sup>

Use the new `mounted` hook instead.

使用新的hook`mounted`来替代。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find all examples of this hook.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到所有这个hook。</p>
</div>
{% endraw %}

### `attached` <sup>deprecated</sup>

### `attached` <sup>已废弃</sup>

Use a custom in-dom check in other hooks. For example, to replace:

在其他hook中进行「in-dom check」来替代。举例，替换如下的内容：

``` js
attached: function () {
  doSomething()
}
```

You could use:

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
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find all examples of this hook.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到所有这个hook。</p>
</div>
{% endraw %}

### `detached` <sup>deprecated</sup>

### `detached` <sup>已废弃</sup>

Use a custom in-dom check in other hooks. For example, to replace:

在其他hook中进行「in-dom check」来替代。举例，替换如下的内容：

``` js
detached: function () {
  doSomething()
}
```

You could use:

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
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find all examples of this hook.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到所有这个hook。</p>
</div>
{% endraw %}

### `init` <sup>deprecated</sup>

### `init` <sup>已废弃</sup>

Use the new `beforeCreate` hook instead, which is essentially the same thing. It was renamed for consistency with other lifecycle methods.

使用新的hook `beforeCreate` 来替代。这实质上是同一个东西，只是换了名称，是为了和其他生命周期方法保持一致。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find all examples of this hook.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到所有这个hook。</p>
</div>
{% endraw %}

### `ready` <sup>deprecated</sup>

### `ready` <sup>已废弃</sup>

Use the new `mounted` hook instead. It should be noted though that with `mounted`, there's no guarantee to be in-document. For that, also include `Vue.nextTick`/`vm.$nextTick`. For example:

用新的hook `mounted` 来替代。 需要注意的是，在`mounted`时，不能保证已经「document ready」。因此需要加入`Vue.nextTick`/`vm.$nextTick`。举例：

``` js
mounted: function () {
  this.$nextTick(function () {
    // code that assumes this.$el is in-document
  })
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find all examples of this hook.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到所有这个hook。</p>
</div>
{% endraw %}

## `v-for`

### `v-for` Argument Order for Arrays

### `v-for` 使用数组时的参数顺序

When including an `index`, the argument order for arrays used to be `(index, value)`. It is now `(value, index)` to be more consistent with JavaScript's native array methods such as `forEach` and `map`.

当在`v-for`中使用数组`index`时，以往的参数顺序是`(index, value)`。现在是`(value, index)`，保持和Javascript原生数组方法的一致性，如`forEach` 和 `map`。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the deprecated argument order. Note that if you name your index arguments something unusual like <code>position</code> or <code>num</code>, the helper will not flag them.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到deprecated argument order。注意，如果你把参数名改成类似<code>position</code> 或 <code>num</code>这样的名字，那么helper将不会提醒。</p>
</div>
{% endraw %}

### `v-for` Argument Order for Objects

### `v-for` 使用Object时的参数顺序

When including a `key`, the argument order for objects used to be `(key, value)`. It is now `(value, key)` to be more consistent with common object iterators such as lodash's.

当在`v-for`中使用Object的`key`时，以往的参数顺序是`(key, value)`。现在是`(value, key)`，保持和大多数库中object迭代器的一致性，如`lodash`。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the deprecated argument order. Note that if you name your key arguments something like <code>name</code> or <code>property</code>, the helper will not flag them.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到deprecated argument order。注意，如果你把参数名key改成类似<code>name</code> 或 <code>property</code>这样的名字，那么helper将不会提醒。</p>
</div>
{% endraw %}

### `$index` and `$key` <sup>deprecated</sup>

### `$index` and `$key` <sup>已废弃</sup>

The implicitly assigned `$index` and `$key` variables have been deprecated in favor of explicitly defining them in `v-for`. This makes the code easier to read for developers less experienced with Vue and also results in much clearer behavior when dealing with nested loops.

隐式声明的两个变量 `$index` 和 `$key` 已经被废弃，因为在 `v-for` 里已经显式声明了它们。这可以让不熟悉Vue的开发者更加易懂，在做嵌套循环的时候也更加清晰。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of these deprecated variables. If you miss any, you should also see <strong>console errors</strong> such as: <code>Uncaught ReferenceError: $index is not defined</code></p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到这些已废弃的变量。如果没有处理完，会看到一个像这样的<strong>console errors</strong>：<code>Uncaught ReferenceError: $index is not defined</code></p>
</div>
{% endraw %}

### `track-by` <sup>deprecated</sup>

### `track-by` <sup>已废弃</sup>

`track-by` has been replaced with `key`, which works like any other attribute: without the `v-bind:` or `:` prefix, it is treated as a literal string. In most cases, you'd want to use a dynamic binding which expects a full expression instead of a key. For example, in place of:

`track-by` 现在已经被 `key` 替代，工作方式和其他属性的方式一样： 如果没有 `v-bind:` 或 `:` 前缀，就当作一个字符串来处理。大多数情况下，你可能需要一个带完整表达式的动态绑定，而不是一个key名。替换举例：

``` html
<div v-for="item in items" track-by="id">
```

You would now write:

现在这么写：

``` html
<div v-for="item in items" v-bind:key="item.id">
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>track-by</code>.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到 <code>track-by</code> 的例子。</p>
</div>
{% endraw %}

### `v-for` Range Values

### `v-for` 的值范围

Previously, `v-for="number in 10"` would have `number` starting at 0 and ending at 9. Now it starts at 1 and ends at 10.

之前，`v-for="number in 10"` 会使 `number` 的值范围从0至9。 现在的值范围是1至10。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Search your codebase for the regex <code>/\w+ in \d+/</code>. Wherever it appears in a <code>v-for</code>, check to see if you may be affected.</p>
  <h4>升级路线</h4>
  <p>在你的项目下找这个正则匹配  <code>/\w+ in \d+/</code> 。如果找到的结果在一个 <code>v-for</code>中，观察是否受到了影响。</p>
</div>
{% endraw %}

## Props

### `coerce` Prop Option <sup>deprecated</sup>

### `coerce` Prop Option <sup>已废弃</sup>

If you want to coerce a prop, setup a local computed value based on it instead. For example, instead of:

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

You could write:

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

There are a few advantages:

这样有几个好处：

- You still have access to the original value of the prop.
- You are forced to be more explicit, by giving your coerced value a name that differentiates it from the value passed in the prop.

- 仍然可以获取prop的原始值。
- 强制要求给转换后的值取一个与value值不一样的属性名称。可以让代码的目的性更加明确。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>coerce</code> option.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到 <code>coerce</code> option。</p>
</div>
{% endraw %}

### `twoWay` Prop Option <sup>deprecated</sup>

### `twoWay` Prop Option <sup>已废弃</sup>

Props are now always one-way down. To produce side effects in the parent scope, a component needs to explicitly emit an event instead of relying on implicit binding. For more information, see:

Props现在始终是单向流动的。要使组件的变动影响它的parent scope, 就需要在它自身emit事件来触发，而不是依赖于一个隐式的绑定。看以下这些链接了解更多：

- [Custom component events](components.html#Custom-Events)
- [Custom input components](components.html#Form-Input-Components-using-Custom-Events) (using component events)
- [Global state management](state-management.html)

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>twoWay</code> option.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到 <code>twoWay</code> option。</p>
</div>
{% endraw %}

### `v-bind` with `.once` and `.sync` Modifiers <sup>deprecated</sup>

### `v-bind` with `.once` and `.sync` Modifiers <sup>deprecated</sup>

Props are now always one-way down. To produce side effects in the parent scope, a component needs to explicitly emit an event instead of relying on implicit binding. For more information, see:

Props现在始终是单向流动的。要使组件的变动影响它的parent scope, 就需要在它自身emit事件来触发，而不是依赖于一个隐式的绑定。看以下这些链接了解更多：

- [Custom component events](components.html#Custom-Events)
- [Custom input components](components.html#Form-Input-Components-using-Custom-Events) (using component events)
- [Global state management](state-management.html)

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>.once</code> and <code>.sync</code> modifiers.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到 <code>.once</code> 和 <code>.sync</code> 这些modifiers。</p>
</div>
{% endraw %}

### Prop Mutation <sup>deprecated</sup>

### Prop Mutation <sup>已废弃</sup>

Mutating a prop locally is now considered an anti-pattern, e.g. declaring a prop and then setting `this.myProp = 'someOtherValue'` in the component. Due to the new rendering mechanism, whenever the parent component re-renders, the child component's local changes will be overwritten.

在本地改变一个prop值 (mutation) 现在被认为是一个「反模式」，举例：声明一个prop, 然后在component中设置`this.myProp = 'someOtherValue'` 。在新的渲染机制下，当父component重新渲染时，子component的本地修改会被覆盖。

Most use cases of mutating a prop can be replaced by one of these options:

大多数prop值的改变可以替换为以下形式：

- a data property, with the prop used to set its default value
- a computed property

- data property, prop用于设置其默认值
- computed property

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run your end-to-end test suite or app after upgrading and look for <strong>console warnings</strong> about prop mutations.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到关于prop mutations的<strong>console warnings</strong></p>
</div>
{% endraw %}

### Props on a Root Instance <sup>deprecated</sup>

### Props on a Root Instance <sup>已废弃</sup>

On root Vue instances (i.e. instances created with `new Vue({ ... })`), you must use `propsData` instead of `props`.

Vue根实例（Root Instance）上的 `props` 现在需要用 `propsData` 来替换。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run your end-to-end test suite, if you have one. The <strong>failed tests</strong> should alert to you to the fact that props passed to root instances are no longer working.</p>
  <h4>升级路线</h4>
  <p>运行你的端到端测试（如果有的话）。<strong>failed tests</strong> 会提醒你在根实例上传入的props已经无法工作了。</p>
</div>
{% endraw %}

## Built-In Directives

## 内置指令

### Truthiness/Falsiness with `v-bind`

### `v-bind`的真假值

When used with `v-bind`, the only falsy values are now: `null`, `undefined`, and `false`. This means `0` and empty strings will render as truthy. So for example, `v-bind:draggable="''"` will render as `draggable="true"`.

在使用 `v-bind` 时，当前版本的假值有这些： `null`, `undefined` 和 `false` 。也就是说 `0` 和空字符串现在会被处理成真值。举个例子，`v-bind:draggable="''"` 现在会被渲染为 `draggable="true"`。

For enumerated attributes, in addition to the falsy values above, the string `"false"` will also render as `attr="false"`.

对于枚举属性，除了上述提到的，`"false"` 这个字符串也会被渲染为  `attr="false"` 。

<p class="tip">Note that for other directives (e.g. `v-if` and `v-show`), JavaScript's normal truthiness still applies.</p>

<p class="tip">注意，对于其他的指令 (如 `v-if` 和 `v-show`, 上述写法还是会被处理成真值。)</p>

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run your end-to-end test suite, if you have one. The <strong>failed tests</strong> should alert to you to any parts of your app that may be affected by this change.</p>
  <h4>升级路线</h4>
  <p>运行你的端到端测试（如果有的话）。<strong>failed tests</strong> 会提醒你的应用由于这个改动带来的影响。</p>
</div>
{% endraw %}

### Listening for Native Events on Components with `v-on`

### 在组件上使用 `v-on` 来监听原生事件

When used on a component, `v-on` now only listens to custom events `$emit`ted by that component. To listen for a native DOM event on the root element, you can use the `.native` modifier. For example:

当使用组件时， `v-on` 现在只对自身 `$emit` 的自定义事件起作用。 如果需要在一个根元素上监听原生DOM事件，可以使用 `.native` 这个修饰器。举例：

``` html
<my-component v-on:click.native="doSomething"></my-component>
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run your end-to-end test suite, if you have one. The <strong>failed tests</strong> should alert to you to any parts of your app that may be affected by this change.</p>
  <h4>升级路线</h4>
  <p>运行你的端到端测试（如果有的话）。<strong>failed tests</strong> 会提醒你的应用由于这个改动带来的影响。</p>
</div>
{% endraw %}

### `v-model` with `debounce` <sup>deprecated</sup>

### `v-model` 和 `debounce` 结合使用 <sup>已废弃</sup>

Debouncing is used to limit how often we execute Ajax requests and other expensive operations. Vue's `debounce` attribute parameter for `v-model` made this easy for very simple cases, but it actually debounced __state updates__ rather than the expensive operations themselves. It's a subtle difference, but it comes with limitations as an application grows.

Debouncing现在被用于限制发送ajax请求，或其他费时操作的频率。 Vue中使用 `v-model` 的 `debounce` 属性适用于一些非常简单的例子， 但是实际上在运行中__状态变更__ 也延迟执行了，这个时间甚至会比那此费时操作更长。这个差别很小，但是随着应用规模的增长，会带来一定的限制。

These limitations become apparent when designing a search indicator, like this one for example:

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

Using the `debounce` attribute, there'd be no way to detect the "Typing" state, because we lose access to the input's real-time state. By decoupling the debounce function from Vue however, we're able to debounce only the operation we want to limit, removing the limits on features we can develop:

如果使用 `debounce` 这个属性，就没有办法去检测 "Typing" 这个状态，因为这个input的实时状态我们无法
得知。为了让Vue和这个debounce function解耦，我们可以只对需要的操作做限制，移除这个在特性上的限制：

``` html
<!--
By using the debounce function from lodash or another dedicated
utility library, we know the specific debounce implementation we
use will be best-in-class - and we can use it ANYWHERE. Not just
in our template.
-->
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
    // This is where the debounce actually belongs.
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

Another advantage of this approach is there will be times when debouncing isn't quite the right wrapper function. For example, when hitting an API for search suggestions, waiting to offer suggestions until after the user has stopped typing for a period of time isn't an ideal experience. What you probably want instead is a __throttling__ function. Now since you're already using a utility library like lodash, refactoring to use its `throttle` function instead takes only a few seconds.

另外一个好处是，当debounce函数不是包裹函数时，还会耗费时间。举例，当命中搜索建议时，用户已经停止输入了还需要等待一段时间才能得到搜索结果的体验不太理想。这时可能需要一个 __throttling__ 函数来代替它。如果你已经使用了类似lodash这样的工具库，换成使用 `throttle` 函数，很快就能搞定。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>debounce</code> attribute.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到 <code>debounce</code> 的例子。</p>
</div>
{% endraw %}

### `v-model` with `lazy` or `number` Param Attributes <sup>deprecated</sup>

### `v-model` 和 `lazy` 或 `number` 这样的参数特性结合使用 <sup>已废弃</sup>

The `lazy` and `number` param attributes are now modifiers, to make it more clear what That means instead of:

`lazy` 和 `number` 这些参数特性现在是修饰器，以使代码更加清晰，也就是说替换以下例子：

``` html
<input v-model="name" lazy>
<input v-model="age" type="number" number>
```

You would use:

你可以这么写：

``` html
<input v-model.lazy="name">
<input v-model.number="age" type="number">
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the these deprecated param attributes.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到这些被废弃的参数特性的例子。</p>
</div>
{% endraw %}

### `v-model` with Inline `value` <sup>deprecated</sup>

### `v-model` 和行内 `value` 结合使用 <sup>已废弃</sup>

`v-model` no longer cares about the initial value of an inline `value` attribute. For predictability, it will instead always treat the Vue instance data as the source of truth.

`v-model` 现在不再关心行内 `value` 属性的初始值。现在仅会根据Vue实例中的数据来处理。

That means this element:

也就是说这个元素：

``` html
<input v-model="text" value="foo">
```

backed by this data:

在data中的数据是这样的：

``` js
data: {
  text: 'bar'
}
```

will render with a value of "bar" instead of "foo". The same goes for a `<textarea>` with existing content. Instead of:

会渲染成"bar"而不是"foo"。对于 `<textarea>` 也会做同样处理。如下：

``` html
<textarea v-model="text">
  hello world
</textarea>
```

You should ensure your initial value for `text` is "hello world".

需要保证你的 `text` 初始化值是"hello world"。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run your end-to-end test suite or app after upgrading and look for <strong>console warnings</strong> about inline value attributes with <code>v-model</code>.</p>
  <h4>升级路线</h4>
  <p>升级后运行你的端到端测试（如果有的话）。找到<strong>console warnings</strong> 中关于 inline value attributes with <code>v-model</code>的部分。</p>
</div>
{% endraw %}

### `v-model` with `v-for` Iterated Primitive Values <sup>deprecated</sup>

### 使用`v-model` 和 `v-for` 进行原始值迭代 <sup>已废弃</sup>


Cases like this no longer work:

这样的例子将不会工作：

``` html
<input v-for="str in strings" v-model="str">
```

The reason is this is the equivalent JavaScript that the `<input>` would compile to:

因为这样的话，相当于 `<input>` 会被编译成如下的Javascript:

``` js
strings.map(function (str) {
  return createElement('input', ...)
})
```

As you can see, `v-model`'s two-way binding doesn't make sense here. Setting `str` to another value in the iterator function will do nothing because it's just a local variable in the function scope.

如你所见，`v-model` 的双向绑定在这里行不通。在迭代函数中设置 `str`为其他的值将没有作用，因为它在函数作用域中只是一个本地变量。 

Instead, you should use an array of __objects__ so that `v-model` can update the field on the object. For example:

你可以使用一个 __objects__ 数组来替代，以使  `v-model` 可以在这个对象上进行更新。举例：

``` html
<input v-for="obj in objects" v-model="obj.str">
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run your test suite, if you have one. The <strong>failed tests</strong> should alert to you to any parts of your app that may be affected by this change.</p>
  <h4>升级路线</h4>
  <p>运行你的测试（如果有的话）。<strong>failed tests</strong> 会提醒你的应用由于这个改动带来的影响。</p>
</div>
{% endraw %}

### `v-bind:style` with Object Syntax and `!important` <sup>deprecated</sup>

### 在`v-bind:style`表达式中同时使用Object声明和 `!important` <sup>已废弃</sup>

This will no longer work:

以下例子将不再工作：

``` html
<p v-bind:style="{ color: myColor + ' !important' }">hello</p>
```

If you really need to override another `!important`, you must use the string syntax:

如果你确实需要覆盖另外一个 `!important`，需要使用字符串拼接：

``` html
<p v-bind:style="'color: ' + myColor + ' !important'">hello</p>
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of style bindings with <code>!important</code> in objects.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有<code>!important</code>的style绑定的例子。</p>
</div>
{% endraw %}

### `v-el` and `v-ref` <sup>deprecated</sup>

### `v-el` 和 `v-ref` <sup>已废弃</sup>

For simplicity, `v-el` and `v-ref` have been merged into the `ref` attribute, accessible on a component instance via `$refs`. That means `v-el:my-element` would become `ref="myElement"` and `v-ref:my-component` would become `ref="myComponent"`. When used on a normal element, the `ref` will be the DOM element, and when used on a component, the `ref` will be the component instance.

简单说，`v-el` 和 `v-ref` 已经被合并为 `ref`属性，同时在组件实例中可以通过 `$refs` 来访问它。也就是说 `v-el:my-element` 会变成 `ref="myElement"` ，`v-ref:my-component` 会变成 `ref="myComponent"` 。当在一个普通html元素上使用时， `ref`会是这个DOM元素，当在一个组件上使用时，`ref`将会是这个组件的实例。

Since `v-ref` is no longer a directive, but a special attribute, it can also be dynamically defined. This is especially useful in combination with `v-for`. For example:

虽然 `v-ref` 已经不再是一个内置指令而是个特殊属性，同样可以动态声明它。当在和 `v-for` 一起使用时会变得更实用。举例。

``` html
<p v-for="item in items" v-bind:ref="'item' + item.id"></p>
```

Previously, `v-el`/`v-ref` combined with `v-for` would produce an array of elements/components, because there was no way to give each item a unique name. You can still achieve this behavior by given each item the same `ref`:

在之前版本中，`v-el`/`v-ref` 和 `v-for` 一起使用时，会产生一个 html元素/组件的数组，因为没有别的方式可以给每个item定义唯一的名称。同时你也可以通过给每个item同样的`ref`来实现这个功能：

``` html
<p v-for="item in items" ref="items"></p>
```

Unlike in 1.x, these `$refs` are not reactive, because they're registered/updated during the render process itself. Making them reactive would require duplicate renders for every change.

不像1.x版本， 这些 `$refs` 不是响应式的，因为在渲染过程中已经注册或更新过它们了。让它们变为响应式需要对每个改动做重复的渲染工作。

On the other hand, `$refs` are designed primarily for programmatic access in JavaScript - it is not recommended to rely on them in templates, because that would mean referring to state that does not belong to the instance itself. This would violate Vue's data-driven view model.

另一方面， `$refs` 主要是被设计成在Javascript程序中访问的 - 所以并不推荐在模板中去依赖它们，因为指向的是不属于该实例本身的状态。这会违反Vue的数据驱动视图模型。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>v-el</code> and <code>v-ref</code>.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有 <code>v-el</code> 和 <code>v-ref</code>的例子。</p>
</div>
{% endraw %}

### `v-else` with `v-show` <sup>deprecated</sup>

### `v-else` 和 `v-show` 结合使用 <sup>已废弃</sup>

`v-else` no longer works with `v-show`. Use `v-if` with a negation expression instead. For example, instead of:

`v-else` 和 `v-show` 将不能在一起结合使用。 使用 `v-if`和一个否定表达式来替代。举例，要替换：

``` html
<p v-if="foo">Foo</p>
<p v-else v-show="bar">Not foo, but bar</p>
```

You can use:

可以写成：

``` html
<p v-if="foo">Foo</p>
<p v-if="!foo && bar">Not foo, but bar</p>
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>v-else</code> with <code>v-show</code>.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有 <code>v-else</code> 和 <code>v-show</code>结合使用的例子。</p>
</div>
{% endraw %}

## Custom Directives

## 自定义指令

Directives have a greatly reduced scope of responsibility: they are now only used for applying low-level direct DOM manipulations. In most cases, you should prefer using components as the main code-reuse abstraction.

指令（Directives）的职责范围被大大缩小了：它们现在只能用于执行一些低级的直接DOM操作。大多数情况下你更应该去使用组件(components)来进行代码复用。

Some of the most notable differences include:

一些最需要说明的不同点包括：

- Directives no longer have instances. This means there's no more `this` inside directive hooks. Instead, they receive everything they might need as arguments. If you really must persist state across hooks, you can do so on `el`.
- Directives将不再会有实例。也就是说在directive的钩子函数中将不存在`this`。在指令中所有需要的东西都将使用参数传入。如果你需要在钩子中做状态持久化，可以在`el`中处理。
- Options such as `acceptStatement`, `deep`, `priority`, etc are all deprecated. To replace `twoWay` directives, see [this example](#Two-Way-Filters-deprecated).
- 像 `acceptStatement`, `deep`, `priority` 等选项已经全部被废弃。替代双向绑定的directives，可见[这个例子](#Two-Way-Filters-deprecated)。
- Some of the current hooks have different behavior and there are also a couple new hooks.
- 现有的directive钩子将会有不同的行为，同时会有一些新钩子。

Fortunately, since the new directives are much simpler, you can master them more easily. Read the new [Custom Directives guide](custom-directive.html) to learn more.

幸运的是，新的directives变得简单多了，掌握起来也更为简单。可以看新的[自定义指令指南](custom-directive.html) 了解更多。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of defined directives. The helper will flag all of them, as it's likely in most cases that you'll want to refactor to a component.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到定义directive的例子。它们会被helper全部标记，在大多数情况下你需要将它重构为一个component。</p>
</div>
{% endraw %}

### Directive `.literal` Modifier <sup>deprecated</sup>

### 指令的 `.literal` 修饰器 <sup>已废弃</sup>


The `.literal` modifier has been removed, as the same can be easily achieved by just providing a string literal as the value.

`.literal`这个修饰器已经被移除，因为可以直接通过提供一个字符串字面量来实现。

For example, you can update:

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
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the `.literal` modifier on a directive.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有在directive上定义 `.literal`修饰器的例子。</p>
</div>
{% endraw %}

## Transitions

## 过渡（Transitions）

### `transition` Attribute <sup>deprecated</sup>

### `transition` 属性 <sup>已废弃</sup>

Vue's transition system has changed quite drastically and now uses `<transition>` and `<transition-group>` wrapper elements, rather than the `transition` attribute. It's recommended to read the new [Transitions guide](transitions.html) to learn more.

Vue的过渡系统已经进行了大幅度改动，现在需要使用`<transition>` 和 `<transition-group>` 元素进行包裹，而不是使用`transition` 属性。推荐阅读新的[过渡指南](transitions.html) 来了解更多。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>transition</code> attribute.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有<code>transition</code>属性的例子。</p>
</div>
{% endraw %}

### `Vue.transition` for Reusable Transitions <sup>deprecated</sup>

### 使用`Vue.transition` 创建复用的过渡 <sup>已废弃</sup>


With the new transition system, you can now just [use components for reusable transitions](http://rc.vuejs.org/guide/transitions.html#Reusable-Transitions).

在新的过渡系统下，你可以直接 [使用组件实现复用过渡](http://rc.vuejs.org/guide/transitions.html#Reusable-Transitions)。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.transition</code>.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有<code>Vue.transition</code>属性的例子。</p>
</div>
{% endraw %}

### Transition `stagger` Attribute <sup>deprecated</sup>

### 过渡的 `stagger` 属性 <sup>已废弃</sup>

If you need to stagger list transitions, you can control timing by setting and accessing a `data-index` (or similar attribute) on an element. See [an example here](transitions.html#Staggering-List-Transitions).

如果你要为一个列表创建一个渐近过渡，可以通过设置和访问 `data-index` （或类似的属性）来控制过渡时间。看[这个例子](transitions.html#Staggering-List-Transitions)。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>transition</code> attribute. During your update, you can transition (pun very much intended) to the new staggering strategy as well.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有 <code>transition</code>属性的例子。在升级过程中，你也可以“过渡”到新的渐近过渡策略上来。</p>
</div>
{% endraw %}

## Events

## 事件

### `events` option <sup>deprecated</sup>

### `events` 选项 <sup>已废弃</sup>

The `events` option has been deprecated. Event handlers should now be registered in the `created` hook instead. Check out the [`$dispatch` and `$broadcast` migration guide](#dispatch-and-broadcast-deprecated) for a detailed example.

`events`选项已经被废弃。在`created`钩子中会注册事件处理器。请查看[`$dispatch` 和 `$broadcast` 迁移指南](#dispatch-and-broadcast-deprecated)这个详细示例。

### `Vue.directive('on').keyCodes` <sup>deprecated</sup>

### `Vue.directive('on').keyCodes` <sup>已废弃</sup>

The new, more concise way to configure `keyCodes` is through`Vue.config.keyCodes`. For example:

新的更简洁的方式来设置 `keyCodes` 是通过`Vue.config.keyCodes`。举例：

``` js
// enable v-on:keyup.f1
// 启用v-on:keyup.f1
Vue.config.keyCodes.f1 = 112
```
{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the the old <code>keyCode</code> configuration syntax.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有使用老的<code>keyCode</code>方式进行设置的例子。</p>
</div>
{% endraw %}

### `$dispatch` and `$broadcast` <sup>deprecated</sup>

### `$dispatch` and `$broadcast` <sup>已废弃</sup>

`$dispatch` and `$broadcast` are being deprecated in favor of more explicitly cross-component communication and more maintainable state management solutions, such as [Vuex](https://github.com/vuejs/vuex).

`$dispatch` 和 `$broadcast` 被废弃的原因是：现在有更加清晰的组件间通信和可维护性更好状态管理方案，如 [Vuex](https://github.com/vuejs/vuex)。

The problem is event flows that depend on a component's tree structure can be hard to reason about and very brittle when the tree becomes large. It simply doesn't scale well and we don't want to set you up for pain later. `$dispatch` and `$broadcast` also do not solve communication between sibling components.

问题在于，组件树变得越来越大时，基于组件树结构的事件流将很难跟踪和维护，可扩展性也不好，我们不想让你后续再痛苦下去。`$dispatch` 和 `$broadcast` 也不能很好地解决兄弟组件的通信问题。

One of the most common uses for these methods is to communicate between a parent and its direct children. In these cases, you can actually [listen to an `$emit` from a child with `v-on`](http://vuejs.org/guide/components.html#Form-Input-Components-using-Custom-Events). This allows you to keep the convenience of events with added explicitness.

最常使用这些方法的是在实现父组件和下一级子组件通信的时候。在这些场景下，你实际上可以可以[使用 `v-on` 监听来自子组件的 `$emit`](http://vuejs.org/guide/components.html#Form-Input-Components-using-Custom-Events)。

However, when communicating between distant descendants/ancestors, `$emit` won't help you. Instead, the simplest possible upgrade would be to use a centralized event hub. This has the added benefit of allowing you to communicate between components no matter where they are in the component tree - even between siblings! Because Vue instances implement an event emitter interface, you can actually use an empty Vue instance for this purpose.

但是，当在和跨多层级的子孙组件/祖先组件进行通信时，`$emit` 也起不了作用。最简单的可能升级方式是使用一个「中央事件管理枢纽」。这样做的好处是：你可以在组件树中 -- 甚至是在兄弟组件间通信！因为Vue的实例实现了一个事件发射器，你可以用一个空的Vue实例来实现它。

For example, let's say we have a todo app structured like this:

举例，比如说我们有一个todo的应用，像是如下的结构：

```
Todos
|-- NewTodoInput
|-- Todo
    |-- DeleteTodoButton
```

We could manage communication between components with this single event hub:

我们可以在这个单事件枢纽中来管理组件间通信：

``` js
// This is the event hub we'll use in every
// component to communicate between them.
// 我们在每个组件中都要使用这个「事件枢纽」来进行它们之间的通信。
var eventHub = new Vue()
```

Then in our components, we can use `$emit`, `$on`, `$off` to emit events, listen for events, and clean up event listeners, respectively:

然后在组件中，我们可以使用 `$emit`, `$on`, `$off` 来派发事件，监听事件，清除事件监听器，分别是：

``` js
// NewTodoInput
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
// DeleteTodoButton
// 删除todo的按钮
// ...
methods: {
  deleteTodo: function (id) {
    eventHub.$emit('delete-todo', id)
  }
}
```

``` js
// Todos
// ...
created: function () {
  eventHub.$on('add-todo', this.addTodo)
  eventHub.$on('delete-todo', this.deleteTodo)
},
// It's good to clean up event listeners before
// a component is destroyed.
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

This pattern can serve as a replacement for `$dispatch` and `$broadcast` in simple scenarios, but for more complex cases, it's recommended to use a dedicated state management layer such as [Vuex](https://github.com/vuejs/vuex).

这种模式在简单场景中可以替代`$dispatch` 和 `$broadcast`；但在更复杂场景中，推荐使用专用的状态管理层，如[Vuex](https://github.com/vuejs/vuex)。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>$dispatch</code> and <code>$broadcast</code>.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有 <code>$dispatch</code>和<code>$broadcast</code>的例子。</p>
</div>
{% endraw %}

## Filters

## 过滤器

### Filters Outside Text Interpolations <sup>deprecated</sup>

### 在文本插值外使用过滤器 <sup>已废弃</sup>

Filters can now only be used inside text interpolations (`{% raw %}{{ }}{% endraw %}` tags). In the past we've found using filters within directives such as `v-model`, `v-on`, etc led to more complexity than convenience. For list filtering on `v-for`, it's also better to move that logic into JavaScript as computed properties, so that it can be reused throughout your component.

过滤器现在只能在文本插值 (`{% raw %}{{ }}{% endraw %}` 标记)之内使用。过去我们发现在内置指令如 `v-model` , `v-on` 等中使用filter时，复杂度要大于带来的便利。如在列表中使用`v-for` 并进行过滤，更好的方式是把这部分逻辑移动到Javascript中使用computed property来实现，这样在组件中也可以复用过滤后的数据。

In general, whenever something can be achieved in plain JavaScript, we want to avoid introducing a special syntax like filters to take care of the same concern. Here's how you can replace Vue's built-in directive filters:

总之，在简单的Javascript可以实现的功能，我们都避免去引入一个特殊的语法，如filter去实现同样简单的功能。Vue的内置指令filter的替换可以参考以下例子：

#### Replacing the `debounce` Filter

#### 替换`debounce`过滤器

Instead of:

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

Use [lodash's `debounce`](https://lodash.com/docs/4.15.0#debounce) (or possibly [`throttle`](https://lodash.com/docs/4.15.0#throttle)) to directly limit calling the expensive method. You can achieve the same as above like this:

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

For more on the advantages of this strategy, see [the example here with `v-model`](#v-model-with-debounce-deprecated).

这个策略带来的好处，看[`v-model`的这个例子](#v-model-with-debounce-deprecated)了解更多。

#### Replacing the `limitBy` Filter

#### 替换 `limitBy` 过滤器

Instead of:

替换如下例子：

``` html
<p v-for="item in items | limitBy 10">{{ item }}</p>
```

Use JavaScript's built-in [`.slice` method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice#Examples) in a computed property:

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

#### Replacing the `filterBy` Filter

#### 替换 `filterBy` 过滤器

Instead of:

替换如下例子：

``` html
<p v-for="user in users | filterBy searchQuery in 'name'">{{ user.name }}</p>
```

Use JavaScript's built-in [`.filter` method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter#Examples) in a computed property:

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

JavaScript's native `.filter` can also manage much more complex filtering operations, because you have access to the full power of JavaScript within computed properties. For example, if you wanted to find all active users and case-insensitively match against both their name and email:

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

#### Replacing the `orderBy` Filter

#### 替换 `orderBy` 过滤器

Instead of:

替换如下例子：

``` html
<p v-for="user in users | orderBy 'name'">{{ user.name }}</p>
```

Use [lodash's `orderBy`](https://lodash.com/docs/4.15.0#orderBy) (or possibly [`sortBy`](https://lodash.com/docs/4.15.0#sortBy)) in a computed property:

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

You can even order by multiple columns:

你甚至可以进行多列排序：

``` js
_.orderBy(this.users, ['name', 'last_login'], ['asc', 'desc'])
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of filters being used inside directives. If you miss any, you should also see <strong>console errors</strong>.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有在内置指令上使用filters的例子。如果没有被完全替换，会看到 <strong>console errors</strong>。</p>
</div>
{% endraw %}

### Filter Argument Syntax

### 过滤器参数语法

Filters' syntax for arguments now better aligns with JavaScript function invocation. So instead of taking space-delimited arguments:

过滤器参数的语法现在和Javascript的函数调用相同。因此，如下这样空格分隔的参数：

``` html
<p>{{ date | formatDate 'YY-MM-DD' timeZone }}</p>
```

We surround the arguments with parentheses and delimit the arguments with commas:

用括号包裹，逗号分隔：

``` html
<p>{{ date | formatDate('YY-MM-DD', timeZone) }}</p>
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the old filter syntax. If you miss any, you should also see <strong>console errors</strong>.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有在内置指令上使用filters的例子。如果没有被完全替换，会看到 <strong>console errors</strong>。</p>
</div>
{% endraw %}

### Built-In Text Filters <sup>deprecated</sup>

### 内置文本过滤器 <sup>已废弃</sup>

Although filters within text interpolations are still allowed, all of the filters have been removed. Instead, it's recommended to use more specialized libraries for solving problems in each domain (e.g. [`date-fns`](https://date-fns.org/) to format dates and [`accounting`](http://openexchangerates.github.io/accounting.js/) for currencies).

尽管仍允许在文本插值中使用过滤器，所有内置过滤器都已经被移除了。推荐使用其他专门处理这些领域问题的库 (如 [`date-fns`](https://date-fns.org/) 用于格式化日期 ， [`accounting`](http://openexchangerates.github.io/accounting.js/) 用于格式化货币)。

For each of Vue's built-in text filters, we go through how you can replace them below. The example code could exist in custom helper functions, methods, or computed properties.

对于Vue内置过滤器, 我们将在下面例子中说明如何替代他们。这些示例代码可以被放在自定义的帮助函数，方法，或计算属性中。

#### Replacing the `json` Filter

#### 替换 `json` 过滤器

You actually don't need to for debugging anymore, as Vue will nicely format output for you automatically, whether it's a string, number, array, or plain object. If you want the exact same functionality as JavaScript's `JSON.stringify` though, then you can use that in a method or computed property.

实际上并不需要任何的调试，因为Vue为自动格式化输出，不管是字符串，数字，数组或简单对象。如果你需要和Javascript的 `JSON.stringify` 方法实现同样的功能，可以将它放在方法或计算属性中。

#### Replacing the `capitalize` Filter

#### 替换  `capitalize` 过滤器

``` js
text[0].toUpperCase() + text.slice(1)
```

#### Replacing the `uppercase` Filter

#### 替换  `uppercase` 过滤器

``` js
text.toUpperCase()
```

#### Replacing the `lowercase` Filter

#### 替换  `lowercase` 过滤器


``` js
text.toLowerCase()
```

#### Replacing the `pluralize` Filter

#### 替换  `pluralize` 过滤器

The [pluralize](https://www.npmjs.com/package/pluralize) package on NPM serves this purpose nicely, but if you only want to pluralize a specific word or want to have special output for cases like `0`, then you can also easily define your own pluralize functions. For example:

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

#### Replacing the `currency` Filter

#### 替换  `currency` 过滤器

For a very naive implementation, you could just do something like this:

一个非常简单的实现，可以直接这么做：

``` js
'$' + price.toFixed(2)
```

In many cases though, you'll still run into strange behavior (e.g. `0.035.toFixed(2)` rounds up to `0.4`, but `0.045` rounds down to `0.4`). To work around these issues, you can use the [`accounting`](http://openexchangerates.github.io/accounting.js/) library to more reliably format currencies.

但在很多场景中，你会遇到一些奇怪的行为 （如`0.035.toFixed(2)` 会四舍五入成 `0.4`, but `0.045` 会四舍五入成 `0.4`)。要处理这些问题，可以使用[`accounting`](http://openexchangerates.github.io/accounting.js/) 库来进行更可靠的货币格式化。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the deprecated text filters. If you miss any, you should also see <strong>console errors</strong>.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到有使用这些已废弃的text filters的例子。如果没有被完全替换，会看到 <strong>console errors</strong>。</p>
</div>
{% endraw %}

### Two-Way Filters <sup>deprecated</sup>

### 双向过滤器 <sup>已废弃</sup>

Some users have enjoyed using two-way filters with `v-model` to create interesting inputs with very little code. While _seemingly_ simple however, two-way filters can also hide a great deal of complexity - and even encourage poor UX by delaying state updates. Instead, components wrapping an input are recommended as a more explicit and feature-rich way of creating custom inputs.

As an example, we'll now walk the migration of a two-way currency filter:

<iframe width="100%" height="300" src="https://jsfiddle.net/chrisvfritz/6744xnjk/embedded/js,html,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

It mostly works well, but the delayed state updates can cause strange behavior. For example, click on the `Result` tab and try entering `9.999` into one of those inputs. When the input loses focus, its value will update to `$10.00`. When looking at the calculated total however, you'll see that `9.999` is what's stored in our data. The version of reality that the user sees is out of sync!

To start transitioning towards a more robust solution using Vue 2.0, let's first wrap this filter in a new `<currency-input>` component:

<iframe width="100%" height="300" src="https://jsfiddle.net/chrisvfritz/943zfbsh/embedded/js,html,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

This allows us add behavior that a filter alone couldn't encapsulate, such as selecting the content of an input on focus. Now the next step will be to extract the business logic from the filter. Below, we pull everything out into an external [`currencyValidator` object](https://gist.github.com/chrisvfritz/5f0a639590d6e648933416f90ba7ae4e):

<iframe width="100%" height="300" src="https://jsfiddle.net/chrisvfritz/9c32kev2/embedded/js,html,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

This increased modularity not only makes it easier to migrate to Vue 2, but also allows currency parsing and formatting to be:

- unit tested in isolation from your Vue code
- used by other parts of your application, such as to validate the payload to an API endpoint

Having this validator extracted out, we've also more comfortably built it up into a more robust solution. The state quirks have been eliminated and it's actually impossible for users to enter anything wrong, similar to what the browser's native number input tries to do.

We're still limited however, by filters and by Vue 1.0 in general, so let's complete the upgrade to Vue 2.0:

<iframe width="100%" height="300" src="https://jsfiddle.net/chrisvfritz/1oqjojjx/embedded/js,html,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

You may notice that:

- Every aspect of our input is more explicit, using lifecycle hooks and DOM events in place of the hidden behavior of two-way filters.
- We can now use `v-model` directly on our custom inputs, which is not only more consistent with normal inputs, but also means our component is Vuex-friendly.
- Since we're no longer using filter options that require a value to be returned, our currency work could actually be done asynchronously. That means if we had a lot of apps that had to work with currencies, we could easily refactor this logic into a shared microservice.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of filters used in directives like <code>v-model</code>. If you miss any, you should also see <strong>console errors</strong>.</p>
</div>
{% endraw %}

## Slots

### Duplicate Slots <sup>deprecated</sup>

It is no longer supported to have `<slot>`s with the same name in the same template. When a slot is rendered it is "used up" and cannot be rendered elsewhere in the same render tree. If you must render the same content in multiple places, pass that content as a prop.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run your end-to-end test suite or app after upgrading and look for <strong>console warnings</strong> about duplicate slots <code>v-model</code>.</p>
</div>
{% endraw %}

### `slot` Attribute Styling <sup>deprecated</sup>

Content inserted via named `<slot>` no longer preserves the `slot` attribute. Use a wrapper element to style them, or for advanced use cases, modify the inserted content programmatically using [render functions](render-function.html).

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find CSS selectors targeting named slots (e.g. <code>[slot="my-slot-name"]</code>).</p>
</div>
{% endraw %}

## Special Attributes

### `keep-alive` Attribute <sup>deprecated</sup>

`keep-alive` is no longer a special attribute, but rather a wrapper component, similar to `<transition>`. For example:

``` html
<keep-alive>
  <component v-bind:is="view"></component>
</keep-alive>
```

This makes it possible to use `<keep-alive>` on multiple conditional children:

``` html
<keep-alive>
  <todo-list v-if="todos.length > 0"></todo-list>
  <no-todos-gif v-else></no-todos-gif>
</keep-alive>
```

<p class="tip">When `<keep-alive>` has multiple children, they should eventually evaluate to a single child. Any child other than the first one will simply be ignored.</p>

When used together with `<transition>`, make sure to nest it inside:

``` html
<transition>
  <keep-alive>
    <component v-bind:is="view"></component>
  </keep-alive>
</transition>
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find <code>keep-alive</code> attributes.</p>
</div>
{% endraw %}

## Interpolation

### Interpolation within Attributes <sup>deprecated</sup>

Interpolation within attributes is no longer valid. For example:

``` html
<button class="btn btn-{{ size }}"></button>
```

Should either be updated to use an inline expression:

``` html
<button v-bind:class="'btn btn-' + size"></button>
```

Or a data/computed property:

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
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of interpolation used within attributes.</p>
</div>
{% endraw %}

### HTML Interpolation <sup>deprecated</sup>

HTML interpolations (`{% raw %}{{{ foo }}}{% endraw %}`) have been deprecated in favor of the [`v-html` directive](/api/#v-html).

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find HTML interpolations.</p>
</div>
{% endraw %}

### One-Time Bindings <sup>deprecated</sup>

One time bindings (`{% raw %}{{* foo }}{% endraw %}`) have been deprecated in favor of the new [`v-once` directive](/api/#v-once).

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find one-time bindings.</p>
</div>
{% endraw %}

## Reactivity

### `vm.$watch`

Watchers created via `vm.$watch` are now fired before the associated component rerenders. This gives you the chance to further update state before the component rerender, thus avoiding unnecessary updates. For example, you can watch a component prop and update the component's own data when the prop changes.

If you were previously relying on `vm.$watch` to do something with the DOM after a component updates, you can instead do so in the `updated` lifecycle hook.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run your end-to-end test suite, if you have one. The <strong>failed tests</strong> should alert to you to the fact that a watcher was relying on the old behavior.</p>
</div>
{% endraw %}

### `vm.$set`

The former `vm.$set` behavior has been deprecated and it is now just an alias for [`Vue.set`](/api/#Vue-set).

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the deprecated usage.</p>
</div>
{% endraw %}

### `vm.$delete`

The former `vm.$delete` behavior has been deprecated and it is now just an alias for [`Vue.delete`](/api/#Vue-delete)

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the deprecated usage.</p>
</div>
{% endraw %}

### `Array.prototype.$set`  <sup>deprecated</sup>

use Vue.set instead

(console error, migration helper)

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>.$set</code> on an array. If you miss any, you should see <strong>console errors</strong> from the missing method.</p>
</div>
{% endraw %}

### `Array.prototype.$remove` <sup>deprecated</sup>

Use `Array.prototype.splice` instead. For example:

``` js
methods: {
  removeTodo: function (todo) {
    var index = this.todos.indexOf(todo)
    this.todos.splice(index, 1)
  }
}
```

Or better yet, just pass removal methods an index:

``` js
methods: {
  removeTodo: function (index) {
    this.todos.splice(index, 1)
  }
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>.$remove</code> on an array. If you miss any, you should see <strong>console errors</strong> from the missing method.</p>
</div>
{% endraw %}

### `Vue.set` and `Vue.delete` on Vue instances <sup>deprecated</sup>

Vue.set and Vue.delete can no longer work on Vue instances. It is now mandatory to properly declare all top-level reactive properties in the data option. If you'd like to delete properties on a Vue instance or its `$data`, just set it to null.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.set</code> or <code>Vue.delete</code> on a Vue instance. If you miss any, they'll trigger <strong>console warnings</strong>.</p>
</div>
{% endraw %}

### Replacing `vm.$data` <sup>deprecated</sup>

It is now prohibited to replace a component instance's root $data. This prevents some edge cases in the reactivity system and makes the component state more predictable (especially with type-checking systems).

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of overwriting <code>vm.$data</code>. If you miss any, <strong>console warnings</strong> will be emitted.</p>
</div>
{% endraw %}

### `vm.$get` <sup>deprecated</sup>

Just retrieve reactive data directly.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$get</code>. If you miss any, you'll see <strong>console errors</strong>.</p>
</div>
{% endraw %}

## DOM-Focused Instance Methods

### `vm.$appendTo` <sup>deprecated</sup>

Use the native DOM API:

``` js
myElement.appendChild(vm.$el)
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$appendTo</code>. If you miss any, you'll see <strong>console errors</strong>.</p>
</div>
{% endraw %}

### `vm.$before` <sup>deprecated</sup>

Use the native DOM API:

``` js
myElement.parentNode.insertBefore(vm.$el, myElement)
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$before</code>. If you miss any, you'll see <strong>console errors</strong>.</p>
</div>
{% endraw %}

### `vm.$after` <sup>deprecated</sup>

Use the native DOM API:

``` js
myElement.parentNode.insertBefore(vm.$el, myElement.nextSibling)
```

Or if `myElement` is the last child:

``` js
myElement.parentNode.appendChild(vm.$el)
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$after</code>. If you miss any, you'll see <strong>console errors</strong>.</p>
</div>
{% endraw %}

### `vm.$remove` <sup>deprecated</sup>

Use the native DOM API:

``` js
vm.$el.remove()
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$remove</code>. If you miss any, you'll see <strong>console errors</strong>.</p>
</div>
{% endraw %}

## Meta Instance Methods

### `vm.$eval` <sup>deprecated</sup>

No real use. If you do happen to rely on this feature somehow and aren't sure how to work around it, post on [the forum](http://forum.vuejs.org/) for ideas.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$eval</code>. If you miss any, you'll see <strong>console errors</strong>.</p>
</div>
{% endraw %}

### `vm.$interpolate` <sup>deprecated</sup>

No real use. If you do happen to rely on this feature somehow and aren't sure how to work around it, post on [the forum](http://forum.vuejs.org/) for ideas.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$interpolate</code>. If you miss any, you'll see <strong>console errors</strong>.</p>
</div>
{% endraw %}

### `vm.$log` <sup>deprecated</sup>

Use the [Vue Devtools](https://github.com/vuejs/vue-devtools) for the optimal debugging experience.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$log</code>. If you miss any, you'll see <strong>console errors</strong>.</p>
</div>
{% endraw %}

## Instance DOM Options

### `replace: false` <sup>deprecated</sup>

Components now always replace the element they're bound to. To simulate the behavior of `replace: false`, you can wrap your root component with an element similar to the one you're replacing. For example:

``` js
new Vue({
  el: '#app',
  template: '<div id="app"> ... </div>'
})
```

Or with a render function:

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
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>replace: false</code>.</p>
</div>
{% endraw %}

## Global Config

### `Vue.config.debug` <sup>deprecated</sup>

No longer necessary, since warnings come with stack traces by default now.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.config.debug</code>.</p>
</div>
{% endraw %}

### `Vue.config.async` <sup>deprecated</sup>

Async is now required for rendering performance.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.config.async</code>.</p>
</div>
{% endraw %}

### `Vue.config.delimiters` <sup>deprecated</sup>

This has been reworked as a [component-level option](/api/#delimiters). This allows you to use alternative delimiters within your app without breaking 3rd-party components.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.config.delimiters</code>.</p>
</div>
{% endraw %}

### `Vue.config.unsafeDelimiters` <sup>deprecated</sup>

HTML interpolation has been [deprecated in favor of `v-html`](#HTML-Interpolation-deprecated).

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.config.unsafeDelimiters</code>. After this, the helper will also find instances of HTML interpolation so that you can replace them with `v-html`.</p>
</div>
{% endraw %}

## Global API

### `Vue.extend` with `el` <sup>deprecated</sup>

The el option can no longer be used in `Vue.extend`. It's only valid as an instance creation option.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run your end-to-end test suite or app after upgrading and look for <strong>console warnings</strong> about the <code>el</code> option with <code>Vue.extend</code>.</p>
</div>
{% endraw %}

### `Vue.elementDirective` <sup>deprecated</sup>

Use components instead.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.elementDirective</code>.</p>
</div>
{% endraw %}

### `Vue.partial` <sup>deprecated</sup>

Partials have been deprecated in favor of more explicit data flow between components, using props. Unless you're using a partial in a performance-critical area, the recommendation is to simply use a [normal component](components.html) instead. If you were dynamically binding the `name` of a partial, you can use a [dynamic component](http://vuejs.org/guide/components.html#Dynamic-Components).

If you happen to be using partials in a performance-critical part of your app, then you should upgrade to [functional components](render-function.html#Functional-Components). They must be in a plain JS/JSX file (rather than in a `.vue` file) and are stateless and instanceless, just like partials. This makes rendering extremely fast.

A benefit of functional components over partials is that they can be much more dynamic, because they grant you access to the full power of JavaScript. There is a cost to this power however. If you've never used a component framework with render functions before, they may take a bit longer to learn.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.partial</code>.</p>
</div>
{% endraw %}
