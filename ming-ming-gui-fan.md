---
icon: '0'
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

### 1.3. 有意义的命名

{% hint style="info" %}
避免丝毫无异议的命名

避免无意义的冗余
{% endhint %}

```
✅
function copyArray(source: string[], destination: string[]) {
    for (let i = 0; i < source.length; i++) {
        destination[i] = source[i];
    }
}

⛔
function copyArray(a1: string[], a2: string[]) {
    for (let i = 0; i < a1.length; i++) {
        a2[i] = a1[i];
    }
}
```

## 2. 最佳实践

### 2.1. 组件命名规则

{% hint style="info" %}
组件名采用帕斯卡命名法(单词相连，单词首字母大写)
{% endhint %}

```
✅
AppSet
```

***

{% hint style="info" %}
组件属性 使用名词或者名词短语命名
{% endhint %}

```
✅
name、isEnable
```

***

{% hint style="info" %}
组件对外回调使用 on + 名词或者名词短语（可省略） + 动词 命名
{% endhint %}

```
✅
onChange、onButtonClick
```

***

{% hint style="info" %}
组件回调事件处理函数 使用 handle + 名词或名词短语（可省略） + 动词，并与属性相匹配
{% endhint %}

```
✅
handleChange、handleButtonClick
```

***

{% hint style="info" %}
组件自定义数据获取属性 使用 on + 名词或名词短语 + Getter 命名
{% endhint %}

```
✅
onFieldGetter、onEditUrlGetter
```

***

{% hint style="info" %}
组件自定义数据处理属性 使用 on + 名词或名词短语 + Format 命名
{% endhint %}

```
✅
onFieldsFormat
```

***

{% hint style="info" %}
组件自定义渲染属性 使用 on + 名词或名词短语 + Render 命名
{% endhint %}

{% code fullWidth="false" %}
```javascript
✅
onEmptyRender、onLodingRender
```
{% endcode %}

***

{% hint style="info" %}
组件中渲染逻辑方法 使用 render + 名词或名词短语
{% endhint %}

```
✅
renderLoading、renderFormContent
```

***

{% hint style="info" %}
组件中RefObject类型变量 使用 名词 + Ref 命名
{% endhint %}

```
✅
containerRef
```

***

{% hint style="info" %}
组件数据中数据请求方法 应该以 get + 名词或者名词短语 命名
{% endhint %}

```
✅
getFields、getFieldById
```

### 2.2. 数据流Model命名规则

{% hint style="info" %}
数据model类 使用帕斯卡命名法（单词相连，单词首字母大写）+ Model后缀 命名
{% endhint %}

```
✅
CorpUserModel

⛔
CorpUser
```

***

{% hint style="info" %}
计算属性 使用名词或者名词短语命名
{% endhint %}

```
✅
formatFields

⛔
getFormatFields
```

### 2.3. 逻辑命名规则

{% hint style="info" %}
常量名 采用帕斯卡命名法(单词大写且使用下划线分隔)
{% endhint %}

```
✅
MAX_FILE_SIZE
```

***

{% hint style="info" %}
方法名 应当是动词或者动词短语
{% endhint %}

```
✅
save、deleteForm、getFieldNameById
```

***

{% hint style="info" %}
类名、组件名和对象名 应该是名词或者名词短语
{% endhint %}

```
✅
Form、AppTrigger
```

***

{% hint style="info" %}
逻辑判断方法 (典型特征返回Boolean类型数据作为返回值)使用 is + 名词或者名词短语 命名
{% endhint %}

```
✅
isPercentage、isDOMNode
```

### 2.4. 样式命名规则

{% hint style="info" %}
样式名 采用烤串命名法（单词间以中横线分隔）
{% endhint %}

```
✅
btn-disable
```

***

{% hint style="info" %}
Web端组件顶层样式 采用 fx- 为前缀
{% endhint %}

```
✅
fx-icon-button
```

***

{% hint style="info" %}
Mobile端组件顶层样式 采用 fxm- 为前缀
{% endhint %}

```
✅
fxm-icon-button
```

### 2.5. 文件目录命名规则

{% hint style="info" %}
目录名 采用烤串命名法（单词间以中横线分隔）
{% endhint %}

```
✅
app-icon、color-picker
```

***

{% hint style="info" %}
样式文件 采用烤串命名法（单词间以中横线分隔）+ 文件后缀
{% endhint %}

```
✅
app-icon.less、color-pick.less
```

***

{% hint style="info" %}
工具方法文件 采用烤串命名法（单词间以中横线分隔）+ 文件后缀
{% endhint %}

```
✅
local-storage.ts
```

***

{% hint style="info" %}
组件文件 采用帕斯卡命名法（单词相连，单词首字母大写）+ 文件后缀
{% endhint %}

```
✅
AppSet.tsx 
```

***

{% hint style="info" %}
model文件 采用驼峰命名法（单词相连，除首字母外，其余单词首字母大写）
{% endhint %}

```
✅
corpUser.ts
```

### 2.6. TS命名规则

{% hint style="info" %}
组件props接口定义固定以props结尾，不是用I作为前缀

***



> 不使用 "I" 前缀，因为这可能会导致代码冗长且不必要。接口的命名应当清晰地表达其用途，而不是通过前缀来区分

> 对于组件的属性接口，使用 "Props" 后缀是一个常见的做法。这种命名方式可以明确表示该接口是用于组件的属性定义，例如 `UserProps`。这种方式有助于在代码中快速识别接口的用途
{% endhint %}

```
✅
UserProps

⛔
IUserProps
```



