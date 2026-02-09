# Origin USD (Unified Semantic DSL) v2 规范

## 概述

USD (Unified Semantic DSL) 是 Origin 的核心抽象层，是连接可视化编辑、代码编辑和 AI 助手的"唯一真实来源"。

## 设计原则

1. **声明式**：描述"是什么"而非"怎么做"
2. **强类型**：编译时检查，IDE 友好
3. **可组合**：支持 Mixin、Extend、Import
4. **可扩展**：支持自定义语义
5. **可版本化**：Git 原生支持

## 语法结构

### 基本结构

```dsl
@version("2.0")
@description("应用描述")
App <AppName> {

    // 元数据
    Metadata { ... }

    // 主题系统
    Theme { ... }

    // 能力声明
    Capabilities { ... }

    // 数据实体
    Entity { ... }

    // 全局状态
    GlobalState { ... }

    // API 定义
    API { ... }

    // 页面
    Screen { ... }

    // 组件
    Component { ... }

    // 流程
    Flow { ... }
}
```

## 8 大语义维度

### 1. 结构语义 (Structure)

定义 UI 组件层次关系。

```dsl
Screen LoginScreen {
    Route {
        path: "/login"
        name: "login"
        access: .public
    }

    Layout {
        Scaffold {
            appBar: AppBar {
                title: "登录"
            }

            body: Container {
                direction: column
                padding: 24
                gap: 16

                Logo { width: 80, height: 80 }
                TextField { ... }
                Button { ... }
            }
        }
    }
}

Component PrimaryButton {
    Props {
        content: String
        variant: "primary" | "secondary" | "outline"
        onClick: Action?
    }

    Layout {
        Button {
            content: props.content
            style: variant[props.variant]
        }
    }
}
```

### 2. 状态语义 (State)

定义组件内部状态和全局状态。

```dsl
Screen HomeScreen {
    // 本地状态
    State {
        searchQuery: String = ""
        selectedCategory: Category? = null
        viewMode: "grid" | "list" = "grid"

        // 派生状态
        hasSearch: Boolean = searchQuery.isNotEmpty
    }

    // 全局状态引用
    GlobalState {
        currentUser: User?
        cart: Cart
        themeMode: "light" | "dark"
    }

    // 状态绑定
    Layout {
        TextField {
            value: bind searchQuery
            onChange: { searchQuery = $0 }
        }
    }
}
```

### 3. 数据语义 (Data)

定义数据模型、验证和关系。

```dsl
Entity User {
    id: UUID @primaryKey @readonly
    username: String @validate(min: 3, max: 20)
    email: Email @validate(format: "email") @unique
    avatar: URL?

    // 计算属性
    displayName: String = username ?? email.prefix("@")

    // 关系
    orders: Relation<Order[]> @inverse("user")
    cart: Relation<Cart> @cascade
}

Entity Product {
    id: UUID @primaryKey
    name: String @validate(min: 1, max: 100)
    price: Money(currency: "CNY") @validate(min: 0)
    category: Relation<Category>
    images: List<Image> @validate(min: 1, max: 9)

    // 索引
    @index(fields: [category, price], order: desc)
}
```

### 4. 交互语义 (Interaction)

定义用户交互响应。

```dsl
Screen LoginScreen {
    State {
        username: String
        password: String
        isLoading: Boolean = false
    }

    Interaction {
        // 按钮点击
        onLoginButtonClick: {
            isLoading = true

            // API 调用
            result = await api.auth.login({
                username: username,
                password: password
            })

            // 成功处理
            if result.isSuccess {
                GlobalState.currentUser = result.user
                navigateTo("/home", replace: true)
            }

            // 错误处理
            if result.isError {
                showError(result.error.message)
            }

            isLoading = false
        }

        // 手势
        onSwipeRight: {
            navigateBack()
        }

        // 生命周期
        onAppear: {
            trackPageView("login")
        }
    }
}
```

### 5. 主题语义 (Theme)

定义设计系统。

```dsl
Theme {
    colors: {
        primary: #007AFF
        secondary: #5856D6
        success: #34C759
        warning: #FF9500
        error: #FF3B30

        background: {
            light: #F2F2F7
            dark: #1C1C1E
        }

        surface: {
            light: #FFFFFF
            dark: #2C2C2E
        }

        text: {
            primary: { light: #000000, dark: #FFFFFF }
            secondary: { light: #8E8E93, dark: #8E8E93 }
        }
    }

    typography: {
        fontFamily: {
            ios: "SF Pro"
            android: "Roboto"
            web: "system-ui"
        }

        scale: {
            h1: { size: 28, weight: 700, lineHeight: 34 }
            h2: { size: 22, weight: 700, lineHeight: 28 }
            body: { size: 16, weight: 400, lineHeight: 22 }
            caption: { size: 12, weight: 400, lineHeight: 16 }
        }
    }

    spacing: {
        unit: 4
        scale: [1, 2, 3, 4, 6, 8, 10, 12]
    }

    radii: {
        sm: 4, md: 8, lg: 12, xl: 16, full: 9999
    }

    shadows: {
        sm: { x: 0, y: 1, blur: 2, color: rgba(0,0,0,0.1) }
        md: { x: 0, y: 4, blur: 8, color: rgba(0,0,0,0.15) }
        lg: { x: 0, y: 8, blur: 16, color: rgba(0,0,0,0.2) }
    }
}
```

### 6. 能力语义 (Capability)

声明平台能力。

```dsl
Capabilities {
    Auth {
        methods: [password, sms, wechat, apple]
        session: jwt {
            expiry: "7d"
            refresh: true
        }
    }

    Network {
        offlineSupport: true
        cacheStrategy: "stale-while-revalidate"
        retryPolicy: exponentialBackoff(maxAttempts: 3)
    }

    Notification {
        push: true
        local: true
        categories: [order, promotion, system]
    }

    Storage {
        local: [preferences, cache, offlineData]
        cloud: optional
    }

    Analytics {
        providers: [amplitude, firebase]
        autoTrack: [pageView, buttonClick, error]
    }
}
```

### 7. 路由语义 (Routing)

定义导航结构。

```dsl
Screen ProfileScreen {
    Route {
        path: "/profile/:id"
        name: "profile"
        access: .authenticated

        deeplink: [
            "app://profile/:id",
            "https://app.com/profile/:id"
        ]
    }

    State {
        userId: String = route.params.id
    }

    Interaction {
        onNavigateBack: {
            navigateBack()
        }

        onEditProfile: {
            navigateTo("/profile/edit", params: {id: userId})
        }
    }
}
```

### 8. 业务语义 (Business)

定义业务流程。

```dsl
Flow Checkout {
    description: "下单流程"

    // 入口守卫
    guard: {
        if GlobalState.currentUser == null {
            navigateTo(Login, redirect: "checkout")
            return .blocked
        }
        if GlobalState.cart.isEmpty {
            showToast("购物车为空")
            return .blocked
        }
        return .allowed
    }

    // 流程步骤
    steps: [
        Step CartReview {
            screen: CartScreen
            validate: GlobalState.cart.items.isNotEmpty
            onNext: { validateStock() }
        }

        Step Shipping {
            screen: AddressScreen
            validate: selectedAddress != null
        }

        Step Payment {
            screen: PaymentScreen
            validate: paymentMethod != null
        }

        Step Confirmation {
            screen: OrderConfirmScreen
            action: submitOrder
            onSuccess: { order in
                GlobalState.cart.clear()
                navigateTo(OrderSuccess, {order: order})
            }
        }
    ]
}
```

## 类型系统

### 基础类型

```dsl
// 原始类型
String
Number
Boolean
DateTime

// 特殊类型
UUID
Email
URL
PhoneNumber
Money(currency: "CNY")
RichText

// 容器类型
List<T>
Map<K, V>
Set<T>

// 可选类型
T?
```

### 联合类型

```dsl
variant: "primary" | "secondary" | "outline"
status: "loading" | "success" | "error"
```

### 枚举

```dsl
enum UserRole {
    guest
    user
    admin
}

enum ThemeMode {
    light
    dark
    system
}
```

## 表达式

### 绑定

```dsl
// 双向绑定
TextField {
    value: bind username
}

// 单向绑定
Text {
    content: username
}

// 计算绑定
Text {
    content: "Hello, ${username}!"
}
```

### 条件

```dsl
// if 表达式
if condition {
    Container { ... }
} else {
    Container { ... }
}

// 内联条件
visible: isLoading
opacity: isPressed ? 0.8 : 1.0
```

### 列表

```dsl
// 列表渲染
List {
    items: users
    render: { user in
        UserCard {
            user: user
            onTap: navigateTo(Profile, {id: user.id})
        }
    }
}

// 列表操作
filtered: items.filter { item => item.active }
mapped: items.map { item => item.name }
summed: items.sum { item => item.price }
```

## 装饰器

```dsl
// 组件装饰器
@route("/login")
@access(.public)
Screen LoginScreen { ... }

// 实体装饰器
@Entity
@table("users")
class User { ... }

// 验证装饰器
@validate(min: 3, max: 20)
username: String
```

## 内置组件

### 布局

```dsl
Container     // 容器
Row           // 横向布局
Column        // 纵向布局
Stack         // 堆叠布局
ScrollView    // 滚动视图
Expanded      // 扩展占满
AspectRatio   // 宽高比
```

### 表单

```dsl
TextField     // 文本输入
TextArea      // 多行输入
NumberField   // 数字输入
CheckBox      // 复选框
Switch        // 开关
Select        // 下拉选择
DatePicker    // 日期选择
```

### 显示

```dsl
Text          // 文本
Image         // 图片
Icon          // 图标
Avatar        // 头像
Badge         // 徽章
Divider       // 分割线
```

### 交互

```dsl
Button        // 按钮
IconButton    // 图标按钮
Link          // 链接
Dropdown      // 下拉菜单
Dialog        // 对话框
BottomSheet   // 底部抽屉
```

## 导入与导出

```dsl
// 导入
Import {
    api: "./api/auth.api"
    component: "./components/Button.origin"
    theme: "./theme/company.origin"
}

// 导出
Export {
    component: PrimaryButton
    entity: User
    api: auth.login
}
```

## 注释

```dsl
// 单行注释

/*
多行注释
*/

/// 文档注释
Screen LoginScreen {
    /// 用户名输入框
    TextField { ... }
}
```

## 代码生成映射

### Flutter 示例

```dsl
// USD
Screen LoginScreen {
    State {
        username: String
    }

    Layout {
        TextField {
            value: bind username
            placeholder: "用户名"
        }
    }
}

// 生成 Flutter
class LoginScreen extends ConsumerStatefulWidget {
  @override
  ConsumerState<LoginScreen> createState() => _LoginScreenState();
}

class _LoginScreenState extends ConsumerState<LoginScreen> {
  late String _username;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: TextField(
        controller: TextEditingController(text: _username),
        decoration: InputDecoration(
          hintText: '用户名',
        ),
        onChanged: (value) => setState(() => _username = value),
      ),
    );
  }
}
```

### React Native 示例

```tsx
// USD
Button {
    content: "点击"
    onClick: handleClick
}

// 生成 React Native
<TouchableOpacity onPress={handleClick}>
  <Text>点击</Text>
</TouchableOpacity>
```

## 向后兼容

USD v2 与 v1 的主要区别：

| 特性 | v1 | v2 |
|------|----|----|
| 语法 | 类 JSON | 类 Swift/Kotlin |
| 类型 | 弱类型 | 强类型 |
| 组件 | Component | Screen + Component |
| 状态 | 分散 | 集中 State 块 |
| 交互 | Action | Interaction 块 |

v1 代码可通过自动迁移工具升级到 v2。
