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

## 下一步

学习更多布局技巧，查看 [布局系统](./05-布局系统.md)！

