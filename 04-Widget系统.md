# 04 - Widget 系统

## Widget 概述

Widget 是 Flutter 应用的基础构建块。每个 Widget 都是不可变的，它们描述了应用 UI 的配置。

## Widget 树

Flutter 应用是一个 Widget 树：
```
MyApp
 └── MaterialApp
      └── Scaffold
           ├── AppBar
           └── Body
                └── Column
                     ├── Text
                     └── Button
```

## 常用 Widget

### 文本 Widget

```dart
Text(
  'Hello Flutter',
  style: TextStyle(
    fontSize: 24,
    color: Colors.blue,
    fontWeight: FontWeight.bold,
  ),
)
```

### 按钮 Widget

```dart
// 普通按钮
ElevatedButton(
  onPressed: () {
    print('Button clicked');
  },
  child: Text('Click me'),
)

// 文本按钮
TextButton(
  onPressed: () {},
  child: Text('Text Button'),
)

// 图标按钮
IconButton(
  icon: Icon(Icons.favorite),
  onPressed: () {},
)
```

### 图片 Widget

```dart
// 网络图片
Image.network('https://example.com/image.jpg')

// 本地图片
Image.asset('assets/images/logo.png')

// 占位符
Image.asset('assets/images/logo.png',
  errorBuilder: (context, error, stackTrace) {
    return Icon(Icons.error);
  },
)
```

### 输入框 Widget

```dart
TextField(
  decoration: InputDecoration(
    labelText: 'Username',
    hintText: 'Enter your username',
    border: OutlineInputBorder(),
  ),
  onChanged: (value) {
    print('Input: $value');
  },
)
```

## 布局 Widget

### Container

```dart
Container(
  width: 200,
  height: 100,
  padding: EdgeInsets.all(16),
  margin: EdgeInsets.all(8),
  decoration: BoxDecoration(
    color: Colors.blue,
    borderRadius: BorderRadius.circular(8),
  ),
  child: Text('Container'),
)
```

### Row 和 Column

```dart
// 水平布局
Row(
  mainAxisAlignment: MainAxisAlignment.center,
  children: [
    Text('Left'),
    Text('Center'),
    Text('Right'),
  ],
)

// 垂直布局
Column(
  crossAxisAlignment: CrossAxisAlignment.start,
  children: [
    Text('Top'),
    Text('Middle'),
    Text('Bottom'),
  ],
)
```

### Stack

```dart
Stack(
  children: [
    Container(color: Colors.blue),
    Positioned(
      top: 10,
      left: 10,
      child: Text('Positioned'),
    ),
  ],
)
```

## 页面结构 Widget

页面结构 Widget 是用于构建完整页面的基础框架，它们提供了标准的页面布局和功能。

### Scaffold（Material Design 页面框架）

`Scaffold` 是 Flutter 中用于实现 Material Design 页面布局的基础 Widget，它为页面提供标准的页面结构。

**主要组件：**
- `appBar` - 顶部应用栏（标题栏）
- `body` - 页面主体内容区域
- `floatingActionButton` - 悬浮操作按钮（可选）
- `drawer` - 侧边抽屉菜单（可选）
- `bottomNavigationBar` - 底部导航栏（可选）
- `snackBar` - 底部消息提示（可选）

**示例：**

```dart
Scaffold(
  appBar: AppBar(
    title: Text('我的应用'),
  ),
  body: Center(
    child: Text('这是页面内容'),
  ),
  floatingActionButton: FloatingActionButton(
    onPressed: () {},
    child: Icon(Icons.add),
  ),
)
```

**在 Widget 树中的位置：**

```
MyApp
 └── MaterialApp
      └── Scaffold  ← 提供页面框架
           ├── AppBar
           └── Body
                └── Column
                     ├── Text
                     └── Button
```

### CupertinoPageScaffold（iOS 风格页面框架）

`CupertinoPageScaffold` 是 iOS 风格的页面结构，对应 Material Design 的 `Scaffold`。

**主要属性：**
- `navigationBar` - 顶部导航栏（对应 AppBar）
- `child` - 页面主体内容

**示例：**

```dart
CupertinoPageScaffold(
  navigationBar: CupertinoNavigationBar(
    middle: Text('我的应用'),
  ),
  child: Center(
    child: Text('这是页面内容'),
  ),
)
```

### 其他页面结构 Widget

#### SafeArea（安全区域处理）

处理安全区域，避开刘海屏、状态栏等系统 UI：

```dart
SafeArea(
  child: Scaffold(
    body: Text('内容'),
  ),
)
```

#### Center（居中布局）

将子 Widget 居中显示：

```dart
Center(
  child: Text('居中文本'),
)
```

#### Padding（内边距容器）

为子 Widget 添加内边距：

```dart
Padding(
  padding: EdgeInsets.all(16),
  child: Text('有内边距的内容'),
)
```

### 页面结构 Widget 对比

| Widget | 用途 | 适用场景 |
|--------|------|----------|
| **Scaffold** | Material Design 页面框架 | Android 风格应用 |
| **CupertinoPageScaffold** | iOS 风格页面框架 | iOS 风格应用 |
| **Container** | 通用容器 | 自定义样式、布局 |
| **SafeArea** | 安全区域处理 | 需要避开系统 UI |
| **Center** | 居中布局 | 简单居中内容 |
| **Padding** | 内边距 | 添加间距 |
| **Stack** | 层叠布局 | 悬浮元素、遮罩 |

### 使用建议

- **Material 应用**：使用 `Scaffold` 作为页面根容器
- **iOS 风格应用**：使用 `CupertinoPageScaffold` 作为页面根容器
- **自定义布局**：使用 `Container` + 布局 Widget 组合
- **需要避开系统 UI**：外层包裹 `SafeArea`

## 下一步

学习更多布局技巧，查看 [布局系统](./05-布局系统.md)！

