---
icon: react
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

# React规范

## 1. 最佳实践

### 1.1. 利用自定义Hooks提取重复逻辑

{% hint style="info" %}
提升代码可维护性
{% endhint %}

```
✅
const useAppInstall = (props: {appId: string}) => {
    const {appId} = props;
    // 安装应用逻辑
    const handleInstallApp = () => {
        return post(url, { appId }).then(() => {
            // 业务处理
        });
    }
    return {
        handleInstallApp
    };
};
 
 
const A = (props) => {
    const {handleInstallApp} = useAppInstall(props)
    return (
        <div>
            <button onClick={handleInstallApp}>Install</button>
        </div>
    )
 
}
 
const B = (props) => {
    const {handleInstallApp} = useAppInstall(props)
    return (
        <button onClick={handleInstallApp}>Install</button>
    )
}
```

```
⛔
const A = (props) => {
    const {appId} = props;
    // 安装应用逻辑
    const handleInstallApp = () => {
        return post(url, { appId }).then(() => {
            // 业务处理
        });
    }
    return (
        <button onClick={handleInstallApp}>Install</button>
    )
}
 
const B = (props) => {
    const {appId} = props;
    // 安装应用逻辑
    const handleInstallApp = () => {
        return post(url, { appId }).then(() => {
            // 业务处理
        });
    }
    return (
        <button onClick={handleInstallApp}>Install</button>
    )
}
```

### 1.2. 利用组件外纯函数简化组件

{% hint style="info" %}
合理将组件内的函数实现为纯函数并独立于组件外，避免每次渲染创建新实例
{% endhint %}

```
✅
const getText = (type: string) => {
    switch (type) {
        case 'something':
        default:
            return 'everything'
    }
}
 
const Comp = () => {
    return (
        <div>
            {getText(type)}
        </div>
    );
}
 
export default Comp;
```

```
⛔
const Comp = () => {
 
    const getText = (type: string) => {
        switch (type) {
            case 'something':
            default:
                return 'everything'
        }
    }
 
    return (
        <div>
            {getText(type)}
        </div>
    );
 
}
 
export default Comp;
```

### 1.3. 副作用函数按照业务拆分而非按照触发时机拆分

{% hint style="info" %}
组件中利用useEffect处理有关副作用，即使触发时机时机相同，也应该将不同业务的逻辑聚合在同一个副作用中。为了保证代码聚合，应该按照业务拆分副作用。
{% endhint %}

```
✅

const Comp = () => {
    useEffect(() => {
        // 组件数据获取后台数据请求
        post(url).then((res) => {
            // 数据处理
        }).catch(() => {
            // 处理异常逻辑
        });
    }, []);
 
    useEffect(() => {
        // 事件监听与处理
        window.addEventListener('beforeunload', handleBeforeunload);
        return () => {
            window.removeEventListener('beforeunload', handleBeforeunload)
        };
    })
}
```

```
⛔
const Comp = () => {
    useEffect(() => {
        // 组件数据获取后台数据请求
        post(url).then((res) => {
            // 数据处理
        }).catch(() => {
            // 处理异常逻辑
        });
 
        // 事件监听与处理
        window.addEventListener('beforeunload', handleBeforeunload);
        return () => {
            window.removeEventListener('beforeunload', handleBeforeunload)
        };
    }, []);
}
```





