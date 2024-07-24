---
sidebar_position: 4
---

# 事件

:::tip 提示

Yunzai 基于 Icqq的事件进行扩展，部分类型可能未进行补充，会有类型爆红

:::

## 调用

### 群聊

- 回复

```ts
import { Messages } from 'yunzai'
const message = new Messages('message.group')
message.use(
  e => {
    e.reply('你好')
  },
  [/^(#|\/)?你好/]
)
```

- 图片

```ts
import { Messages, Segment } from 'yunzai'
const message = new Messages('message.group')
message.use(
  e => {
    const img: Buffer | null = null
    e.reply(Segment.image(img))
  },
  [/^(#|\/)?你好/]
)
```

- 复合

```ts
import { Messages, Segment } from 'yunzai'
const message = new Messages('message.group')
message.use(
  e => {
    const img: Buffer | null = null
    e.reply(['这是一张图片', Segment.image(img)])
  },
  [/^(#|\/)?你好/]
)
```

### 私聊

- 回复

```ts
import { Messages } from 'yunzai'
const message = new Messages('message.private')
message.response(
  e => {
    e.reply('你好')
  },
  [/^(#|\/)?你好/]
)
```

## 属性

### 群聊

```ts
export interface EventType {
  /**
   * 是否是主人
   */
  isMaster: boolean
  /**
   * 是否是群里
   */
  isGroup: boolean
  /**
   * 是私聊
   */
  isPrivate?: any
  /**
   * 是频道
   */
  isGuild?: any
  /**
   * 用户消息
   */
  msg: string
  /**
   * 用户编号
   */
  user_id: string
  /**
   * 用户名
   */
  user_name: string
  /**
   * 用户头像
   */
  user_avatar: string
  /**
   * 群号
   */
  group_id: number
  /**
   * 群名
   */
  group_name: string
  /**
   *  群头像
   */
  group_avatar: string
}
```

### 私聊

```ts
export interface EventType {
  /**
   * 是否是主人
   */
  isMaster: boolean
  /**
   * 是否是群里
   */
  isGroup: boolean
  /**
   * 是私聊
   */
  isPrivate?: any
  /**
   * 是频道
   */
  isGuild?: any
  /**
   * 用户消息
   */
  msg: string
  /**
   * 用户编号
   */
  user_id: string
  /**
   * 用户名
   */
  user_name: string
  /**
   * 用户头像
   */
  user_avatar: string
}
```
