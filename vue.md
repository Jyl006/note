### 路由

**使用步骤**

0. 安装依赖 -- `pnpm i vue-router`
1. 创建router文件夹并创建index.ts文件
2. index.ts文件中配置路由，如下

```javaScript
// 创建一个路由器，并暴露出去

// 第一步：引入createRouter
import { createRouter, createWebHistory } from 'vue-router'
import HomeView from '../views/HomeView.vue'

// 第二步：创建路由器
const router = createRouter({

  history: createWebHistory(import.meta.env.BASE_URL), //工作模式（history和hash）
  routes: [ // 一个一个路由规则
    {
      path: '/',
      name: 'home',
      component: HomeView,
    },
    {
      path: '/about',
      name: 'about',
      // route level code-splitting
      // this generates a separate chunk (About.[hash].js) for this route
      // which is lazy-loaded when the route is visited.
      component: () => import('../views/AboutView.vue'),
    },
  ],
})
// 第三步：将路由器暴露出去
export default router
```

3. 形成一个个路径对应的组件（路由组件）
4. 在main.ts中使用路由器 -- `app.use(router)`
5. 在组件中使用 `router-view`
	- `router-view` 是一个内置组件，它会根据当前路由路径，渲染匹配到的组件。通常，你会将它放在你的主应用组件 (`App.vue`) 或布局组件中。
6. 使用 `router-link` 进行导航
	- `router-link` 是另一个内置组件，用于创建导航链接。它会渲染为一个 `<a>` 标签，但在内部处理导航，避免浏览器重新加载页面
	```javascript
	<template>
  <div>
    <router-link to="/">去首页</router-link>

    <router-link :to="{ name: 'About' }">去关于页</router-link>

    </div>
</template>
```

---
#### router-link标签

- **是什么**: Vue Router 提供的**内置组件**，用于创建导航链接。
- **核心作用**: 实现 SPA 内的**无刷新页面跳转**，避免浏览器完全刷新页面，替代传统 `<a>` 标签。
- **使用 `router-link`**
使用 `router-link` 主要通过其 `to` 属性来指定要导航到的目标路由。
1. **使用字符串路径:** 最简单的用法是直接指定目标路径的字符串。
    ```JavaScript
    <template>
      <nav>
        <router-link to="/">首页</router-link>
    
        <router-link to="/about">关于我们</router-link>
    
        <router-link to="/users/123">用户 123</router-link>
      </nav>
    </template>
    ```
    
2. **使用路由对象 (推荐，尤其对于复杂路径):** 这种方式更灵活、更强大，特别是在路径包含参数 (`params`) 或查询字符串 (`query`) 时，或者当你使用了**命名路由**时。使用 `:to` 绑定一个 JavaScript 对象。
    - **通过路径指定:**
        ```JavaScript
        <router-link :to="{ path: '/about' }">关于我们 (对象形式)</router-link>
        ```
        
    - **通过路由名称指定 (需要你在路由配置中给路由设置 `name` 属性):** 这种方式是**推荐的**，因为它不依赖于具体的 URL 路径，如果将来修改了路由路径，只需要更新路由配置即可，`<router-link>` 不需要改动。
        ```JavaScript
        // 路由配置 (src/router/index.js)
        const routes = [
          {
            path: '/user/:id',
            name: 'UserProfile', // 给这个路由命名
            component: UserProfileComponent
          }
          // ... 其他路由
        ];
        ```
        
        ```JavaScript
        <template>
          <div>
            <router-link :to="{ name: 'UserProfile', params: { id: 456 }}">查看用户 456</router-link>
          </div>
        </template>
        ```
        
    - **传递路由参数 (`params`):** 用于动态路由匹配。注意，如果你同时使用 `path` 和 `params`，`params` 会被忽略，所以**使用命名路由 (`name`) + `params` 是标准做法**。    
        ```JavaScript
        <router-link :to="{ name: 'ProductDetail', params: { productId: 789 }}">查看产品 789</router-link>
        ```
        
    - **传递查询字符串 (`query`):** 会作为 URL 的问号参数添加到路径后面。    
        ```JavaScript
        <router-link :to="{ path: '/search', query: { keyword: 'vue', category: 'frontend' }}">搜索 Vue 前端</router-link>
        
        <router-link :to="{ name: 'SearchPage', query: { keyword: 'vue' }}">搜索</route
        ```
- **总结**: 
1. `router-link` 是 Vue Router 用于页面导航的组件。
2. 它防止页面刷新，提供 SPA 的无缝跳转。
3. 它会自动为活跃链接添加样式类。
4. 主要通过 `to` 属性指定目标路由，可以使用字符串路径或更灵活的路由对象（推荐使用命名路由 + 对象形式）。

#### Vue Router 工作模式

决定 URL 外观及服务器配置需求：

1. **Hash 模式**:
    - 特点：URL 中包含 **#** 符号 (`example.com/#/about`)。
    - 优点：**无需服务器配置**，兼容性强。
    - **缺点：**URL不美观，**不利于 SEO**
    - 使用 `createWebHashHistory()` 创建。
2. **History 模式**:
    - 优点：URL干净，像传统网页 (`example.com/about`)。**更利于SEO**
    - **缺点：**需要服务器配置**，将所有路径重定向到 `index.html` (后备路由)，以防刷新或直访 404。**兼容性:** 只支持支持 HTML5 History API 的浏览器。
    - 使用 `createWebHistory()` 创建。
    - **注意：**`createWebHistory` 可以接受一个可选的 `base` 参数，用于指定应用的基路径，例如 `createWebHistory('/my-app/')`，这在你将应用部署在服务器的子目录下时很有用。

**选择**: 推荐History模式更符合现代开发，无法配置服务器重定向或者考虑浏览器兼容则用Hash模式

```JavaScript
import { createRouter, createWebHashHistory } from 'vue-router';

const router = createRouter({
  // history: createWebHashHistory(), // 使用 Hash 模式
  history: createWebHistory(), // 使用History模式
  routes: [...]
});
```

