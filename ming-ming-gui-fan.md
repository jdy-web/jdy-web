---
layout:
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
---

# 命名规范

## 1. 原则

### 1.1. 名副其实

{% hint style="info" %}
变量名能够解释自身的含义，而不是通过各种注释加以补充
{% endhint %}

```
✅
let elapsedTimeInDays;

⛔
let d; // elapsed time in days
```

### 1.2. 避免误导

{% hint style="info" %}
避免在环境中命名专有名词

避免使用外形相似度过高的名称
{% endhint %}

```
⛔
const props = {};

⛔
let XYZControllerForEfficientHandlingOfStrings;
let XZYControllerForEfficientStorageOfStrings;
```



