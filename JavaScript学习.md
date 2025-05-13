### JavaScript 与 Java 相关概念笔记

本笔记整理了关于 JavaScript 函数、对象、类与 Java 方法、对象的区别，以及 JavaScript 代码组织方式等基础知识。

**1. JavaScript 函数 vs. Java 方法**

- **名称区别:** Java 中通常称为“方法”（Method），JavaScript 中通常称为“函数”（Function）。
- **定义语法:**
    - **Java:** 方法必须定义在类、接口或枚举中。语法严格，需要指定访问修饰符 (`public`, `private` 等)、返回类型、方法名和带参数类型的参数列表。支持方法重载。
        
        ```Java
        public class MyClass {
            public int myMethod(int param1) { ... }
        }
        ```
        
    - **JavaScript:** 函数定义更灵活。可以使用 `function` 关键字（函数声明/表达式）、箭头函数等。不需要指定返回类型或参数类型（动态类型）。不强制要求定义在类/对象中（可以是独立的）。不直接支持语法层面的函数重载。
    
        ```JavaScript
        function myFunction(param1) { ... }
        const myArrowFunction = (param1) => { ... };
        ```
        
- **类型系统:** Java 是静态类型，编译时检查；JavaScript 是动态类型，运行时检查。
- **`this` 行为:** Java 的 `this` 通常指向当前对象实例，行为相对固定；JavaScript 的 `this` 指向更动态，取决于函数调用方式（箭头函数有区别）。

**2. JavaScript 函数的特性：**

- **“一等公民”与独立性:**
    - JavaScript 函数可以独立存在，不强制依附于对象或类（尽管也可以作为对象的方法或类的成员）。
    - 函数是“一等公民”，意味着可以像普通数据类型一样对待：
        - 赋值给变量 (`const sum = add;`)。
        - 作为参数传递给其他函数（常用作回调函数）。
        - 作为另一个函数的返回值（高阶函数）。
    - `const sum = add;` 表示变量 `sum` **引用**（指向）了变量 `add` 所指向的同一个函数实体。
- **返回值:**
    - JavaScript 函数**不需要声明返回类型**。
    - 可以使用 `return 值;` 来**明确返回**任何类型的值（数字、字符串、对象、数组、`null`、`undefined`、另一个函数等）。
    - 如果函数执行完毕：
        - **没有** `return` 语句。
        - 只有 `return;` 语句（后面没有值）。
        - 函数会**隐式地、默认地返回 `undefined`**。`undefined` 表示该函数没有明确指定要返回的值。

**3. JavaScript 中的对象与函数的关系:**

- **函数本身就是对象:** 在 JavaScript 中，函数是一种特殊类型的对象。它不仅包含可执行的代码，还可以拥有属性（如 `length`, `name` 等内置属性，或自定义属性）和方法（如 `call`, `apply`, `bind` 等内置方法）。
- **对象的概念:** JavaScript 对象是键值对的无序集合。键通常是字符串（或 Symbol），值可以是任何数据类型，包括其他对象和函数。
- **方法即函数属性:** 当一个函数被赋值给对象的某个属性时，这个函数就被称为该对象的一个“方法”。
- **总结:** 函数是具备可调用能力的对象。它们既具备对象的属性和方法特性，又能被执行。

**4. JavaScript 中的类 (Class):**

- **ES6+ 引入 `class` 语法:** 现代 JavaScript (ECMAScript 2015 及以后) 提供了 `class` 关键字，用于定义类的结构，这是一种比之前的构造函数和原型更清晰、更易读的语法糖。
- **底层机制:** `class` 语法是对 JavaScript 原型继承机制的封装，本质上没有改变底层原理。
- **作用:** `class` 用于创建对象的蓝图，通过 `new ClassName()` 来创建类的实例（对象）。

**5. JavaScript 代码分多个文件的原因和意义:**

- **核心目的:** 实现**模块化 (Modularity)**。
- **具体原因:**
    - **代码组织与可读性:** 将大型代码库按功能拆分成小文件，提高代码的可读性、理解性和管理性。
    - **避免全局作用域污染:** 利用模块系统（如 ES Modules 的 `import`/`export`），每个文件形成独立作用域，避免变量/函数名冲突。
    - **代码复用:** 方便在不同地方或项目重用特定文件中的函数、对象或类。
    - **降低维护成本:** 修改特定功能时，只需关注相关文件。
    - **管理依赖:** 清晰地定义文件间的依赖关系。
    - **并行开发:** 便于团队成员同时开发不同文件。
- **总结:** 分文件是软件工程中提升代码可维护性、可读性、复用性和协作效率的基本实践，与是否使用类关联不强（尽管通常一个类会放在一个单独文件里）。

**6. ES6 是什么？如何使用？**

- **ES6:** 正式名称 ECMAScript 2015，是 JavaScript 语言标准的一个重要版本更新，引入了许多新特性（如 `let`/`const`, 箭头函数, `class`, `import`/`export`, Promise 等）。
- **使用方式:**
    - 直接在支持 ES6+ 的**现代浏览器或 Node.js** 环境中运行。
    - 对于需要兼容旧环境或使用更新特性时，使用 **Babel 等转译器**将 ES6+ 代码转换为旧版本代码。
    - 在现代前端开发中，通常结合 **Webpack 等构建工具**一起使用，构建工具会集成转译器并处理代码打包优化。
---
### 监听器
**`.addEventListener(事件类型,回调函数)`**
**事件类型 (Event Type)**。
- `"click"`：鼠标点击事件
- `"mouseover"`：鼠标悬停事件
- `"input"`：输入框内容变化事件
- `"submit"`：表单提交事件
- `"load"`：资源加载完成事件
- `"scroll"`：滚动事件

### 事件监听

**1. 什么是事件监听？为什么需要它？**

- **定义:** 事件监听是让网页元素能够“听到”用户或浏览器发生的特定动作（事件），并在事件发生时执行指定代码的机制。
- **事件类型:** 常见的事件包括 `click` (点击), `input` (输入), `mouseover` (鼠标悬停), `keydown` (按键), `submit` (表单提交) 等。
- **重要性:** 事件监听是实现网页交互性和动态效果的基础，让网页能够响应用户的操作。

**2. 如何使用事件监听 (`addEventListener`)**

- 推荐使用现代方法 `element.addEventListener(event, handler, options)`。
- **`element`:** 要监听事件的 DOM 元素。
- **`event`:** 要监听的事件类型字符串（如 `"click"`）。
- **`handler`:** 事件发生时执行的回调函数（事件处理程序）。
- **`options` (可选):** 配置选项，如是否在捕获阶段触发。
- **示例:**
    
    JavaScript
    
    ```
    const button = document.getElementById('myButton');
    button.addEventListener('click', function() {
      console.log('按钮被点击了');
    });
    ```
    
- 可以使用命名函数、匿名函数或箭头函数作为 `handler`。

**3. 事件对象 (Event Object)**

- 事件发生时，浏览器自动创建并传递给 `handler` 函数的参数。
- 包含事件的详细信息。

**4. **重点问题**: 回调函数是箭头函数时如何接收参数？**

- **回答:** 无论回调函数是普通函数、匿名函数还是箭头函数，浏览器**都会自动**将事件对象作为**第一个参数**传递给它。
- 你需要在箭头函数的参数列表中声明一个参数（通常命名为 `event` 或 `e`）来接收这个事件对象。
- **示例:**
    
    JavaScript
    
    ```
    button.addEventListener('click', (event) => { // 声明 event 参数来接收
      console.log('事件对象:', event);
      console.log('触发元素:', event.target);
    });
    ```
    
- 如果你想传递“额外”的自定义参数，需要使用闭包或 `bind()` 方法（这是一个稍微进阶的概念）。

**5. **重点问题**: 回调函数是空参时还能接收事件对象吗？**

- **回答:** 事件对象**会被传递**给函数，但如果你在函数定义时**没有声明参数来接收它**，你就**无法在函数内部通过参数名（如 `event` 或 `e`）直接访问**这个事件对象。
- 为了能在函数内部使用事件对象，**必须**在函数定义时声明一个参数来接收它。
- 这是一个良好的编码习惯，即使暂时用不到事件对象，也建议声明参数接收。

**6. 事件对象可以用来做什么？使用场景是什么？**

事件对象提供了多种属性和方法，用于获取事件的详细信息和控制事件流程：

- **`event.target`:** 触发事件的元素。
    - **场景:** 事件委托（给父元素绑定监听器，通过 `target` 判断是哪个子元素触发）、获取触发元素的属性 (ID, class, value 等)。
- **`event.type`:** 事件的类型（字符串）。
- **`event.preventDefault()`:** 阻止事件的默认行为。
    - **场景:** 阻止表单默认提交、阻止链接默认跳转、阻止右键菜单显示。
- **`event.stopPropagation()`:** 阻止事件在 DOM 树中向上（冒泡阶段）或向下（捕获阶段）传播。
    - **场景:** 隔离内层元素事件，防止触发外层元素的相同类型事件。
- **鼠标事件属性 (`event.clientX`, `event.clientY`, `event.pageX`, `event.pageY`, `event.button`):** 鼠标位置和按键信息。
    - **场景:** 实现元素拖拽、自定义上下文菜单、绘图功能。
- **键盘事件属性 (`event.key`, `event.code`, `event.keyCode`, `event.altKey`, `event.ctrlKey`, `event.metaKey`, `event.shiftKey`):** 按下的键和组合键信息。
    - **场景:** 实现键盘快捷键、输入验证、游戏控制。
- **触摸事件属性 (`event.touches`, `event.changedTouches` 等):** 触摸点信息。
    - **场景:** 移动端触摸手势处理（滑动、捏合等）。
---
### 前端与后端协作

#### 第一部分：认识 petite-vue - 轻量级的前端增强工具

##### 1. 什么是 petite-vue？

想象一下，你用 Spring Boot 在服务器上生成了一个完整的 HTML 页面。这个页面包含了文字、图片和布局，但可能缺乏一些动态的交互功能，比如点击按钮显示/隐藏内容，或者根据输入实时更新某个区域。

`petite-vue` 就是一个非常小的 JavaScript 库（只有几 KB），它的主要工作就是给这些**已经由服务器生成好的 HTML 页面**添加一些**简单的、局部的动态交互功能**。

##### 2. 它和“标准”的 Vue 有什么关系？

`petite-vue` 可以看作是标准 Vue 的一个**迷你版**。它使用了和标准 Vue **类似**的语法（比如 `{{ 数据 }}`、`@click` 等），但它的目标**不是**构建大型、复杂的“单页应用”（SPA，Single Page Application，这种应用通常整个页面内容都在浏览器里通过 JavaScript 动态生成和管理）。

它的核心理念是**“渐进式增强”（Progressive Enhancement）**：

1. 先确保你的页面在**没有 JavaScript 的情况下也能显示基本内容**（这是你 Spring Boot 后端渲染 HTML 的优势）。
2. 然后使用 `petite-vue` **选择性地**给页面的某些部分添加更高级的动态交互，提升用户体验。即使这部分 JavaScript 出错或未加载，用户仍然能看到页面的基础信息。

##### 3. petite-vue 的主要特点

- **体积小巧：** 代码量很少，加载速度快。
- **无需复杂构建：** 最简单的使用方法就是直接在 HTML 里引用一个 CDN 链接，**不需要**像标准 Vue 那样配置 Webpack、Vite 等复杂的构建工具（这对初学者很友好！）。
- **Vue 语法：** 使用 `v-scope`、`{{}}`、`@click` 等 Vue 风格的指令和模板语法。
- **针对服务器渲染页面：** 专门优化用来增强已有的 HTML 结构。

##### 4. petite-vue 如何实现“增强”？

`petite-vue` 主要通过以下方式工作：

- **标记控制区域：`v-scope`**
    - 你在 HTML 元素的标签上加上 `v-scope` 属性，告诉 `petite-vue`：“这个区域归你管了！”
    - `v-scope` 也可以用来定义这个区域内部的**本地数据**，比如 `<div v-scope="{ count: 0 }">`。
- **加载库文件：**
    - 在你的 HTML 文件中通过 `<script>` 标签引入 `petite-vue` 库。
    - 如果 script 标签有 `init` 属性 (`<script src="..." init defer>`)，`petite-vue` 会在页面加载后**自动找到所有带有 `v-scope` 的元素**并进行初始化。
    - 如果没有 `init`，你需要手动调用 `PetiteVue.createApp().mount()` 来启动它。`mount()` 可以不传参数（挂载整个页面），也可以指定一个 CSS 选择器（只挂载到特定区域）。
- **数据绑定和事件处理：**
    - 在 `v-scope` 区域内，你可以使用 `{{ 数据名 }}` 来显示数据。当数据变化时，页面显示会自动更新（**响应式**）。
    - 使用 `@事件名` 来绑定事件，比如 `@click="执行的 JavaScript 表达式"`。

##### 5. `v-effect` 指令

这是一个比较特殊的指令，用于执行一段**响应式**的代码。当代码中依赖的数据发生变化时，这段代码会自动重新运行。

- 例如：`<div v-scope="{ count: 0 }"><div v-effect="$el.textContent = count"></div><button @click="count++">++</button></div>`
    - `v-effect="$el.textContent = count"` 这段代码会在 `count` 变化时执行，将当前元素 (`$el`) 的文本内容设置为新的 `count` 值。

---

#### 第二部分：Vue 组件基础 - 数据和方法 (`data` vs `methods`)

尽管 `petite-vue` 是精简版，它共享了 Vue 的一些核心概念和语法，特别是关于如何组织**数据（状态）**和**行为（方法）**。我们来看你提供的代码片段，它展示了 Vue 组件的两个重要部分：`data` 和 `methods`。

##### 1. 什么是 Vue 组件？

可以把 Vue 组件想象成网页中一个独立的、可复用的**积木块**。每个积木块都有自己的：

- **数据 (State):** 记住自己的状态（比如一个计数器当前的值是多少，一个列表里有哪些项）。
- **模板 (Template):** 决定自己长什么样，如何展示数据。
- **方法 (Methods):** 定义自己能做什么（比如被点击时做什么，如何改变自己的数据）。

你提供的代码就是这样一个积木块的“蓝图”。

##### 2. `data` 选项：存放组件的数据状态

```JavaScript
data() {
  return {
    names: [], // 数据：一个空数组
    input: "", // 数据：一个空字符串
    // 这里放了一个函数，这是不常规的做法！
    clearAll: () => {
      this.names = [];
    },
  };
},
```

- `data` 选项是一个**函数**，这个函数需要**返回一个对象** `{}`。
- 这个对象里存放的就是组件的**响应式数据**。比如 `names` 和 `input`。
- **响应式**的意思是：当这些数据的值发生变化时，Vue 会自动更新页面中使用了这些数据的地方。
- **为什么 `data` 是一个函数并返回对象？** 这是为了确保每个组件实例都有**独立的数据副本**，避免数据互相影响。

##### 3. `methods` 选项：存放组件的方法（行为）

``` JavaScript
methods: {
  plus() { // 方法：添加数据到 names 数组
    this.names.push(this.input);
  },
  clear() { // 方法：移除 names 数组的第一个元素
    this.names.shift();
  },
  // 推荐将 clearAll 也放在这里！
  // clearAllMethods() {
  //   this.names = [];
  // }
},
```

- `methods` 选项是一个**对象**。
- 这个对象里存放的就是组件的**方法（函数）**。这些方法用来执行一些操作，比如响应用户点击、修改 `data` 里的数据、调用后端接口等。
- 在 `methods` 中定义的方法里，`this` 会**自动指向当前的组件实例**。这意味着你可以在方法里通过 `this.数据名` 来访问和修改 `data` 中的数据，或者通过 `this.其他方法名` 来调用同一个组件内的其他方法。

##### 4. `props` 选项：接收外部传入的数据

```JavaScript
props: {
  title: { // 接收一个名为 title 的数据
    type: String,
    default: "default title", // 如果外部没传，就用默认值
  },
},
```

- `props` 选项用来定义组件可以接收哪些**外部传来的数据**。
- 这就像给积木块设置了一些插孔，外部（父组件或使用这个组件的地方）可以通过这些插孔给它“喂”数据。
- 接收到的 `prop` 数据可以直接在模板中使用，也可以在组件内部通过 `this.prop名` 访问。通常 `prop` 数据不应该在组件内部直接修改，而是通过事件等方式通知外部修改。

##### 5. 对比 `data` 中的 `clearAll` 和 `methods` 中的 `clear`

你代码中一个特别的点是在 `data` 里定义了一个 `clearAll` 函数。我们来对比一下它和 `methods` 中的 `clear`：

|   |   |   |
|---|---|---|
|**特性**|**data 中的 clearAll (() => {})**|**methods 中的 clear (function() {} 或 shorthand)**|
|**位置/用途**|定义在 `data` 里，不符合 Vue 约定，`data` 应放数据|定义在 `methods` 里，标准方法定义位置，用于放行为逻辑|
|**函数类型**|**箭头函数 (`=>`)**|**标准函数**（在对象中通常写成 shorthand 形式）|
|**`this` 绑定**|**捕获外层 `this`**。因 `data()` 被调用时 `this` 是组件实例，所以箭头函数里的 `this` 也指向组件实例。|Vue **特殊处理**，确保 `this` **总是**指向组件实例。|
|**Vue 处理**|作为 `data` 对象的一个属性（其值是函数），不会被特殊处理成组件方法|被 Vue 添加到组件实例上，并确保调用时 `this` 正确绑定|
|**代码可读性**|不常规，可能让人困惑这是一个数据还是方法|清晰表明这是一个组件的方法|
|**访问方式**|`this.clearAll` 或 `@click="clearAll"`|`this.clear` 或 `@click="clear"`|
|**功能实现**|**在这个例子中**，都能修改 `this.names`（因为 `this` 都正确指向了）|**标准且推荐**的实现方法|

**核心思想：**

- 在 Vue 中，**数据（状态）**和**方法（行为）**应该被清晰地分开放在 `data` 和 `methods` 选项中。
- Vue 对 `methods` 中的函数有特殊的处理，会自动将它们的 `this` 绑定到组件实例。
- 虽然在 `data` 中使用箭头函数“碰巧”可以让 `this` 指向组件实例，但这**不符合 Vue 的约定**，不推荐这样做。将方法放在 `methods` 中是更清晰、更标准、更利于维护的方式。

##### 6. 为什么 `data` 里的函数用箭头函数“能工作”？

这是一个关于 JavaScript 中 `this` 绑定的细节：

- **标准函数**的 `this` 取决于**如何调用**它。
- **箭头函数**没有自己的 `this`，它查找**定义它时**所处的**外层（词法作用域）的 `this`**。

当 Vue 创建组件实例并执行 `data()` 函数时，Vue 内部会以某种方式调用 `data()`，使得 `data()` 函数内部的 `this` **就是那个新的组件实例**。

所以，当你像这样定义 `clearAll` 时：

```JavaScript
data() {
  // 在 data() 函数执行时，这里的 this 是组件实例
  return {
    // ...其他数据
    clearAll: () => {
      // 箭头函数没有自己的 this，它会向上查找
      // 找到了 data() 函数执行时的 this，也就是组件实例
      this.names = []; // 所以这里的 this 正确
    },
  };
}
```

箭头函数 `clearAll` 捕获了它定义时外层作用域（即 `data()` 函数执行时）的 `this`，而那个 `this` 正好是组件实例。所以它能够正确访问 `this.names`。

但是，如果你的函数定义不是箭头函数：

```JavaScript
data() {
  return {
    // ...其他数据
    // 危险！这里的 this 绑定是不确定的，取决于如何调用这个函数
    // 在这个位置定义标准函数，this 很可能不是你想要的组件实例
    wrongWay: function() {
      console.log(this); // 这个 this 可能不是组件实例！
    }
  };
}
```

这种情况下，`wrongWay` 函数内部的 `this` 就不会自动绑定到组件实例了，除非你在调用它时显式地绑定 `this`（比如 `wrongWay.call(this)`），但这非常麻烦且不是 Vue 的设计方式。

**所以，虽然箭头函数在 `data` 里“碰巧”让 `this` 工作，但它打破了 `data` 存放数据、`methods` 存放方法的约定，增加了代码理解的难度。将方法放在 `methods` 里是更可靠、更清晰的做法。**

#### 第三部分：核心概念回顾

- **服务器渲染 (Server Rendering):** 后端（如 Spring Boot）在服务器上生成完整的 HTML 页面，然后发送给浏览器。用户可以快速看到页面内容。
- **客户端 JavaScript (Client-Side JavaScript):** 在用户的浏览器中运行的 JavaScript 代码，用于增加页面的交互性、动态更新内容等。`petite-vue` 就是运行在客户端的 JavaScript 库。
- **渐进式增强 (Progressive Enhancement):** 先构建一个能工作的基本版本（无 JS），然后在此基础上增加 JS 来提升体验。`petite-vue` 很适合这种模式。
- **响应式 (Reactivity):** 数据变化后，UI 会自动更新，你不需要手动去操作 DOM (Document Object Model)。Vue (包括 petite-vue 的核心 `@vue/reactivity`) 的一个重要特性。
- **`this` 绑定：** 在 JavaScript 函数中，`this` 指向什么取决于函数的调用方式。在 Vue 组件的 `methods` 中，Vue 会自动帮你处理好 `this`，让它指向组件实例。箭头函数则根据定义时的外部作用域来确定 `this`。
---
### 路由

**核心问题：** 在不重新加载整个页面的情况下，如何根据 URL 显示不同的内容？

**两种主要的实现方法：**

1. **Hash 模式 (Hash Mode)**
2. **History 模式 (History Mode)**

**1. Hash 模式 (Hash Mode)**

- **思路概念：** 利用 URL 中的 `#` 符号后面的部分（称为 **Hash** 或 **片段标识符**）。Hash 的改变**不会**触发浏览器的整页加载，而且 Hash 部分**不会**发送给服务器。
    
- **实现方法：**
    
    - **客户端监听：** JavaScript 监听 `window` 上的 `hashchange` 事件。当 URL 的 Hash 变化时，这个事件会触发。
    - **获取路径：** 在 `hashchange` 事件处理函数中，通过 `window.location.hash` 获取当前的 Hash 值（例如 `#/about`）。
    - **映射组件：** 根据获取的 Hash 值，查找预先定义好的“路由规则”（例如 `{ '/about': AboutComponent }`），确定应该显示哪个 Vue 组件。
    - **动态渲染：** 使用 Vue 的响应式系统和动态组件 `<component :is="..." />` 来根据找到的组件更新页面显示。
- **你提供的 Vue 代码示例就是典型的 Hash 模式实现：**
    
```JavaScript
<script>
import Home from './Home.vue'
import About from './About.vue'
import NotFound from './NotFound.vue'

// 定义路由规则：将路径映射到组件
const routes = {
  '/': Home,
  '/about': About
}

export default {
  // 组件的响应式数据
  data() {
    return {
      // 初始化 currentPath 为当前的 URL Hash
      currentPath: window.location.hash
    }
  },

  // 计算属性：根据 currentPath 计算出要显示的组件
  computed: {
    currentView() {
      // 1. 获取 currentPath，并移除开头的 '#'
      // 2. 如果移除 '#' 后为空，则使用 '/' 作为默认路径
      const path = this.currentPath.slice(1) || '/'
      // 3. 在 routes 对象中查找对应的组件
      // 4. 如果找不到，则回退到 NotFound 组件
      return routes[path] || NotFound
    }
  },

  // 生命周期钩子：组件挂载到 DOM 后执行
  mounted() {
    // 监听 window 的 hashchange 事件
    window.addEventListener('hashchange', () => {
      // 当 Hash 变化时，更新 currentPath 数据
      this.currentPath = window.location.hash
    })
  }
}
</script>

<template>
  <a href="#/">Home</a> |
  <a href="#/about">About</a> |
  <a href="#/non-existent-path">Broken Link</a>

  <component :is="currentView" />
</template>

```

---

**2. Vue 组件中的关键概念 (结合代码示例)**

- **`data()`**:
    
    - **作用：** 定义组件的**响应式状态数据**。
    - **特点：** 函数形式返回一个对象，里面的属性（如 `currentPath`）会成为响应式数据。当这些数据改变时，Vue 会知道并自动更新使用到这些数据的地方（如模板或计算属性）。
- **`computed: {}`**:
    
    - **作用：** 定义**计算属性**。它的值是根据其他响应式数据**计算**得出的。
        
    - **结构：** 它是一个 JavaScript 对象 `{}`。
        
    - **里面的函数（如 `currentView()`）:**
        
        - 定义了计算该属性**值**的逻辑。
        - 是这个计算属性的 **Getter 函数**。当你访问 `this.currentView` 时，Vue 会调用这个函数来获取值。
        - **重要：通常必须有 `return` 语句！** 因为计算属性的目的是计算并“返回”一个值。
        - **不是一直在执行！** `computed` 具有**缓存**特性。它只在它**依赖的响应式数据**（这里是 `this.currentPath`）发生变化时才会重新执行。如果依赖的数据没变，即使多次访问或页面重新渲染，它会返回上一次的缓存结果，这比在模板中使用方法更高效。
    - **可以在 `computed` 中定义多个函数吗？** **可以！** `computed` 对象里可以定义任意多个计算属性，每个都有自己的名称和对应的计算函数（getter）。例如 `isHomePage() { return this.currentPath === '#/'; }`。
        
- **`mounted()`**:
    
    - **作用：** 是组件的**生命周期钩子**之一。在组件**被挂载到 DOM 后**执行。
    - **用途：** 常用于执行需要在 DOM 准备好后进行的操作，比如：
        - 添加事件监听器 (`window.addEventListener(...)`)
        - 发起网络请求获取数据
        - 操作 DOM 元素（如果需要的话）
- **`<component :is="..."/>`**:
    
    - **作用：** Vue 的一个**动态组件**。
    - **用法：** `:is` 绑定的值（例如 `currentView`）决定了实际渲染哪个组件。
    - **效果：** 当 `currentView` 的值变化时，Vue 会自动切换显示对应的组件。

---

**3. History 模式 (History Mode)**

- **思路概念：** 使用看起来“干净”的 URL，没有 `#` 符号，例如 `/about`。这依赖于现代浏览器提供的 **History API** 来改变 URL 而不触发整页加载。
    
- **核心技术：** **History API** (通过 `window.history` 对象访问)。
    
    - `history.pushState(state, title, url)`: 在浏览器历史中**添加**一个新条目。改变 URL，**不**刷新页面。
    - `history.replaceState(state, title, url)`: **替换**当前历史条目。改变 URL，**不**刷新页面。
    - `window.onpopstate` 事件：当用户点击浏览器的**前进/后退**按钮时触发。JavaScript 需要监听此事件，然后获取新的 URL 路径 (`window.location.pathname`)，并重新渲染对应组件。
- **实现方法：**
    
    - **客户端：** JavaScript 调用 `pushState` 或 `replaceState` 来改变 URL，并根据 `window.location.pathname` 渲染对应组件。同时监听 `popstate` 事件来处理前进/后退。
    - **后端（非常重要！）:** 这是 History 模式的关键区别。当用户**直接访问**一个非首页路径（如 `/about`）或在 `/about` 页面**刷新**时，浏览器会向服务器请求 `/about`。由于 `/about` 在服务器上通常不是一个实际的 HTML 文件，服务器需要配置一个**“回退”规则**：
        - 对于**所有**不匹配服务器上实际存在的静态文件（如 `.js`, `.css`, 图片等）的请求，服务器都应该**返回你的 SPA 的主 HTML 文件**（通常是 `index.html`）。
        - 这样，即使用户请求的是 `/about`，浏览器加载的还是 `index.html`。然后 SPA 的 JavaScript 运行，读取 URL (`window.location.pathname` 是 `/about`)，最后在客户端渲染出“关于我们”页面。
- **实际开发：** 在 Vue 中，通常使用官方的 **Vue Router** 库来处理前端路由。Vue Router 默认使用 History 模式，并封装了 History API 的复杂性。你只需要关注客户端的路由配置和后端服务器的回退配置即可。
    

---

**4. JavaScript 对象属性访问 (`.` vs `[]`)**

- **对象：** JavaScript 对象是键值对的集合，键通常是字符串。例如：`{ name: '张三', age: 30 }`。
    
- **点表示法 (`.`)**: `object.propertyName`
    
    - 用于访问已知属性名且属性名是合法标识符的情况。
    - 例如：`person.name`。
    - 这里的 `propertyName` 是**字面的**属性名。不能使用变量。
- **方括号表示法 (`[]`)**: `object[propertyName]`
    
    - 用于更灵活的访问。
    - `propertyName` 可以是**一个变量**，JavaScript 会使用这个变量的**值**作为键去查找属性。
    - 例如：`let key = 'name'; person[key]` (访问的就是 `person.name`)。
    - 也用于属性名不是合法标识符的情况（如含有空格、连字符）。例如：`object['my-property']`。
    - **与数组的关系：** 数组的索引访问（`array[0]`）也是方括号表示法的一种应用，因为在 JavaScript 内部，数组索引也是作为字符串键（"0"）来处理的。
- **为什么 `routes[path]`？**
    
    - 在你的代码中，`routes` 是一个对象，键是路由路径字符串（`/`, `/about`）。
    - `path` 是一个**变量**，它的值是动态变化的（来自 `currentPath` 处理后得到 `/` 或 `/about` 等）。
    - 你需要根据 `path` 这个**变量的值**来查找 `routes` 对象中对应的属性。所以**必须**使用方括号表示法 `routes[path]`。点表示法 `routes.path` 会错误地去查找一个字面量为 `"path"` 的属性。
### 路由的使用
你好！之前我们聊了 Vue 文件 (`.vue`) 是如何通过构建工具处理和在 `main.js` 中挂载到页面上的。这构成了单页应用的基础。但一个真正的应用需要能在不同“页面”之间切换，这就是“路由”的职责了。

**1. 为什么需要路由？（从传统到 SPA）**

- **传统网站:** 每个页面对应一个独立的 HTML 文件。点击链接时，浏览器向服务器请求新的 HTML，页面**完全刷新**。
- **单页应用 (SPA):** 只有一个 HTML 文件（比如 `index.html`）。点击链接时，页面**不刷新**，只更新部分内容。这种体验更流畅，像桌面应用。
- **问题：** 如何在不刷新的情况下，根据浏览器地址栏的 URL 变化，展示不同的内容？这就需要**客户端路由器**。

**2. 什么是客户端路由器？**

- 正如我们之前提到的定义：一个客户端路由器的职责就是利用诸如 History API 或是 `hashchange` 事件这样的浏览器 API 来管理应用当前应该渲染的视图。
- 在 Vue 中，我们使用官方库 **Vue Router** 来扮演这个角色。
- 它的核心工作是：**监听 URL 变化 -> 匹配 URL -> 找到对应的 Vue 组件 -> 在指定位置显示这个组件，同时隐藏旧组件。**

**3. 客户端路由器的核心机制：浏览器 API**

Vue Router 主要利用浏览器提供的两种方式来实现 URL 的变化和监听，而避免页面刷新：

- **History API (对应 History 模式)**
    
    - 使用方法：`pushState()`, `replaceState()`。这些方法可以在不刷新页面的情况下修改浏览器地址栏的 URL，并操作浏览器历史记录。
    - 监听变化：`popstate` 事件。用户点击浏览器前进/后退按钮时触发，路由器监听此事件来更新视图。
    - URL 形式：干净、传统的路径，如 `/users/123`。
    - 注意：通常需要服务器端配置，确保所有路径都返回 `index.html`。
- **Hash 模式 (`#` 和 `hashchange` 事件)**
    
    - 使用方法：修改 URL 的 `#` 后面的部分（哈希值）。
    - 监听变化：`hashchange` 事件。当 URL 的哈希值改变时触发。
    - URL 形式：带有 `#` 符号，如 `/#/users/123`。
    - 优势：不需要服务器端特殊配置。
- Vue Router 允许你选择使用 History 模式（推荐）还是 Hash 模式。
    

**4. 如何在 Vue 项目中使用 Vue Router (实现步骤)**

这是一个标准的使用流程：

**步骤 1：安装 Vue Router**

在你的项目终端中运行安装命令（需要联网）：

Bash

```
npm install vue-router@next
# 或者 yarn add vue-router@next
# 或者 pnpm add vue-router@next
```

`@next` 是为了安装 Vue 3 的兼容版本。

**步骤 2：定义路由规则 (创建路由实例)**

通常在一个新的文件里完成，比如 `src/router/index.js`。

```JavaScript
// src/router/index.js

// 导入创建路由器和历史模式的函数
import { createRouter, createWebHistory } from 'vue-router';

// 导入你需要通过路由展示的 Vue 组件（通常放在 src/views 文件夹）
import HomePage from '../views/HomePage.vue';
import AboutPage from '../views/AboutPage.vue';
import UserPage from '../views/UserPage.vue'; // 用于用户详情页

// ！！或者使用动态导入（推荐大部分页面组件这样做，实现懒加载，优化性能）
// const AboutPage = () => import('../views/AboutPage.vue');


// 1. 定义路由规则数组
// 数组中的每个对象都是一个路由规则
const routes = [
  {
    path: '/', // 当 URL 路径是 '/' 时
    name: 'Home', // 给这个路由起个名字 (可选，方便后面使用)
    component: HomePage // 显示 HomePage 组件
  },
  {
    path: '/about', // 当 URL 路径是 '/about' 时
    name: 'About',
    component: AboutPage // 显示 AboutPage 组件
  },
  {
    path: '/users/:id', // ★★ 重点：动态路径参数 ★★
    name: 'User',
    component: UserPage, // 显示 UserPage 组件
    props: true // ★★ 重点：将动态参数作为 props 传递给 UserPage 组件
  }
  // 还可以定义更多路由...
  // {
  //   path: '/:pathMatch(.*)*', // 匹配所有未匹配到的路径，常用于显示 404 页面
  //   name: 'NotFound',
  //   component: NotFoundPage
  // }
];

// 2. 创建路由器实例
const router = createRouter({
  // 使用哪种历史模式，这里选择了 History 模式
  // createWebHistory() -> URL 不带 #
  // createWebHashHistory() -> URL 带 #
  history: createWebHistory(import.meta.env.BASE_URL), // Vue Router 会使用 History API
  // history: createWebHashHistory(), // Vue Router 会监听 hashchange 事件

  routes // 将上面定义的路由规则数组传进去
});

// 3. 导出路由器实例 ★★ 理解为什么要导出：为了在 main.js 中能导入并使用它 ★★
export default router;
```

**关于动态路径参数 `path: '/users/:id'`：**

- `:` 后面的 `id` 是你给这个参数起的名字。
- 它表示 `/users/` 后面跟着的任何内容都会被 Vue Router **捕获**，并赋值给名为 `id` 的参数。
- 例如，访问 `/users/123`，参数 `id` 的值就是 `123`。访问 `/users/abc`，参数 `id` 的值就是 `"abc"`。
- 你可以在对应的组件 (`UserPage.vue`) 中通过 `$route.params.id` 获取这个值。
- 或者，如果在路由配置中设置了 `props: true`，Vue Router 会自动把 `$route.params` 中的参数作为 `props` 传递给组件，这是更推荐的方式，因为它可以解耦组件对 `$route` 的直接依赖。在 `UserPage.vue` 中像接收普通 prop 一样声明并使用 `id` 即可。

**关于为什么要导出 `export default router;`：**

- `src/router/index.js` 是一个独立的 JavaScript 模块文件。
- 你在文件内部创建了一个 `router` 常量来保存路由实例。
- 默认情况下，这个 `router` 常量是这个文件**私有**的，其他文件无法访问。
- 为了让 `main.js` 文件能够访问到这个创建好的 `router` 实例，并用 `app.use(router)` 将其添加到 Vue 应用中，你必须使用 `export default` 将它**暴露出去**。
- `export default` 表示这是这个模块的**主要导出值**。

**步骤 3：将路由实例挂载到 Vue 应用实例上**

打开你的 `main.js` 文件，导入并使用刚才创建的路由器实例。

```JavaScript
// src/main.js
import { createApp } from 'vue';
import App from './App.vue'; // 你的根组件
import router from './router'; // ★★ 导入上面创建的路由器实例 ★★

const app = createApp(App); // 创建 Vue 应用实例

// ... 其他 app.use()，比如 ElementPlus

app.use(router); // ★★ 使用 app.use() 方法将路由器“安装”到应用实例上 ★★

app.mount('#app'); // 将应用挂载到 HTML 中的 #app 元素
```

`app.use(router)` 这一步非常重要，它做了几件事：

- 使路由器实例在整个应用中可用（可以通过 `$router` 访问）。
- 注册了两个全局组件：`<router-view>` 和 `<router-link>`。
- 启动了路由的监听机制。

**关于 `createRouter` 函数的执行时机：**

- `createRouter()` 的调用代码在 `src/router/index.js` 文件中。
- 这个文件**不会自动执行**。它只有在被其他模块通过 `import` 语句**导入**时，其内部代码才会被执行。
- 在你的项目中，`main.js` 文件导入了 `src/router/index.js` (`import router from './router';`)。
- `main.js` 文件是在你的应用的入口 HTML 文件（比如 `index.html`）被浏览器加载时，通过 `<script src="...">` 标签**执行**的。

所以，`createRouter()` 函数的执行时机是：**在你的 Vue 应用启动加载 `main.js` 模块时，因为 `main.js` 依赖并导入了 `src/router/index.js`，所以 `src/router/index.js` 的代码会被执行，从而调用 `createRouter()` 创建路由实例。**

它只在应用启动时执行**一次**，用于初始化路由系统。

**步骤 4：在应用中使用路由提供的组件**

现在路由器已经在你的应用中生效了，你可以在组件的模板中使用 `<router-view>` 和 `<router-link>`。

- **`<router-view>`：路由出口/占位符**
    
    - 放在你希望匹配到的路由组件显示的位置。通常在 `App.vue` 的模板中。
    - 当 URL 匹配到某个路由规则时，该规则对应的组件就会渲染在 `<router-view>` 的位置。
    
    ```JavaScript
    <template>
      <div>
        <nav>
          <router-link to="/">首页</router-link> |
          <router-link to="/about">关于</router-link> |
          <router-link :to="{ name: 'User', params: { id: 123 } }">用户详情 123</router-link>
          </nav>
    
        <router-view></router-view>
    
        <footer>...</footer>
      </div>
    </template>
    <script>
    // ... App 组件的 script
    </script>
    ```
    
- **`<router-link>`：导航链接**
    
    - 用于创建页面间的导航链接。最终会渲染成一个 `<a>` 标签。
    - 使用 `to` 属性指定导航的目标路径。`to` 的值可以是字符串路径或路由位置对象。
    - 点击它时，Vue Router 会拦截默认的链接跳转行为，转而使用 History API 或修改哈希来实现无刷新的页面内容切换。

**回顾与联系：子组件的使用**

在我们使用 Vue Router 后，那些作为路由规则中 `component` 的 `.vue` 文件（比如 `HomePage.vue`, `UserPage.vue`），它们被渲染是因为**当前 URL 匹配到了对应的路由规则**，然后 Vue Router 指示它们在 `<router-view>` 位置进行渲染。在这个上下文中，它们可以被看作是 `<router-view>` 的“子内容”或者说是在路由出口处动态插入的组件。

这些组件本身（比如 `UserPage.vue`）仍然是可以像其他 `.vue` 文件一样，被其他组件导入并作为**常规子组件**在模板中使用的（不通过路由）。

例如，你可以在 `HomePage.vue` 的模板里写 `<SomeOtherComponent />`，这时 `SomeOtherComponent` 就是 `HomePage` 的子组件，它的渲染取决于 `HomePage` 的渲染。而 `HomePage` 的渲染取决于它是否被路由匹配并在 `<router-view>` 中显示。

所以，`.vue` 文件是组件的**定义**，它可以：

1. 作为某个路由规则的 `component`，通过路由机制在 `<router-view>` 中渲染。
2. 被其他组件导入，并在父组件的模板中作为标签使用，成为父组件的子组件。
3. 作为独立应用实例的根组件，直接 `mount` 到页面某个元素上（较少用于标准 SPA）。

这三种使用方式并不冲突，同一个组件定义可以用于不同的场景。

---
### Vue启动时的执行顺序

首先，你需要明确浏览器执行网页的顺序：

1. **浏览器总是先加载并解析 `index.html` 文件。** 这是入口点。
2. 当浏览器解析到 `index.html` 中的 `<script src="...">` 标签时，它会暂停解析 HTML（除非使用了 `async` 或 `defer` 属性），并去下载并执行这个 `src` 指向的 JavaScript 文件。

在标准的 Vue 项目中：

- `index.html` 负责提供基本的页面结构和一个挂载点 (`<div id="app"></div>`)。
- `main.js` 是 Vue 应用的 JavaScript 入口文件，负责创建 Vue 应用实例，引入根组件 (`App.vue`)，配置插件（如路由、状态管理等），并最终将应用**挂载**到 `index.html` 中指定的 DOM 元素上。
- `App.vue` 是应用的根组件，定义了应用的主体结构、逻辑和样式。它本身不是一个直接被浏览器执行的文件，而是**由 Vue 运行时处理和渲染的**。

所以，流程是这样的：

1. **浏览器加载并解析 `index.html`。** 浏览器读取 HTML 文件的内容，构建 DOM 树。
2. **浏览器遇到 `<script src="..."></script>` 标签。** 这个标签通常会指向打包工具（如 Webpack 或 Vite）生成的最终 JavaScript 文件，而这个文件的内容就是从 `main.js` 开始组织和编译的。
3. **浏览器下载并执行这个 JavaScript 文件（包含 `main.js` 的逻辑）。** 这里的执行是关键。
    - 在执行这个 JS 文件时，`main.js` 中的代码开始运行。
    - `main.js` 会创建 Vue 应用实例 (`createApp(...)`)。
    - `main.js` 会**引入并使用** `App.vue` 作为根组件的定义。
    - `main.js` 调用 `app.mount('#app')`。
4. **Vue 应用被挂载。** `app.mount('#app')` 这条指令告诉 Vue：现在，请将我这个 Vue 应用（以 `App.vue` 为根组件）的内容，渲染并控制到 `index.html` 中 id 为 `app` 的那个元素内部。
5. **Vue 渲染 `App.vue`。** Vue 接管了 `#app` 元素，开始处理 `App.vue` 组件的模板、脚本和样式，生成实际的 HTML DOM 结构，并插入到 `index.html` 中 `#app` 元素的位置。
6. **应用运行。** 现在用户在浏览器中看到了由 Vue 渲染的内容，应用开始响应交互。

**总结一下：**

- `index.html` 是被浏览器**最先加载和解析**的。
- `index.html` 自身不执行 Vue 代码，它只是**提供一个容器** (`#app`) 和**一个入口** (`<script>` 标签) 来启动 Vue 应用的 JavaScript 代码。
- `index.html` 是因为**引入了包含 `main.js` 逻辑的 JavaScript 文件**，并由这个 JS 文件执行了 `mount()` 方法，**才使得 Vue 应用得以启动并将组件渲染到 `index.html` 的特定位置上**。
- `App.vue` **不是独立执行的**，它是作为一个组件的定义，由运行起来的 Vue 应用（由 `main.js` 启动）来处理和渲染的。它的渲染发生在使用 `mount()` 之后。

所以，不是因为挂载了组件才执行 `index.html`，而是 `index.html` 被浏览器执行时，因为其中引入了脚本，**这个脚本执行了创建应用并挂载组件的操作**，从而让 Vue 应用（包括渲染 `App.vue`）跑起来。

顺序是：`index.html` (加载/解析) -> 脚本加载/执行 (从 `main.js` 开始的逻辑运行) -> Vue 应用创建/挂载 (`mount` 方法被调用) -> Vue 渲染根组件 (`App.vue` 被处理并显示在 `#app` 位置)。


### 其他
###### 便捷语法
**`div{$}*5`**
- `div`: 创建一个 `div` 元素。
- `{$}`: 这是一个**计数器占位符**。在使用 Emmet 展开时，它会被替换为一个递增的数字（默认从 1 开始）。
- `*5`: 表示将前面的元素（在这里是 `div{$}`）**重复生成 5 次**。

- `div.item$@5*3`
```html
<div class="item5"></div>
<div class="item6"></div>
<div class="item7"></div>
```

##### 执行和挂载

**Vue 执行流程：**

1. 构建工具将所有 `.vue` 文件转换为可执行的 JavaScript 模块。
2. `main.js` 作为入口文件被执行。
3. `main.js` 导入了 `App.vue` 转换后的组件定义对象。
4. `createApp(App)` 创建了以 `App` 为根组件的 Vue 应用实例。
5. `app.mount('#app')` 启动应用渲染。
6. Vue 实例开始渲染根组件 `App`。它会查找 `App` 组件定义中的模板，并执行对应的渲染函数，生成 DOM 元素。
7. 如果在 `App` 的模板中使用了其他组件（例如你在 `App.vue` 里写了 `<MyHeader/>`），Vue 会找到 `MyHeader` 组件的定义（它也是从 `MyHeader.vue` 文件构建而来），并递归地渲染 `MyHeader` 组件。
8. 这个过程会一直向下，直到渲染完所有子组件，最终在 `#app` 元素内呈现出完整的页面结构。

**总结来说，Vue 执行不同的 `.vue` 文件不是直接运行它们，而是依赖构建工具将这些 `.vue` 文件预处理成浏览器能够理解的 JavaScript 模块（包含组件定义和渲染逻辑）。`main.js` 作为入口，导入根组件（通常是 `App.vue` 处理后的结果），创建应用实例，并将其挂载到页面上。Vue 运行时根据组件定义和导入关系，负责整个组件树的渲染和管理。**

#### 逻辑运算

|                 |                |
| --------------- | -------------- |
| **真值 (Truthy)** | **假值 (Falsy)** |
| 非零数字            | undefined      |
| 非空字符串           | null           |
| true            | 0              |
|                 | '' (空字符串)      |
|                 | NaN            |
|                 | false          |

### 插件
在浏览器中**自定义显示的组件名**而不用专门写一个vue2的script
1. `npm i vite-plugin-vue-setup-extend -D` 安装依赖
2. 修改vite.config.ts配置：导包；defineConfig追加调用
3. 直接在`<script>`中写name属性=自定义名称