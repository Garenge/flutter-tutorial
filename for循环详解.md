# Dart for 循环详解

> 一份完整的 Dart for 循环教程，涵盖所有循环写法和实际应用场景

---

## 目录

1. [传统 for 循环](#1-传统-for-循环)
2. [for-in 循环](#2-for-in-循环)
3. [forEach 方法](#3-foreach-方法)
4. [for-in 配合索引](#4-for-in-配合索引)
5. [while 和 do-while 循环](#5-while-和-do-while-循环)
6. [循环控制语句](#6-循环控制语句)
7. [集合的 map、where、reduce 等方法](#7-集合的-mapwherereduce-等方法)
8. [在 Flutter Widget 中使用循环](#8-在-flutter-widget-中使用循环)
9. [性能对比和最佳实践](#9-性能对比和最佳实践)

---

## 1. 传统 for 循环

### 基本语法

**标准写法**：
```dart
for (初始化; 条件; 增量) {
  // 循环体
}
```

**示例**：
```dart
// 从 0 到 9
for (int i = 0; i < 10; i++) {
  print(i); // 输出: 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
}

// 从 10 到 1（倒序）
for (int i = 10; i > 0; i--) {
  print(i); // 输出: 10, 9, 8, 7, 6, 5, 4, 3, 2, 1
}

// 步长为 2
for (int i = 0; i < 10; i += 2) {
  print(i); // 输出: 0, 2, 4, 6, 8
}
```

**特点**：
- ✅ 可以精确控制循环次数
- ✅ 可以访问索引
- ✅ 可以自定义步长
- ✅ 适合需要索引的场景

**使用场景**：
- 需要索引的循环
- 需要倒序或自定义步长
- 需要精确控制循环次数

---

## 2. for-in 循环

### 基本语法

**语法**：
```dart
for (var item in collection) {
  // 循环体
}
```

**示例**：
```dart
// 遍历 List
List<String> fruits = ['苹果', '香蕉', '橙子'];
for (var fruit in fruits) {
  print(fruit); // 输出: 苹果, 香蕉, 橙子
}

// 遍历 Set
Set<int> numbers = {1, 2, 3, 4, 5};
for (var number in numbers) {
  print(number); // 输出: 1, 2, 3, 4, 5
}

// 遍历 Map 的键
Map<String, int> ages = {'Alice': 25, 'Bob': 30, 'Charlie': 35};
for (var name in ages.keys) {
  print('$name: ${ages[name]}');
}
// 输出:
// Alice: 25
// Bob: 30
// Charlie: 35

// 遍历 Map 的键值对
for (var entry in ages.entries) {
  print('${entry.key}: ${entry.value}');
}
```

**特点**：
- ✅ 语法简洁，易读
- ✅ 不需要手动管理索引
- ✅ 适合遍历集合元素
- ❌ 无法直接获取索引

**使用场景**：
- 遍历集合元素，不需要索引
- 代码简洁性优先的场景

---

## 3. forEach 方法

### 基本语法

**语法**：
```dart
collection.forEach((item) {
  // 循环体
});
```

**示例**：
```dart
// List 的 forEach
List<String> fruits = ['苹果', '香蕉', '橙子'];
fruits.forEach((fruit) {
  print(fruit); // 输出: 苹果, 香蕉, 橙子
});

// 箭头函数写法（单行）
fruits.forEach((fruit) => print(fruit));

// Map 的 forEach
Map<String, int> ages = {'Alice': 25, 'Bob': 30};
ages.forEach((key, value) {
  print('$key: $value');
});
// 输出:
// Alice: 25
// Bob: 30
```

**特点**：
- ✅ 函数式编程风格
- ✅ 链式调用友好
- ❌ 无法使用 break/continue
- ❌ 无法直接获取索引

**使用场景**：
- 简单的遍历操作
- 函数式编程风格
- 不需要中断循环的场景

---

## 4. for-in 配合索引

### 使用 asMap() 获取索引

**语法**：
```dart
for (var entry in list.asMap().entries) {
  int index = entry.key;
  var item = entry.value;
  // 循环体
}
```

**示例**：
```dart
List<String> fruits = ['苹果', '香蕉', '橙子'];

// 方法 1: 使用 asMap().entries
for (var entry in fruits.asMap().entries) {
  int index = entry.key;
  String fruit = entry.value;
  print('$index: $fruit');
}
// 输出:
// 0: 苹果
// 1: 香蕉
// 2: 橙子

// 方法 2: 使用传统 for 循环（更直接）
for (int i = 0; i < fruits.length; i++) {
  print('$i: ${fruits[i]}');
}
```

**特点**：
- ✅ 结合了 for-in 的简洁和索引的便利
- ✅ 适合需要索引但想用 for-in 的场景

---

## 5. while 和 do-while 循环

### while 循环

**语法**：
```dart
while (条件) {
  // 循环体
}
```

**示例**：
```dart
int count = 0;
while (count < 5) {
  print(count); // 输出: 0, 1, 2, 3, 4
  count++;
}
```

**特点**：
- ✅ 条件在循环前检查
- ✅ 如果条件一开始就为 false，循环体不会执行

### do-while 循环

**语法**：
```dart
do {
  // 循环体
} while (条件);
```

**示例**：
```dart
int count = 0;
do {
  print(count); // 输出: 0, 1, 2, 3, 4
  count++;
} while (count < 5);
```

**特点**：
- ✅ 循环体至少执行一次
- ✅ 条件在循环后检查

**使用场景**：
- 需要至少执行一次的场景
- 条件判断依赖于循环体执行结果

---

## 6. 循环控制语句

### break - 跳出循环

**示例**：
```dart
for (int i = 0; i < 10; i++) {
  if (i == 5) {
    break; // 跳出循环
  }
  print(i); // 输出: 0, 1, 2, 3, 4
}
```

### continue - 跳过本次循环

**示例**：
```dart
for (int i = 0; i < 10; i++) {
  if (i % 2 == 0) {
    continue; // 跳过偶数
  }
  print(i); // 输出: 1, 3, 5, 7, 9
}
```

### 标签 break/continue - 跳出多层循环

**示例**：
```dart
outerLoop: // 标签
for (int i = 0; i < 3; i++) {
  for (int j = 0; j < 3; j++) {
    if (i == 1 && j == 1) {
      break outerLoop; // 跳出外层循环
    }
    print('i=$i, j=$j');
  }
}
// 输出:
// i=0, j=0
// i=0, j=1
// i=0, j=2
// i=1, j=0
```

---

## 7. 集合的 map、where、reduce 等方法

### map - 转换每个元素

**语法**：
```dart
var newList = list.map((item) => transform(item)).toList();
```

**示例**：
```dart
List<int> numbers = [1, 2, 3, 4, 5];
List<int> doubled = numbers.map((n) => n * 2).toList();
print(doubled); // 输出: [2, 4, 6, 8, 10]
```

### where - 过滤元素

**语法**：
```dart
var filtered = list.where((item) => condition).toList();
```

**示例**：
```dart
List<int> numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
List<int> evens = numbers.where((n) => n % 2 == 0).toList();
print(evens); // 输出: [2, 4, 6, 8, 10]
```

### reduce - 累积计算

**语法**：
```dart
var result = list.reduce((acc, item) => operation(acc, item));
```

**示例**：
```dart
List<int> numbers = [1, 2, 3, 4, 5];
int sum = numbers.reduce((acc, n) => acc + n);
print(sum); // 输出: 15
```

### fold - 带初始值的累积

**语法**：
```dart
var result = list.fold(initialValue, (acc, item) => operation(acc, item));
```

**示例**：
```dart
List<int> numbers = [1, 2, 3, 4, 5];
int sum = numbers.fold(0, (acc, n) => acc + n);
print(sum); // 输出: 15

// 字符串拼接
List<String> words = ['Hello', 'World', 'Flutter'];
String sentence = words.fold('', (acc, word) => acc.isEmpty ? word : '$acc $word');
print(sentence); // 输出: Hello World Flutter
```

### any - 判断是否有元素满足条件

**示例**：
```dart
List<int> numbers = [1, 2, 3, 4, 5];
bool hasEven = numbers.any((n) => n % 2 == 0);
print(hasEven); // 输出: true
```

### every - 判断是否所有元素都满足条件

**示例**：
```dart
List<int> numbers = [2, 4, 6, 8, 10];
bool allEven = numbers.every((n) => n % 2 == 0);
print(allEven); // 输出: true
```

---

## 8. 在 Flutter Widget 中使用循环

### 在 children 中使用展开运算符

**示例**：
```dart
Column(
  children: [
    // 使用展开运算符展开循环结果
    ...List.generate(5, (index) => Text('Item $index')),
  ],
)
```

### 使用 List.generate

**语法**：
```dart
List.generate(count, (index) => widget)
```

**示例**：
```dart
GridView.count(
  crossAxisCount: 3,
  children: List.generate(
    9,
    (index) => Container(
      color: Colors.blue,
      child: Center(child: Text('Item $index')),
    ),
  ),
)
```

### 使用 map 转换数据为 Widget

**示例**：
```dart
List<String> items = ['苹果', '香蕉', '橙子'];

Column(
  children: items.map((item) => ListTile(
    title: Text(item),
  )).toList(),
)
```

### 在 build 方法中使用循环

**示例**：
```dart
@override
Widget build(BuildContext context) {
  List<Widget> widgets = [];
  
  for (int i = 0; i < 5; i++) {
    widgets.add(Text('Item $i'));
  }
  
  return Column(children: widgets);
}
```

---

## 9. 性能对比和最佳实践

### 性能对比

| 方法 | 性能 | 可读性 | 灵活性 | 使用场景 |
|------|------|--------|--------|---------|
| 传统 for | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | 需要索引、精确控制 |
| for-in | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | 简单遍历 |
| forEach | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ | 函数式风格 |
| map/where | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | 数据转换、过滤 |
| List.generate | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | 生成 Widget 列表 |

### 最佳实践

1. **需要索引时**：使用传统 `for` 循环
   ```dart
   for (int i = 0; i < list.length; i++) {
     // 使用 i 和 list[i]
   }
   ```

2. **简单遍历时**：使用 `for-in`
   ```dart
   for (var item in list) {
     // 处理 item
   }
   ```

3. **数据转换时**：使用 `map`
   ```dart
   var newList = list.map((item) => transform(item)).toList();
   ```

4. **过滤数据时**：使用 `where`
   ```dart
   var filtered = list.where((item) => condition).toList();
   ```

5. **生成 Widget 列表时**：使用 `List.generate` 或展开运算符
   ```dart
   Column(
     children: List.generate(5, (i) => Text('Item $i')),
   )
   ```

6. **需要中断循环时**：使用传统 `for` 或 `for-in`（`forEach` 不支持 break/continue）

### 常见错误

❌ **错误**：在 forEach 中使用 break
```dart
list.forEach((item) {
  if (condition) {
    break; // ❌ 错误！forEach 不支持 break
  }
});
```

✅ **正确**：使用 for-in
```dart
for (var item in list) {
  if (condition) {
    break; // ✅ 正确
  }
}
```

---

## 总结

Dart 提供了多种循环方式，选择合适的方式可以让代码更清晰、更高效：

- **传统 for**：需要索引、精确控制
- **for-in**：简单遍历，代码简洁
- **forEach**：函数式风格，简单操作
- **map/where**：数据转换和过滤
- **List.generate**：生成 Widget 列表

根据具体场景选择最合适的循环方式！
