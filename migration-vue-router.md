---
title: Migration from Vue Router 0.7.x
type: guide
order: 25
---

> Only Vue Router 2 is compatible with Vue 2, so if you're updating Vue, you'll have to update Vue Router as well. That's why we've included details on the migration path here in the main docs. For a complete guide on using the new Vue Router, see the [Vue Router docs](http://router.vuejs.org/en/).

> 只有Vue Router 2和Vue 2是兼容的，所以如果更新了Vue,你也必须更新Vue Router。我们在主文档中会包含详细的迁移路线。如果想看新的Vue Router完全版的指南，请看[Vue Router文档](http://router.vuejs.org/en/)。

<p class="tip">The list of deprecations below should be relatively complete, but the migration helper is still being updated to catch them.</p>

<p class="tip">下面被提及的废弃内容已经全部完成，但是migration helper还在更新中。</p>

## Route Definitions

## 路由定义

### `router.map` <sup>deprecated</sup>

### `router.map` <sup>已废弃</sup>

Routes are now defined as an array on a [`routes` option](http://router.vuejs.org/en/essentials/getting-started.html#javascript) at router instantiation. So these routes for example:

路由现在在路由实例的[路由选项](http://router.vuejs.org/en/essentials/getting-started.html#javascript)中被定义为一个数组。举例，下面这些路由的写法：

``` js
router.map({
  '/foo': {
    component: Foo
  },
  '/bar': {
    component: Bar
  }
})
```

Will instead be defined with:

将会被替代为：

``` js
var router = new VueRouter({
  routes: [
    { path: '/foo', component: Foo },
    { path: '/bar', component: Bar }
  ]
})
```

The array syntax allows more predictable route matching, since iterating over an object is not guaranteed to use the same key order across browsers.

数组语法让路由匹配更加可预期，因为在对象上进行迭代在不同的浏览上不能保证用同样的key。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>router.map</code> being called.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>router.map</code>的例子。</p>
</div>
{% endraw %}

### `router.on` <sup>deprecated</sup>

### `router.on` <sup>已废弃</sup>

If you need to programmatically generate routes when starting up your app, you can do so by dynamically pushing definitions to a routes array. For example:

如果需要在应用开始时使用程序来生成路由，你也可以动态地把这些路由定义push到路由数组中。举例：

``` js
// 基础路由数组
var routes = [
  // ...
]

// 动态生成路由
marketingPages.forEach(function (page) {
  routes.push({
    path: '/marketing/' + page.slug
    component: {
      extends: MarketingComponent
      data: function () {
        return { page: page }
      }
    }
  })
})

var router = new Router({
  routes: routes
})
```

If you need to add new routes after the router has been instantiated, you can replace the router's matcher with a new one that includes the route you'd like to add:

如果需要在路由初始化完成后添加新的路由，需要用一个包含你想添加的路由的matcher来替换原有路由的matcher：

``` js
router.match = createMatcher(
  [{
    path: '/my/new/path',
    component: MyComponent
  }].concat(router.options.routes)
)
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>router.on</code> being called.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>router.on<</code>的调用例子。</p>
</div>
{% endraw %}

### `subroutes` <sup>deprecated</sup>

### `subroutes` <sup>已废弃</sup>

[Renamed to `children`](http://router.vuejs.org/en/essentials/nested-routes.html) for consistency within Vue and with other routing libraries.

[重命名为 `children`](http://router.vuejs.org/en/essentials/nested-routes.html) , 这是为了和其他的路由库保持一致。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>subroutes</code> option.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>subroutes</code>选项的例子。</p>
</div>
{% endraw %}

### `router.redirect` <sup>deprecated</sup>

### `router.redirect` <sup>已废弃</sup>

This is now an [option on route definitions](http://router.vuejs.org/en/essentials/redirect-and-alias.html). So for example, you will update:

这现在是一个 [路由定义的选项](http://router.vuejs.org/en/essentials/redirect-and-alias.html)。所以，如下的例子：

``` js
router.redirect({
  '/tos': '/terms-of-service'
})
```

to a definition like below in your `routes` configuration:

可以更新为如下的`路由`配置项：

``` js
{
  path: '/tos',
  redirect: '/terms-of-service'
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>router.redirect</code> being called.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>router.redirect</code>的调用例子。</p>
</div>
{% endraw %}

### `router.alias` <sup>deprecated</sup>

### `router.alias` <sup>已废弃</sup>

This is now an [option on the definition for the route](http://router.vuejs.org/en/essentials/redirect-and-alias.html) you'd like to alias to. So for example, you will update:

这现在是一个 [路由定义的选项](http://router.vuejs.org/en/essentials/redirect-and-alias.html)。所以，如下的例子：

``` js
router.alias({
  '/manage': '/admin'
})
```

to a definition like below in your `routes` configuration:

可以更新为如下的`路由`配置项：

``` js
{
  path: '/admin',
  component: AdminPanel,
  alias: '/manage'
}
```

If you need multiple aliases, you can also use an array syntax:

如果需要更多别名，同样可以使用一个数组来表示：

``` js
alias: ['/manage', '/administer', '/administrate']
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>router.alias</code> being called.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>router.alias</code>的调用例子。</p>
</div>
{% endraw %}

## Route Matching

## 路由匹配

Route matching now uses [path-to-regexp](https://github.com/pillarjs/path-to-regexp) under the hood, making it much more flexible than previously.

路由匹配现在在内部使用 [路径-正则](https://github.com/pillarjs/path-to-regexp)来匹配，比起老版本更加灵活。

### One or More Named Parameters

### 一或多个具名参数

The syntax has changed slightly, so `/category/*tags` for example, should be updated to `/category/:tags+`.

这个语法做了轻微的改动，举例， `/category/*tags` 需要改成  `/category/:tags+` 。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the deprecated route syntax.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于这些旧的路由语法的例子。</p>
</div>
{% endraw %}

## Links

## 链接

### `v-link` <sup>deprecated</sup>

### `v-link` <sup>已废弃</sup>

The `v-link` directive has been replaced with a new [`<router-link>` component](http://router.vuejs.org/en/api/router-link.html), as this sort of job is now solely the responsibility of components in Vue 2. That means whenever wherever you have a link like this:

`v-link` 这个指令已经被替换为一个新的 [`<router-link>` 组件](http://router.vuejs.org/en/api/router-link.html) ， 因为在Vue 2中这样的功能已经完全由组件来实现。也就是说，在你的项目中如果有个链接像这样：

``` html
<a v-link="'/about'">About</a>
```

You'll need to update it like this:

需要修改成这样：

``` html
<router-link to="/about">About</router-link>
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>v-link</code> directive.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>v-link</code>指令的使用例子。</p>
</div>
{% endraw %}

### `v-link-active` <sup>deprecated</sup>

### `v-link-active` <sup>已废弃</sup>

The `v-link-active` directive has also been deprecated in favor of specifying a separate tag on [the `<router-link>` component](http://router.vuejs.org/en/api/router-link.html). So for example, you'll update this:

 `v-link-active` 指令也被废弃了，因为可以在[ `<router-link>` 组件](http://router.vuejs.org/en/api/router-link.html) 中声明一个单独的tag。所以，像下面这个例子：

``` html
<li v-link-active>
  <a v-link="'/about'">About</a>
</li>
```

to this:

需要修改为：

``` html
<router-link tag="li" to="/about">
  <a>About</a>
</router-link>
```

The `<a>` will be the actual link (and will get the correct href), but the active class will be applied to the outer `<li>`.

这个 `<a>` 标签是实际的链接（也会得到正确的链接地址），但是active class会被应用到外部的包裹元素 `<li>` 上。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>v-link-active</code> directive.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>v-link-active</code>指令的使用例子。</p>
</div>
{% endraw %}

## Programmatic Navigation

## 使用程序进行路由跳转

### `router.go`

[Renamed to `router.push`](http://router.vuejs.org/en/essentials/navigation.html#routerpushlocation) for consistency with terminology used in the [HTML5 History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API).

为了与 [HTML5 History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API)中的术语一致，`router.go`已被[重命名为 `router.push`](http://router.vuejs.org/en/essentials/navigation.html#routerpushlocation)。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>router.go</code> being called.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>router.go</code>的调用例子。</p>
</div>
</div>
{% endraw %}

## Router Options: Modes

## 路由选项： Modes

### `hashbang: false` <sup>deprecated</sup>

### `hashbang: false` <sup>已废弃</sup>

Hashbangs are no longer required for Google to crawl a URL, so they are no longer the default (or even an option) for the hash strategy.

Google的URL爬虫已经不再需要Hashbangs， 所以它们不再是哈希策略的默认选项（甚至已经不是一个选项）。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>hashbang: false</code> option.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>hashbang: false</code>的选项。</p>
</div>
{% endraw %}

### `history: true` <sup>deprecated</sup>

### `history: true` <sup>已废弃</sup>

All routing mode options have been condensed into a single [`mode` option](http://router.vuejs.org/en/api/options.html#mode). Update:

所有路由模式选项现在已经被精简显一个单独的 [`mode` 选项](http://router.vuejs.org/en/api/options.html#mode)。如下：

``` js
var router = new VueRouter({
  history: 'true'
})
```

to:

需要修改为：

``` js
var router = new VueRouter({
  mode: 'history'
})
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>history: true</code> option.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>history: true</code>的选项。</p>
</div>
{% endraw %}

### `abstract: true` <sup>deprecated</sup>

### `abstract: true` <sup>已废弃</sup>

All routing mode options have been condensed into a single [`mode` option](http://router.vuejs.org/en/api/options.html#mode). Update:

所有路由模式选项现在已经被精简显一个单独的 [`mode` 选项](http://router.vuejs.org/en/api/options.html#mode)。如下：

``` js
var router = new VueRouter({
  abstract: 'true'
})
```

to:

需要修改为：

``` js
var router = new VueRouter({
  mode: 'abstract'
})
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>abstract: true</code> option.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>abstract: true</code>的选项。</p>
</div>
{% endraw %}

## Route Options: Misc

## 路由选项：杂项

### `saveScrollPosition` <sup>deprecated</sup>

### `saveScrollPosition` <sup>已废弃</sup>

This has been replaced with a [`scrollBehavior` option](http://router.vuejs.org/en/advanced/scroll-behavior.html) that accepts a function, so that the scroll behavior is completely customizable - even per route. This opens many new possibilities, but to simply replicate the old behavior of:

此选项已经被替换为[`scrollBehavior` 选项](http://router.vuejs.org/en/advanced/scroll-behavior.html)。此选项需要传入一个函数，这样页面滚动的行为可以完全自定义 - 甚至是在每个路由上。这个改动带来了很多的可能性，如下：

``` js
saveScrollPosition: true
```

You can replace it with:

只要简单地替换为：

``` js
scrollBehavior: function (to, from, savedPosition) {
  return savedPosition || { x: 0, y: 0 }
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>saveScrollPosition: true</code> option.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>saveScrollPosition: true</code>的选项。</p>
</div>
{% endraw %}

### `root` <sup>deprecated</sup>

### `root` <sup>已废弃</sup>

Renamed to `base` for consistency with [the HTML `<base>` element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/base).

为了保持和[HTML `<base>` 元素](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/base)一致，此选项已被重命名为 `base`。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>root</code> option.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>root</code>的选项。</p>
</div>
{% endraw %}

### `transitionOnLoad` <sup>deprecated</sup>

### `transitionOnLoad` <sup>已废弃</sup>

This option is no longer necessary now that Vue's transition system has explicit [`appear` transition control](transitions.html#Transitions-on-Initial-Render).

这个选项已经不再必要，因为Vue的过渡系统现在已经有明确的[`appear` transition 控制](transitions.html#Transitions-on-Initial-Render)。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>transitionOnLoad: true</code> option.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>transitionOnLoad: true</code>的选项。</p>
</div>
{% endraw %}

### `suppressTransitionError` <sup>deprecated</sup>

### `suppressTransitionError` <sup>已废弃</sup>

Removed due to hooks simplification. If you really must suppress transition errors, you can use [`try`...`catch`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch) instead.

由于路由钩子的精简，该选项已经被废弃。如果你需要抹掉transition errors, 可以使用[`try`...`catch`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch)来替代。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>suppressTransitionError: true</code> option.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>suppressTransitionError: true</code>的选项。</p>
</div>
{% endraw %}

## Route Hooks

## 路由钩子

### `activate` <sup>deprecated</sup>

### `activate` <sup>已废弃</sup>

Use [`beforeRouteEnter`](http://router.vuejs.org/en/advanced/navigation-guards.html#incomponent-guards) in the component instead.

在组件中使用[`beforeRouteEnter`](http://router.vuejs.org/en/advanced/navigation-guards.html#incomponent-guards) 来替代。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>beforeRouteEnter</code> hook.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>beforeRouteEnter</code>的钩子。</p>
</div>
{% endraw %}

### `canActivate` <sup>deprecated</sup>

### `canActivate` <sup>已废弃</sup>

Use [`beforeEnter`](http://router.vuejs.org/en/advanced/navigation-guards.html#perroute-guard) in the route instead.

在路由中使用[`beforeEnter`](http://router.vuejs.org/en/advanced/navigation-guards.html#perroute-guard) 来替代。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>canActivate</code> hook.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>canActivate</code>的钩子。</p>
</div>
{% endraw %}

### `deactivate` <sup>deprecated</sup>

### `deactivate` <sup>已废弃</sup>

Use the component's [`beforeDestroy`](/api/#beforeDestroy) or [`destroyed`](/api/#destroyed) hooks instead.

在组件中使用[`beforeDestroy`](/api/#beforeDestroy) 或 [`destroyed`](/api/#destroyed) 钩子来替代。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>deactivate</code> hook.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>deactivate</code>的钩子。</p>
</div>
{% endraw %}

### `canDeactivate` <sup>deprecated</sup>

### `canDeactivate` <sup>已废弃</sup>

Use [`beforeRouteLeave`](http://router.vuejs.org/en/advanced/navigation-guards.html#incomponent-guards) in the component instead.

在组件中使用[`beforeRouteLeave`](http://router.vuejs.org/en/advanced/navigation-guards.html#incomponent-guards) 钩子来替代。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>canDeactivate</code> hook.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>canDeactivate</code>的钩子。</p>
</div>
{% endraw %}

### `canReuse: false` <sup>deprecated</sup>

### `canReuse: false` <sup>已废弃</sup>

There's no longer a use case for this in the new Vue Router.

新的Vue Router已经不再使用此选项。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>canReuse: false</code> option.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>canReuse: false</code>的选项。</p>
</div>
{% endraw %}

### `data` <sup>deprecated</sup>

### `data` <sup>已废弃</sup>

The `$route` property is reactive, so you can just use a watcher to react to route changes, like this:

 `$route` 属性现在是响应式的，所以你可以使用一个监听器来响应路由变化，像这样：

``` js
watch: {
  '$route': 'fetchData'
},
methods: {
  fetchData: function () {
    // ...
  }
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>data</code> hook.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>data</code>钩子的例子。</p>
</div>
{% endraw %}

### `$loadingRouteData` <sup>deprecated</sup>

### `$loadingRouteData` <sup>已废弃</sup>

Define your own property (e.g. `isLoading`), then update the loading state in a watcher on the route. For example, if fetching data with [axios](https://github.com/mzabriskie/axios):

定义你自己的属性 (如 `isLoading`)，然后在路由监听器中更新这个loading的状态。举例，从[axios](https://github.com/mzabriskie/axios)中获取数据：

``` js
data: function () {
  return {
    posts: [],
    isLoading: false,
    fetchError: null
  }
},
watch: {
  '$route': function () {
    this.isLoading = true
    this.fetchData().then(() => {
      this.isLoading = false
    })
  }
},
methods: {
  fetchData: function () {
    return axios.get('/api/posts')
      .then(function (response) {
        this.posts = response.data.posts
      })
      .catch(function (error) {
        this.fetchError = error
      })
  }
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>$loadingRouteData</code> meta property.</p>
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>$loadingRouteData</code>这个元属性。</p>
</div>
{% endraw %}
