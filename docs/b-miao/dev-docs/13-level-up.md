---
sidebar_position: 13
---

# 升级

:::tip 提示

Next支持V3插件，但你可以阅读文档来改善代码

:::

### 差异

- 全局变量

V3中`segment`、`plugin`、`Bot`和`redis`都是全局的，

请避免生产全局对象，该行为会对环境造成污染，产生不可估计的影响

在Next,我们更推荐你从核心模块中导出。

```ts
import { Segment, Plugin, Bot } from 'yunzaijs'
import { Redis } from 'yunzaijs'
```

V3的命名是混乱的，毫无章法的

但在Next中，导出的量都尽可能的使用大写开头，而函数使用驼峰命名。

- 调用

```js title="./message.js"
import { Plugin } from 'yunzaijs'
export default class App extends plugin {
  constructor(e) {
    // 废弃，不再通过super传参
    // super({rule:[]})

    // 使用
    super()
    this.rule = [
      //
    ]

    // 废弃，不需要自己赋值
    this.e = e
  }

  async test(e) {
    // e参数废弃，其方法无法使用类型提示
    e.reply('')
    // 推荐使用
    // highlight-next-line
    this.e.reply('')
  }
}
```

```js title="./message.js"
import { Plugin } from 'yunzaijs'
export default class App extends plugin {
  constructor() {
    super()
  }
  async test() {
    // 警告性写法！！！
    // 请勿将任何非boolean值的变量让机器人接收
    return this.e.reply('')

    // 你的函数，应该是一个只返回boolean的纯函数
    // 用于控制机器人是否继续执行下一个函数

    // 正确写法，先执行发送，再结束
    // highlight-next-line
    this.e.reply('')
    return

    // 或者等待执行完毕，再结束
    await this.e.reply('')
    return
  }
}
```

```js title="./message.js"
export default class App extends plugin {
  constructor() {
    super()
  }
  async test() {
    // 返回值，必然是 bool 值，若为true才会继续匹配其他指令！
    // 这在V3中是不同的，V3默认贪婪模式。而Next当且仅当为true时，继续向后执行
    // highlight-next-line
    return true
  }
}
```

- ESM重载

Next中已删除此功能

### lib目录

:::danger 警告

lib文件夹已全部废弃，Next中不再需要关注引用路径。你只需要从对应的模块中使用原功能。模块内部标注废弃的方法都计划在未来中移除。

:::

- lib/common/common.js

```ts
// bot类方法
import * as common from 'yunzaijs'
// 工具类方法
import { sleep } from 'yunzaijs'
```

- lib/plugin/plugin.js

```ts
import { Plugin as plugin } from 'yunzaijs'
```

- lib/config/config.js

```ts
import { ConfigController as cfg } from 'yunzaijs'
```

:::tip 注意

3中puppeteer内容在Next中不再推荐使用，可阅读

[react-puppeteer](https://github.com/lemonade-lab/react-puppeteer) 了解如何使用此库开发独立于机器人的应用

:::

- lib/puppeteer/puppeteer.js

```ts
import { puppeteer } from 'yunzaijs'
```

- renderers/puppeteer/index.js

```ts
import { renderers } from 'yunzaijs'
```

- renderers/puppeteer/lib/puppeteer.js

```ts
import { Renderers } from 'yunzaijs'
```

- lib/renderer/renderer.js

```ts
import { Renderer } from 'yunzaijs'
```

- lib/renderer/loader.js

```ts
import { renderer } from 'yunzaijs'
```

### other

- plugins/other/restart.js

```ts
import { Restart } from 'yz-system'
```

### genshin

- model

修改为 lib/model

- model/mys && modle/db

修改为 @yunzaijs/mys

```ts
import {} from '@yunzaijs/mys'
```

### miao-plugin

Next仅保留了`#yunzai`本地模块

请注意自身是否依赖于喵喵插件和Miao-Yunzai本地模块，

并在说明文件中特别强调，这对用户来说至关重要

- package.json

```json
{
  // js模式的本地模块 等同 import { } from '#yunzai'
  "imports": {
    // 使用 import { } from '#yunzai'
    "#yunzai": "./lib/index.js",
    // 使用 import { } from '#miao'
    "#miao": "./plugins/miao-plugin/components/index.js",
    // 使用 import { } from '#miao.models'
    "#miao.models": "./plugins/miao-plugin/models/index.js"
  }
}
```
