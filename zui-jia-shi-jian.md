---
icon: wrench
layout:
  width: default
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
---

# 最佳实践

## 1. classNames引入规则 <img src=".gitbook/assets/ekko.png" alt="ekko" data-size="line">

{% hint style="info" %}
推荐使用首字母小写的classNames
{% endhint %}

```
👍
import classNames from 'classNames'

🤔
import ClassNames from 'classNames'
```

## 2. lodash使用建议

Lodash 作为业界经典的实用工具库，以其优秀的 API 设计、完善的错误处理机制和丰富的功能特性而著称。

在大多数场景下，我们强烈推荐使用 lodash 提供的方法来提升代码质量和开发效率。

然而，在以下几种特殊情况下，我们<mark style="color:red;">**不推荐**</mark>使用 lodash：

### 2.1 get，set : 类型安全问题

```javascript
// 🤔 不推荐 - 破坏 TypeScript 类型推断
const value = get(obj, 'a.b.c'); // 类型丢失
set(obj, 'a.b.c', newValue);     // 类型不安全

// 👍 推荐 - 使用原生可选链
const value = obj?.a?.b?.c;
obj.a.b.c = newValue;
```

### 2.2 isEmpty : 预期之外的意外行为

{% hint style="info" %}
isEmpty 方法仅推荐用于对象、数组等集合类型，对于数字、日期、布尔值等基础类型，建议使用更明确的类型检查方式，避免产生与直觉不符的结果
{% endhint %}

```javascript
// 👍 推荐 - 安全的 isEmpty 使用
isEmpty({});        // true - 空对象
isEmpty([]);        // true - 空数组  
isEmpty('');        // true - 空字符串
isEmpty(null);      // true - null
isEmpty(undefined); // true - undefined

// ⛔ 禁止使用 - 可能产生意外结果
isEmpty(1);         // true - 数字1被认为是"空"
isEmpty(0);         // true - 数字0被认为是"空"
isEmpty(new Date()); // true - 日期对象被认为是"空"
isEmpty(true);      // true - 布尔值被认为是"空"
```
