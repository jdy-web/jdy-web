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

# 函数规范

## 1.原则

### 1.1. 短小精悍

{% hint style="info" %}
函数的第一条规则是应该要短小精悍，人类的精力是有限的，函数体积越小意味着函数内部要维护的逻辑越少，也意味着更少的出错概率。并且如果函数本身命名合理，也能进一步提升可阅读性。
{% endhint %}

```
interface Employee {
    type: string;
}

✅
const calculatePay = (e: Employee) => {
    switch (e.type) {
        case 'COMMISSIONED':
            return calculateCommissionedPay(e);
        case 'HOURLY':
            return calculateHourlyPay(e);
        case 'SALARIED':
            return calculateSalariedPay(e);
    }
};
 
⛔
const calculatePay = (e: Employee) => {
    switch (e.type) {
        case 'COMMISSIONED':
        // 计算算法
        // ...
        // return ...
        case 'HOURLY':
        // 计算算法
        // ...       
        // return ...
        case 'SALARIED':
        // 计算算法
        // ...
        // return ...
    }
};
```

### 1.2. 单一职责

{% hint style="info" %}
函数应该只做一件事。做好这件事。只做这一件事。

> _只做一件事并不是指字面意义上的函数只能做一个步骤操作。而是指函数中的逻辑都应该在同一抽象层。一般而言，判断函数是否只做了一件事可以看它是否能(或者是应该)被拆分出一个逻辑独立的函数。_
{% endhint %}

```
interface IUser {
    id: string;
    name: string;
    male: boolean;
    score: number;
    status: number;
}

✅
// 数据预处理
const formatMalePassUsers = (users: IUser | IUser[]) => {
    const userList = Array.isArray(users) ? users : [users];
    const formatUsers = userList.filter((user) => {
        return user.male && user.score >= 60
    });
}
// 数据修改
const modifyMalePassUsers = (users: IUser | IUser[]) => {
    const formatUsers = formatMalePassUsers(users)
    return formatUsers.map((user) => ({ ...user, status: 1 }));
}
 
⛔
const modifyMalePassUsers = (users: IUser | IUser[]) => {
    const userList = Array.isArray(users) ? users : [users];
    const formatUsers = userList.filter((user) => {
        return user.male && user.score >= 60
    });
    return formatUsers.map((user) => ({ ...user, status: 1 }));
}
 
```

### 1.3. 纯函数

{% hint style="info" %}
项目工程中推荐大量使用纯函数



什么是纯函数？

* 相同的函数永远返回相同的结果，函数不依赖外部任何状态或数据的变化，必须只依赖于其输入参数。
* 函数不会产生任何可观察的副作用



纯函数的优点有哪些？

* 易于维护性
* 可缓存性(例如：利用useMemo缓存计算结果)
* 易于测试性(例如：利用useMemo缓存计算结果)
{% endhint %}

```
interface IUser {
    id: string;
    name: string;
}

✅
const modifyUserName = (users: IUser[], id: string, newName: string) => {
    return users.map((user) => {
        return user.id === id ? {...user, name: newName}: user;
    });
}
 
⛔
const modifyUserName = (users: IUser[], id: string, newName: string) => {
    users.forEach((user) => {
        if(user.id === id) {
            user.name = newName;
        }
    });
    return users;
}
```

### 1.4. 避免重复

{% hint style="info" %}
重复可能是软件中一切邪恶的根源。许多原则和实践规则都是为了控制与消除重复而创建的。当代码出现重复时，不仅降低了因为臃肿而降低了可读性，而且一旦算法改变时需要改动多个地方，增加了出错的概率。
{% endhint %}

```
✅
export const isSubformWidget = (name: string) => {
    return /\./.test(name);
};
 
const registerField = (field: FieldModel) => {
    if(isSubformWidget(field.name)) {
        // ....
    }
    // ...
    if(isSubformWidget(field.name)) {
        // ....
    }
}

⛔
const registerField = (field: FieldModel) => {
    if(/\./.test(field.name)) {
        // ......
    }
    // ......
    if(/\./.test(field.name)) {
        // ......
    }
}
```

## 2. 最佳实践

### 2.1. 省略else

{% hint style="info" %}
部分场景中函数可以省略else



> _原因：减少无用代码，尽量保持代码简洁_
{% endhint %}

```
👍
const getPosition = (placement: string) => {
    if (placement === 'bottom') {
        // return
    }
    if (placement === 'top') {
        // return
    }
    if (placement === 'left') {
        // return
    }
    if (placement === 'right') {
        // return
    }
}

🤔
const getPosition = (placement: string) => {
    if (placement === 'bottom') {
        // return
    }
    else if (placement === 'top') {
        // return
    }
    else if (placement === 'left') {
        // return
    }
    else if (placement === 'right') {
        // return
    }
}
```

### 2.2. 提前退出

{% hint style="info" %}
非复杂函数不必遵循结构化编程规则（每个函数保持有一个入口、一个出口），可提前退出



> _原因：可减少无用变量的创建、代码提前退出保证代码可阅读性，函数使用者可只关注部分逻辑，避免整体阅读_
{% endhint %}

```
👍
const getPosition = (placement: string) => {
    if (placement === 'bottom') {
        // return {x: 0, y: 0}
    }
    if (placement === 'top') {
        // return {x: 0, y: 0}
    }
    if (placement === 'left') {
        // return {x: 0, y: 0}
    }
    if (placement === 'right') {
        // return {x: 0, y: 0}
    }
    // return {x: 0, y: 0}
}

🤔
const getPosition = (placement: string) => {
    const position = {};
    if (placement === 'bottom') {
        // assign(position, {x: 0, y: 0})
    }
    if (placement === 'top') {
        // assign(position, {x: 0, y: 0})
    }
    if (placement === 'left') {
        // assign(position, {x: 0, y: 0})
    }
    if (placement === 'right') {
        // assign(position, {x: 0, y: 0})
    }
    return position;
}
```

### 2.3. 函数参数不超过三个

{% hint style="info" %}
函数参数不超过三个。如确实无法减少，使用对象传参替代



> _原因：函数参数过多会降低易用性和可阅读性。对象参数方便给定函数参数默认值。_
{% endhint %}

```
✅
const getAppConfig = (
    appId: string,
    options: { hasRefApp: boolean; hasSubForm?: boolean, hasAggregateTable?: boolean }
) => {
    // .....
};

⛔
const getAppConfig = (appId: string, hasRefApp: boolean, hasSubForm ?: boolean, hasAggregateTable ?: boolean) => {
    // 省略
}
```

### 2.4. 避免布尔值类型的标识性参数

{% hint style="info" %}
标识性参数会导致使用处可读性降低
{% endhint %}

```
👍
const renderForSuite = () => {
    // 对应 isSuite: true
    // ......
}
 
const renderNormal = () => {
     // 对应 isSuite: false
    // ......
}

🤔
const render = (isSuite: boolean) => {
    // ......
}
```

### 2.5. 避免指令函数和询问函数

{% hint style="info" %}
函数要么做什么事，要么回答什么事，但不可兼得。例如函数内同时在修改对象状态或是返回对象信息，会导致混乱。
{% endhint %}

```
✅
const attributeExists = (obj: object, key: string, value: string): boolean => {
    // ....
}
 
const setAttribute = (obj: object, key: string, value: string) => {
    // ....
}
 
if (attributeExists(obj, 'name', 'jdy')) {
    setAttribute(obj, 'name', 'jdy')
}

⛔
const set = (obj: object, key: string, value: string) => {
    if(obj.hasOwnProperty(key)) {
        obj[key] = value;
        return true
    }
    return false
}
 
if (set(obj, 'name', 'jdy')) {
    // 使用if(set(obj, 'name', 'jdy'))
    // 作为判断语句无法直观理解set的返回值是什么？到底是设置是否成功? 还是是否存在？
}
```

### 2.6. 避免回调函数和Promise的混用，优先使用Promise

{% hint style="info" %}
混用回调函数和Promise容易产生逻辑混乱
{% endhint %}

```
✅
const getUserInfo = (userId: string, callback: (user: object) => void) => {
    return post(url, { userId }).then((data) => {
        return get(data, 'user');
    })
}
 
getUserInfo(userId).then((user) => {
    // ......
})

⛔
const getUserInfo = (userId: string, callback: (user: object) => void) => {
    post(url, { userId }).then((data) => {
        const user = get(data, 'user');
        callback(user)
    })
}
 
getUserInfo(userId, (user) => {
    // ......
})
```

### 2.7. 避免输入参数函数，参数只能用来传递。输入应该使用返回值

{% hint style="info" %}
输入参数容易不易维护，造成可读性降低
{% endhint %}

```
✅
const clearStorage = () => {
    // .......
    if (success) {
        return { valid: true }
    }
    return { valid: false, msg: 'something wrong' }
}

⛔
const clearStorage = (result: { valid: boolean, msg: string }) => {
    // .......
    if (success) {
        result = { valid: true }
    } else {
        result = { valid: false, msg: 'something wrong' }
    }
}
```

