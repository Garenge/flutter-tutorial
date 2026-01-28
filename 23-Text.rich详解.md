# Text.rich 和 TextSpan 详解

## 层级结构

```dart
Text.rich(                    // 第1层：Text.rich Widget
  TextSpan(                   // 第2层：根 TextSpan（容器）
    children: [               // 第3层：children 数组（包含多个 TextSpan）
      TextSpan(               // 第4层：第一个子 TextSpan
        text: '文本内容',
        style: TextStyle(...),
      ),
      TextSpan(               // 第4层：第二个子 TextSpan
        text: '更多文本',
        style: TextStyle(...),
      ),
    ],
  ),
)
```

## 逐层详解

### 第1层：Text.rich Widget

**作用**：Flutter 的一个 Widget，用于显示富文本（可以包含不同样式的文本）

**特点**：
- 类似于普通的 `Text` Widget，但可以包含多个不同样式的文本片段
- 必须包含一个 `TextSpan` 作为根节点

**语法**：
```dart
Text.rich(
  TextSpan(...),  // 必须有一个根 TextSpan
)
```

### 第2层：TextSpan（根节点）

**作用**：文本片段的容器，可以包含文本内容和样式

**属性**：
- `text`：文本内容（可选，如果只作为容器可以省略）
- `style`：文本样式（可选）
- `children`：子 TextSpan 列表（可选）

**在你的代码中**：
```dart
TextSpan(
  children: [  // 这个 TextSpan 只作为容器，不包含自己的文本
    // 子 TextSpan...
  ],
)
```

### 第3层：children 数组

**作用**：包含多个 `TextSpan` 的列表

**特点**：
- 类型是 `List<TextSpan>`
- 可以包含任意数量的 `TextSpan`
- 每个 `TextSpan` 可以有不同的样式
- 它们会按顺序连接在一起显示

**示例**：
```dart
children: [
  TextSpan(...),  // 第一个片段
  TextSpan(...),  // 第二个片段
  TextSpan(...),  // 第三个片段
]
```

### 第4层：子 TextSpan

**作用**：实际的文本片段，包含文本内容和样式

**属性**：
- `text`：**必需**，要显示的文本内容
- `style`：**可选**，`TextStyle` 对象，定义字体、颜色、大小等

**在你的代码中**：

```dart
// 第一个 TextSpan
TextSpan(
  text: 'You have pushed the button\n',  // 文本内容
  style: TextStyle(                       // 样式
    fontSize: 20,
    fontFamily: 'Menlo',
    color: Colors.red,
  ),
)

// 第二个 TextSpan
TextSpan(
  text: ' this many times:',              // 文本内容
  style: TextStyle(                       // 样式
    fontSize: 15,
    fontFamily: 'Times',
    color: Colors.blue,
  ),
)
```

## 完整示例解析

```dart
Text.rich(
  TextSpan(                    // 根容器（不包含文本）
    children: [                // 子片段列表
      TextSpan(                // 片段1：红色 Menlo 字体
        text: 'You have pushed the button\n',
        style: TextStyle(
          fontSize: 20,
          fontFamily: 'Menlo',
          color: Colors.red,
        ),
      ),
      TextSpan(                // 片段2：蓝色 Times 字体
        text: ' this many times:',
        style: TextStyle(
          fontSize: 15,
          fontFamily: 'Times',
          color: Colors.blue,
        ),
      ),
    ],
  ),
)
```

**显示效果**：
- 第一行：`You have pushed the button`（红色，Menlo，20号）
- 第二行：` this many times:`（蓝色，Times，15号）

## 对比：Text vs Text.rich

### 普通 Text（单一样式）
```dart
Text(
  'Hello World',
  style: TextStyle(color: Colors.red),
)
```
- ✅ 简单，适合单一样式
- ❌ 无法在同一行使用不同样式

### Text.rich（多种样式）
```dart
Text.rich(
  TextSpan(
    children: [
      TextSpan(text: 'Hello', style: TextStyle(color: Colors.red)),
      TextSpan(text: ' World', style: TextStyle(color: Colors.blue)),
    ],
  ),
)
```
- ✅ 可以在同一行使用不同样式
- ✅ 更灵活，可以嵌套
- ❌ 代码稍微复杂一些

## 嵌套 TextSpan

`TextSpan` 可以嵌套，形成更复杂的结构：

```dart
Text.rich(
  TextSpan(
    children: [
      TextSpan(
        text: 'Hello ',
        style: TextStyle(color: Colors.red),
      ),
      TextSpan(
        text: 'World',
        style: TextStyle(color: Colors.blue),
        children: [  // 嵌套的子 TextSpan
          TextSpan(
            text: '!',
            style: TextStyle(color: Colors.green),
          ),
        ],
      ),
    ],
  ),
)
```

## 常见用法

### 1. 混合样式文本
```dart
Text.rich(
  TextSpan(
    children: [
      TextSpan(text: '正常文本 '),
      TextSpan(
        text: '粗体文本',
        style: TextStyle(fontWeight: FontWeight.bold),
      ),
      TextSpan(text: ' 正常文本'),
    ],
  ),
)
```

### 2. 带链接的文本
```dart
Text.rich(
  TextSpan(
    children: [
      TextSpan(text: '访问 '),
      TextSpan(
        text: 'Flutter官网',
        style: TextStyle(color: Colors.blue, decoration: TextDecoration.underline),
        recognizer: TapGestureRecognizer()
          ..onTap = () {
            // 打开链接
          },
      ),
    ],
  ),
)
```

### 3. 高亮关键词
```dart
Text.rich(
  TextSpan(
    children: [
      TextSpan(text: '搜索关键词：'),
      TextSpan(
        text: 'Flutter',
        style: TextStyle(
          backgroundColor: Colors.yellow,
          fontWeight: FontWeight.bold,
        ),
      ),
    ],
  ),
)
```

## 总结

**层级关系**：
1. **Text.rich** → Widget 层（显示组件）
2. **TextSpan** → 容器层（组织文本片段）
3. **children** → 数组层（包含多个片段）
4. **TextSpan** → 片段层（实际的文本和样式）

**记忆技巧**：
- `Text.rich` = 富文本 Widget
- `TextSpan` = 文本片段（可以嵌套）
- `children` = 多个片段组成的列表
- 每个 `TextSpan` 可以有自己的 `text` 和 `style`

