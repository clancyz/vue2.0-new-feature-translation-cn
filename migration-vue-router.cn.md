---
title: Migration from Vue Router 0.7.x
type: guide
order: 25
---

> 只有Vue Router 2和Vue 2是兼容的，所以如果更新了Vue,你也必须更新Vue Router。我们在主文档中会包含详细的迁移路线。如果想看新的Vue Router完全版的指南，请看[Vue Router文档](http://router.vuejs.org/en/)。

<p class="tip">下面被提及的废弃内容已经全部完成，但是migration helper还在更新中。</p>

## 路由定义

### `router.map` <sup>已废弃</sup>

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

将会被替代为：

``` js
var router = new VueRouter({
  routes: [
    { path: '/foo', component: Foo },
    { path: '/bar', component: Bar }
  ]
})
```

数组语法让路由匹配更加可预期，因为在对象上进行迭代在不同的浏览上不能保证用同样的key。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>router.map</code>的例子。</p>
</div>
{% endraw %}

### `router.on` <sup>已废弃</sup>

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
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>router.on<</code>的调用例子。</p>
</div>
{% endraw %}

### `subroutes` <sup>已废弃</sup>

[重命名为 `children`](http://router.vuejs.org/en/essentials/nested-routes.html) , 这是为了和其他的路由库保持一致。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>subroutes</code>选项的例子。</p>
</div>
{% endraw %}

### `router.redirect` <sup>已废弃</sup>

这现在是一个 [路由定义的选项](http://router.vuejs.org/en/essentials/redirect-and-alias.html)。所以，如下的例子：

``` js
router.redirect({
  '/tos': '/terms-of-service'
})
```

可以更新为如下的`路由`配置项：

``` js
{
  path: '/tos',
  redirect: '/terms-of-service'
}
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>router.redirect</code>的调用例子。</p>
</div>
{% endraw %}

### `router.alias` <sup>已废弃</sup>

这现在是一个 [路由定义的选项](http://router.vuejs.org/en/essentials/redirect-and-alias.html)。所以，如下的例子：

``` js
router.alias({
  '/manage': '/admin'
})
```

可以更新为如下的`路由`配置项：

``` js
{
  path: '/admin',
  component: AdminPanel,
  alias: '/manage'
}
```

如果需要更多别名，同样可以使用一个数组来表示：

``` js
alias: ['/manage', '/administer', '/administrate']
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>router.alias</code>的调用例子。</p>
</div>
{% endraw %}

## 路由匹配

路由匹配现在在内部使用 [路径-正则](https://github.com/pillarjs/path-to-regexp)来匹配，比起老版本更加灵活。

### 一或多个具名参数

这个语法做了轻微的改动，举例， `/category/*tags` 需要改成  `/category/:tags+` 。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于这些旧的路由语法的例子。</p>
</div>
{% endraw %}

## 链接

### `v-link` <sup>已废弃</sup>

`v-link` 这个指令已经被替换为一个新的 [`<router-link>` 组件](http://router.vuejs.org/en/api/router-link.html) ， 因为在Vue 2中这样的功能已经完全由组件来实现。也就是说，在你的项目中如果有个链接像这样：

``` html
<a v-link="'/about'">About</a>
```

需要修改成这样：

``` html
<router-link to="/about">About</router-link>
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>v-link</code>指令的使用例子。</p>
</div>
{% endraw %}

### `v-link-active` <sup>已废弃</sup>

 `v-link-active` 指令也被废弃了，因为可以在[ `<router-link>` 组件](http://router.vuejs.org/en/api/router-link.html) 中声明一个单独的tag。所以，像下面这个例子：

``` html
<li v-link-active>
  <a v-link="'/about'">About</a>
</li>
```

需要修改为：

``` html
<router-link tag="li" to="/about">
  <a>About</a>
</router-link>
```

这个 `<a>` 标签是实际的链接（也会得到正确的链接地址），但是active class会被应用到外部的包裹元素 `<li>` 上。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>v-link-active</code>指令的使用例子。</p>
</div>
{% endraw %}

## 使用程序进行路由跳转

为了与 [HTML5 History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API)中的术语一致，`router.go`已被[重命名为 `router.push`](http://router.vuejs.org/en/essentials/navigation.html#routerpushlocation)。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>router.go</code>的调用例子。</p>
</div>
</div>
{% endraw %}

## 路由选项： Modes

### `hashbang: false` <sup>已废弃</sup>

Google的URL爬虫已经不再需要Hashbangs， 所以它们不再是哈希策略的默认选项（甚至已经不是一个选项）。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>hashbang: false</code>的选项。</p>
</div>
{% endraw %}

### `history: true` <sup>已废弃</sup>

所有路由模式选项现在已经被精简显一个单独的 [`mode` 选项](http://router.vuejs.org/en/api/options.html#mode)。如下：

``` js
var router = new VueRouter({
  history: 'true'
})
```

需要修改为：

``` js
var router = new VueRouter({
  mode: 'history'
})
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>history: true</code>的选项。</p>
</div>
{% endraw %}

### `abstract: true` <sup>已废弃</sup>

所有路由模式选项现在已经被精简显一个单独的 [`mode` 选项](http://router.vuejs.org/en/api/options.html#mode)。如下：

``` js
var router = new VueRouter({
  abstract: 'true'
})
```

需要修改为：

``` js
var router = new VueRouter({
  mode: 'abstract'
})
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>abstract: true</code>的选项。</p>
</div>
{% endraw %}

## 路由选项：杂项

### `saveScrollPosition` <sup>已废弃</sup>

此选项已经被替换为[`scrollBehavior` 选项](http://router.vuejs.org/en/advanced/scroll-behavior.html)。此选项需要传入一个函数，这样页面滚动的行为可以完全自定义 - 甚至是在每个路由上。这个改动带来了很多的可能性，如下：

``` js
saveScrollPosition: true
```

只要简单地替换为：

``` js
scrollBehavior: function (to, from, savedPosition) {
  return savedPosition || { x: 0, y: 0 }
}
```

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>saveScrollPosition: true</code>的选项。</p>
</div>
{% endraw %}

### `root` <sup>已废弃</sup>

为了保持和[HTML `<base>` 元素](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/base)一致，此选项已被重命名为 `base`。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>root</code>的选项。</p>
</div>
{% endraw %}

### `transitionOnLoad` <sup>已废弃</sup>

这个选项已经不再必要，因为Vue的过渡系统现在已经有明确的[`appear` transition 控制](transitions.html#Transitions-on-Initial-Render)。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>transitionOnLoad: true</code>的选项。</p>
</div>
{% endraw %}

### `suppressTransitionError` <sup>已废弃</sup>

由于路由钩子的精简，该选项已经被废弃。如果你需要抹掉transition errors, 可以使用[`try`...`catch`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch)来替代。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>suppressTransitionError: true</code>的选项。</p>
</div>
{% endraw %}

## 路由钩子

### `activate` <sup>已废弃</sup>

在组件中使用[`beforeRouteEnter`](http://router.vuejs.org/en/advanced/navigation-guards.html#incomponent-guards) 来替代。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>beforeRouteEnter</code>的钩子。</p>
</div>
{% endraw %}

### `canActivate` <sup>已废弃</sup>

在路由中使用[`beforeEnter`](http://router.vuejs.org/en/advanced/navigation-guards.html#perroute-guard) 来替代。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>canActivate</code>的钩子。</p>
</div>
{% endraw %}

### `deactivate` <sup>已废弃</sup>

在组件中使用[`beforeDestroy`](/api/#beforeDestroy) 或 [`destroyed`](/api/#destroyed) 钩子来替代。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>deactivate</code>的钩子。</p>
</div>
{% endraw %}

### `canDeactivate` <sup>已废弃</sup>

在组件中使用[`beforeRouteLeave`](http://router.vuejs.org/en/advanced/navigation-guards.html#incomponent-guards) 钩子来替代。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>canDeactivate</code>的钩子。</p>
</div>
{% endraw %}

### `canReuse: false` <sup>已废弃</sup>

新的Vue Router已经不再使用此选项。

{% raw %}
<div class="upgrade-path">
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>canReuse: false</code>的选项。</p>
</div>
{% endraw %}

### `data` <sup>已废弃</sup>

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
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>data</code>钩子的例子。</p>
</div>
{% endraw %}

### `$loadingRouteData` <sup>已废弃</sup>

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
  <h4>升级路线</h4>
  <p>在你的项目下运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>，找到关于 <code>$loadingRouteData</code>这个元属性。</p>
</div>
{% endraw %}
