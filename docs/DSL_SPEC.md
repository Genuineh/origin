# Origin DSL 规范

## 概述

Origin DSL (Domain Specific Language) 是连接设计稿和代码的中间表示语言。

## 设计原则

1. **声明式**: 描述"是什么"而非"怎么做"
2. **框架无关**: 可转换为多个目标框架
3. **可扩展**: 支持自定义属性和行为
4. **可组合**: 组件可嵌套和复用

## 语法结构

### 基本结构

```dsl
// 文件级别
Project <Name> {
    framework: "flutter"
    version: "1.0.0"

    // 导入
    Import {
        api: "./api/auth.api"
        component: "./components/Button.origin"
    }

    // 页面
    Screen LoginScreen { ... }
    Screen HomeScreen { ... }

    // 可复用组件
    Component PrimaryButton { ... }
}
```

### Screen 定义

```dsl
Screen LoginScreen {
    // 路由配置
    Route {
        path: "/login"
        name: "login"
    }

    // 状态定义
    State {
        username: String
        password: String
        isLoading: Boolean
        errorMessage: String?
        showPassword: Boolean = false
    }

    // UI 布局
    Layout {
        Container {
            direction: column
            padding: 24
            gap: 16
            width: 100%
            height: 100%

            Logo {
                width: 80
                height: 80
                alignSelf: center
            }

            TextField {
                placeholder: "用户名"
                value: bind username
                icon: "user"
            }

            TextField {
                placeholder: "密码"
                value: bind password
                secret: bind !showPassword
                icon: "lock"
                trailingIcon: {
                    name: if showPassword then "eye-off" else "eye"
                    onClick: toggleShowPassword
                }
            }

            if errorMessage != null {
                Text {
                    content: errorMessage
                    color: "error"
                }
            }

            Button {
                content: "登录"
                loading: isLoading
                disabled: username.isEmpty || password.isEmpty
                onClick: loginAction
            }

            Text {
                content: "忘记密码？"
                align: center
                onClick: navigateTo("/forgot-password")
            }
        }
    }

    // 生命周期
    OnInit {
        // 初始化逻辑
    }

    OnDispose {
        // 清理逻辑
    }

    // 动作定义
    Action loginAction {
        // 设置加载状态
        isLoading = true
        errorMessage = null

        // API 调用
        api: auth.login
        request: {
            username: username,
            password: password
        }

        // 成功处理
        OnSuccess { response in
            // 保存 token
            storage.set("token", response.token)

            // 导航
            navigateTo("/home", replace: true)
        }

        // 失败处理
        OnError { error in
            isLoading = false
            errorMessage = error.message
        }

        // 完成处理
        OnComplete {
            isLoading = false
        }
    }

    Action toggleShowPassword {
        showPassword = !showPassword
    }
}
```

### Component 定义

```dsl
Component PrimaryButton {
    // 属性定义
    Props {
        content: String
        variant: "primary" | "secondary" | "outline"
        size: "small" | "medium" | "large"
        disabled: Boolean = false
        loading: Boolean = false
        onClick: Action?
    }

    // 样式变体
    Variant {
        primary {
            backgroundColor: "primary"
            textColor: "white"
        }
        secondary {
            backgroundColor: "secondary"
            textColor: "white"
        }
        outline {
            backgroundColor: "transparent"
            borderColor: "primary"
            textColor: "primary"
        }
    }

    Layout {
        Button {
            content: props.content
            style: variant[props.variant]
            size: props.size
            disabled: props.disabled || props.loading
            onClick: props.onClick

            if props.loading {
                Spinner {}
            }
        }
    }
}
```

### API 定义

```dsl
// 从 OpenAPI 导入
API auth.login {
    method: POST
    path: /auth/login

    Request {
        username: String
        password: String
    }

    Response {
        token: String
        user: User
    }

    Error {
        message: String
        code: String
    }
}
```

## 类型系统

### 基础类型

| 类型 | 描述 | 示例 |
|------|------|------|
| `String` | 字符串 | `"hello"` |
| `Number` | 数字 | `42`, `3.14` |
| `Boolean` | 布尔值 | `true`, `false` |
| `List<T>` | 列表 | `[1, 2, 3]` |
| `Map<K,V>` | 映射 | `{key: value}` |
| `T?` | 可选类型 | `null` 或值 |

### 联合类型

```dsl
variant: "primary" | "secondary" | "outline"
```

### 对象类型

```dsl
State {
    user: {
        id: String
        name: String
        email: String
    }
}
```

## 表达式

### 绑定表达式

```dsl
value: bind username  // 双向绑定
text: username        // 单向绑定
```

### 条件表达式

```dsl
if condition then {
    ...
} else {
    ...
}

// 或内联
visible: isLoading
```

### 列表操作

```dsl
items: users.map { user =>
    Text { content: user.name }
}

filtered: items.filter { item => item.active }
```

### 字符串插值

```dsl
content: "Hello, ${username}!"
```

## 内置组件

### 布局组件

```dsl
Container { ... }       // 容器
Row { ... }             // 横向布局
Column { ... }          // 纵向布局
Stack { ... }           // 堆叠布局
ScrollView { ... }      // 滚动视图
```

### 表单组件

```dsl
TextField { ... }       // 文本输入
TextArea { ... }        // 多行输入
NumberField { ... }     // 数字输入
CheckBox { ... }        // 复选框
RadioGroup { ... }      // 单选组
Switch { ... }          // 开关
Select { ... }          // 下拉选择
DatePicker { ... }      // 日期选择
```

### 显示组件

```dsl
Text { ... }            // 文本
Image { ... }           // 图片
Icon { ... }            // 图标
Avatar { ... }          // 头像
Badge { ... }           // 徽章
Divider { ... }         // 分割线
```

### 交互组件

```dsl
Button { ... }          // 按钮
IconButton { ... }      // 图标按钮
LinkButton { ... }      // 链接按钮
Dropdown { ... }        // 下拉菜单
Dialog { ... }          // 对话框
BottomSheet { ... }     // 底部抽屉
```

### 导航组件

```dsl
TabBar { ... }          // 标签栏
NavigationBar { ... }   // 导航栏
Sidebar { ... }         // 侧边栏
Breadcrumb { ... }      // 面包屑
```

## 样式系统

### 内联样式

```dsl
Container {
    width: 100
    height: 50
    padding: 16
    backgroundColor: "#FF0000"
    borderRadius: 8
}
```

### 主题引用

```dsl
Text {
    color: "colors.primary"
    fontSize: "typography.h1"
}
```

### 响应式

```dsl
Container {
    width: "100%"
    maxWidth: {
        mobile: "100%",
        tablet: "600px",
        desktop: "1200px"
    }
}
```

## 注释

```dsl
// 单行注释

/*
多行注释
*/

/// 文档注释
Screen LoginScreen { ... }
```

## 目标框架映射

### Flutter 示例

```dart
// DSL
TextField {
    value: bind username
    placeholder: "用户名"
}

// 生成
TextFormField(
  decoration: InputDecoration(
    hintText: '用户名',
  ),
  controller: _usernameController,
)
```

### React Native 示例

```tsx
// DSL
Button {
    content: "点击"
    onClick: handleClick
}

// 生成
<TouchableOpacity onPress={handleClick}>
  <Text>点击</Text>
</TouchableOpacity>
```
