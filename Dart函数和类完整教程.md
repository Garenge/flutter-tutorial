# Dart 函数和类完整教程

> 一份清晰、完整的 Dart 函数和类教程，涵盖所有重要概念和实际应用

---

## 目录

1. [函数基础](#1-函数基础)
   - [普通函数](#普通函数)
   - [箭头函数](#箭头函数)
2. [可选参数](#2-可选参数)
   - [可选位置参数](#可选位置参数)
   - [可选命名参数](#可选命名参数)
   - [默认值](#默认值)
   - [为什么需要花括号](#为什么需要花括号)
3. [类基础](#3-类基础)
   - [类定义 vs 对象创建](#类定义-vs-对象创建)
   - [构造函数](#构造函数)
   - [构造函数中使用可选参数](#构造函数中使用可选参数)
4. [常见问题解答](#4-常见问题解答)
5. [最佳实践](#5-最佳实践)

---

## 1. 函数基础

### 普通函数

**语法**：
```dart
String greet(String name) {
  return 'Hello, $name!';
}
```

**特点**：
- ✅ 可以包含多行代码
- ✅ 可以有多个 return 语句
- ✅ 适合处理复杂业务逻辑

**使用**：
```dart
String result = greet('Flutter');
print(result); // 输出: Hello, Flutter!
```

### 箭头函数

**语法**：
```dart
String greet(String name) => 'Hello, $name!';
```

**特点**：
- ✅ 语法简洁，代码更短
- ✅ 自动返回表达式的值
- ❌ 只能包含一个表达式
- ❌ 不能使用多条语句

**等价写法**：
```dart
// 箭头函数
int add(int a, int b) => a + b;

// 等价于普通函数
int add(int a, int b) {
  return a + b;
}
```

**使用场景**：
- 简单计算：`int double(int x) => x * 2;`
- 判断逻辑：`bool isEven(int n) => n % 2 == 0;`

---

## 2. 可选参数

### 可选位置参数

**语法**：使用方括号 `[]`

```dart
void printInfo(String name, [int? age, String? city]) {
  print('Name: $name');
  if (age != null) print('Age: $age');
  if (city != null) print('City: $city');
}
```

**使用**：
```dart
printInfo('Alice');                    // 只传 name
printInfo('Bob', 25);                  // 传 name 和 age
printInfo('Charlie', 30, 'Beijing');   // 传所有参数
```

**限制**：
- ❌ 必须按顺序传递
- ❌ 不能跳过中间参数
- ❌ 不能按名称传递

### 可选命名参数

**语法**：使用花括号 `{}`

```dart
void printInfo(String name, {int? age, String? city}) {
  print('Name: $name');
  if (age != null) print('Age: $age');
  if (city != null) print('City: $city');
}
```

**使用**：
```dart
printInfo('Alice');                          // 只传 name
printInfo('Bob', age: 25);                   // 传 name 和 age
printInfo('Charlie', city: 'Beijing');       // 传 name 和 city（跳过 age）
printInfo('David', age: 30, city: 'Shanghai'); // 传所有参数
```

**优势**：
- ✅ 可以按名称传递
- ✅ 可以跳过中间参数
- ✅ 顺序不重要

### 默认值

**语法**：使用 `= 默认值`

```dart
void calculate(int a, [int b = 0, int c = 0]) {
  print('a=$a, b=$b, c=$c');
}
```

**使用**：
```dart
calculate(1);        // a=1, b=0, c=0（b 和 c 使用默认值）
calculate(1, 2);     // a=1, b=2, c=0（c 使用默认值）
calculate(1, 2, 3);   // a=1, b=2, c=3
```

**对比**：

| 方式 | 类型 | 默认值处理 | 推荐度 |
|------|------|-----------|--------|
| 默认值 | `int b = 0` | 自动处理 | ⭐⭐⭐⭐⭐ |
| 可空类型 | `int? b` | 手动检查 | ⭐⭐⭐ |

### 为什么需要花括号

**核心区别**：
- **可选位置参数 `[]`**：按**位置**识别（第1个、第2个...）
- **可选命名参数 `{}`**：按**名称**识别（`age:`, `city:`）

**为什么位置参数不能按名称传递？**

```dart
void func(int a, [int? b, int? c]) { ... }

func(1, 2, 3);        // ✅ 可以：按位置传递
// func(1, c: 3);     // ❌ 错误：编译器不知道 c 是什么
```

**原因**：位置参数通过位置识别，编译器无法理解 `c: 3` 中的 `c`。

**为什么命名参数可以跳过中间参数？**

```dart
void func(int a, {int? b, int? c}) { ... }

func(1, c: 3);        // ✅ 可以：通过名称识别
```

**原因**：命名参数通过名称识别，`c: 3` 明确表示传给 `c`。

**使用建议**：
- 参数少、有逻辑顺序 → 用位置参数 `[]`
- 参数多、需要灵活 → 用命名参数 `{}`
- 需要跳过中间参数 → 必须用命名参数 `{}`

---

## 3. 类基础

### 类定义 vs 对象创建

**重要概念**：
- **类定义** ≠ **对象创建**
- `class Person { ... }` 只是定义了类的"模板"（蓝图）
- 不会创建实际对象

**示例**：
```dart
// 1. 定义类（模板）
class Person {
  String name;
  int age;
  
  Person(this.name, this.age);
  
  void introduce() {
    print('I am $name, $age years old');
  }
}

// 2. 创建对象（调用构造函数）
Person alice = Person('Alice', 25);
Person bob = Person('Bob', 30);

// 3. 使用对象
alice.introduce();  // 输出: I am Alice, 25 years old
bob.introduce();    // 输出: I am Bob, 30 years old
```

### 构造函数

**语法**：`Person(this.name, this.age)` 是 Dart 的简写形式（语法糖）

**等价写法**：
```dart
// 简写形式
Person(this.name, this.age);

// 完整写法（等价）
Person(String name, int age) {
  this.name = name;
  this.age = age;
}
```

### 构造函数中使用可选参数

#### 方式 1：可选位置参数

```dart
class Person {
  String name;
  int? age;
  String? city;
  
  Person(this.name, [this.age, this.city]);
  
  void introduce() {
    print('I am $name');
    if (age != null) print('Age: $age');
    if (city != null) print('City: $city');
  }
}

// 使用
Person p1 = Person('Alice');                    // 只传 name
Person p2 = Person('Bob', 25);                  // 传 name 和 age
Person p3 = Person('Charlie', 30, 'Beijing');   // 传所有参数
```

#### 方式 2：可选命名参数（推荐）

```dart
class Person {
  String name;
  int? age;
  String? city;
  
  Person(this.name, {this.age, this.city});
  
  void introduce() {
    print('I am $name');
    if (age != null) print('Age: $age');
    if (city != null) print('City: $city');
  }
}

// 使用
Person p1 = Person('Alice');                          // 只传 name
Person p2 = Person('Bob', age: 25);                   // 传 name 和 age
Person p3 = Person('Charlie', city: 'Beijing');       // 传 name 和 city（跳过 age）
Person p4 = Person('David', age: 30, city: 'Shanghai'); // 传所有参数
```

#### 方式 3：默认值

```dart
class Person {
  String name;
  int age;
  String city;
  
  Person(this.name, [this.age = 0, this.city = 'Unknown']);
  
  void introduce() {
    print('I am $name, $age years old, from $city');
  }
}

// 使用
Person p1 = Person('Alice');                    // age=0, city='Unknown'
Person p2 = Person('Bob', 25);                  // city='Unknown'
Person p3 = Person('Charlie', 30, 'Beijing');  // 传所有参数
```

---

## 4. 常见问题解答

### Q1: 为什么可选参数需要 `?`（可空类型）？

**答案**：`[]` 和 `?` 是两个不同的概念

| 符号 | 作用层面 | 含义 |
|------|---------|------|
| `[]` | **语法层面** | 告诉编译器：这个参数在**调用时可以省略** |
| `?` | **类型层面** | 告诉编译器：这个变量的**类型允许为 null** |

**原因**：
- `[]` 允许调用时省略参数
- 省略时，参数值自动为 `null`
- 但 `int` 类型不允许 `null` 值
- 所以类型必须是 `int?`（可空类型）

**类比**：
- `[]` = "这个座位可以空着"（语法规则）
- `?` = "这个座位允许没有人"（类型规则）

### Q2: 可选位置参数能否跳过中间参数？

**答案**：不能！如果需要跳过中间参数，使用可选命名参数。

```dart
// ❌ 不能这样做
void func(int a, [int? b, int? c]) {
  // func(1, , 3);        // 语法错误
  // func(1, c: 3);      // 错误：不能按名称传递
}

// ✅ 解决方案：使用可选命名参数
void func(int a, {int? b, int? c}) {
  // ...
}

func(1, c: 3);        // ✅ 可以跳过 b，只传 c
```

### Q3: 能否在可选参数后面再加必需参数？

**答案**：不能！

```dart
// ❌ 错误
void badFunction(String name, [int? age], String city) {
  // 编译错误：可选位置参数后面不能有必需参数
}

// ✅ 解决方案：使用可选命名参数
void goodFunction(String name, {int? age, required String city}) {
  // ...
}
```

### Q4: 方括号 `[]` 还有其他用途吗？

**答案**：有！根据上下文不同，含义也不同：

| 上下文 | 用途 | 示例 |
|--------|------|------|
| 函数参数 | 可选位置参数 | `void func(String name, [int? age])` |
| 列表字面量 | 创建列表 | `List<int> nums = [1, 2, 3]` |
| 列表索引 | 访问元素 | `nums[0] = 10` |
| Map 访问 | 访问值 | `map['key'] = value` |

---

## 5. 最佳实践

### 函数选择

1. **简单计算** → 使用箭头函数
   ```dart
   int double(int x) => x * 2;
   ```

2. **复杂逻辑** → 使用普通函数
   ```dart
   String formatUser(User user) {
     // 多行处理逻辑
     return '...';
   }
   ```

### 参数选择

1. **参数少、有逻辑顺序** → 位置参数 `[]`
   ```dart
   double calculateVolume(double length, [double width = 1.0, double height = 1.0])
   ```

2. **参数多、需要灵活** → 命名参数 `{}`
   ```dart
   void createUser({required String name, int? age, String? email})
   ```

3. **有合理默认值** → 使用默认值
   ```dart
   Person(this.name, [this.age = 0, this.city = 'Unknown'])
   ```

### 构造函数选择

1. **参数少** → 位置参数
2. **参数多** → 命名参数（推荐）
3. **需要跳过中间参数** → 必须用命名参数

---

## 快速参考

### 参数类型对比

| 写法 | 参数是否必需 | 值可以为 null | 使用场景 |
|------|------------|--------------|---------|
| `int age` | ✅ 必需 | ❌ 不能 | 必须有值 |
| `int? age` | ✅ 必需 | ✅ 可以 | 必须传，但可以传 null |
| `[int? age]` | ❌ 可选 | ✅ 可以 | 可以省略，省略时为 null |
| `{int? age}` | ❌ 可选 | ✅ 可以 | 可以省略，可以跳过中间参数 |
| `[int age = 0]` | ❌ 可选 | ❌ 不能 | 可以省略，使用默认值 |

### 函数类型对比

| 特性 | 普通函数 | 箭头函数 |
|------|---------|---------|
| 语法 | `{}` 大括号 | `=>` 箭头 |
| 多行代码 | ✅ 支持 | ❌ 不支持 |
| return 语句 | ✅ 需要 | ❌ 自动返回 |
| 适用场景 | 复杂逻辑 | 简单表达式 |

---

## 6. 实际应用场景

### 场景 1：用户信息管理

```dart
class User {
  final String name;        // 必需
  final int age;            // 必需
  final String? email;      // 可选
  final String? phone;      // 可选
  final String role;        // 有默认值
  
  User({
    required this.name,
    required this.age,
    this.email,
    this.phone,
    this.role = 'user',  // 默认值
  });
  
  void display() {
    print('用户信息：');
    print('  姓名: $name');
    print('  年龄: $age');
    if (email != null) print('  邮箱: $email');
    if (phone != null) print('  电话: $phone');
    print('  角色: $role');
  }
}

// 使用
User user1 = User(name: 'Alice', age: 25);
User user2 = User(name: 'Bob', age: 30, email: 'bob@example.com');
User user3 = User(
  name: 'Charlie',
  age: 28,
  email: 'charlie@example.com',
  phone: '123456789',
  role: 'admin',
);
```

### 场景 2：商品信息管理

```dart
class Product {
  final String name;
  final double price;
  final int? stock;        // 可选：库存数量
  final String category;   // 有默认值
  
  Product({
    required this.name,
    required this.price,
    this.stock,
    this.category = 'General',
  });
  
  void display() {
    print('商品信息：');
    print('  名称: $name');
    print('  价格: \$$price');
    if (stock != null) {
      print('  库存: $stock');
    } else {
      print('  库存: 未知');
    }
    print('  分类: $category');
  }
}

// 使用
Product product1 = Product(name: 'Laptop', price: 999.99);
Product product2 = Product(
  name: 'Mouse',
  price: 29.99,
  stock: 100,
  category: 'Electronics',
);
```

### 场景 3：计算工具函数

```dart
// 计算矩形体积：有逻辑顺序，使用位置参数 + 默认值
double calculateVolume(double length, [double width = 1.0, double height = 1.0]) {
  return length * width * height;
}

// 使用
calculateVolume(5);           // 5.0
calculateVolume(5, 3);        // 15.0
calculateVolume(5, 3, 2);     // 30.0

// 发送邮件：参数多，需要灵活，使用命名参数
void sendEmail({
  required String to,
  String? subject,
  String? body,
  List<String>? attachments,
}) {
  print('发送邮件到: $to');
  if (subject != null) print('  主题: $subject');
  if (body != null) print('  内容: $body');
  if (attachments != null) print('  附件: ${attachments.length} 个');
}

// 使用
sendEmail(to: 'alice@example.com');
sendEmail(to: 'bob@example.com', body: 'Hello!');
sendEmail(
  to: 'charlie@example.com',
  subject: '重要通知',
  body: '请查看附件',
  attachments: ['file1.pdf', 'file2.doc'],
);
```

---

## 总结

- **函数**：普通函数处理复杂逻辑，箭头函数处理简单计算
- **可选参数**：位置参数简洁但有限制，命名参数灵活但稍长
- **类**：类定义是模板，构造函数定义如何创建对象
- **最佳实践**：根据实际需求选择合适的参数类型和函数形式

---

*最后更新：2024*

