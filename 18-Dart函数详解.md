# Dart 函数详解

## 1. 普通函数（Regular Function）

### 语法结构

```dart
// 普通函数
String greet(String name) {
  return 'Hello, $name!';
}
```

### 详细说明

- **函数名**：`greet` - 函数的标识符
- **返回类型**：`String` - 函数返回值的类型（如果函数不返回值，使用 `void`）
- **参数列表**：`(String name)` - 函数接收的参数，`name` 是参数名，`String` 是参数类型
- **函数体**：`{ return 'Hello, $name!'; }` - 用大括号包裹的代码块
- **return 语句**：显式返回结果

### 使用示例

```dart
void main() {
  String result = greet('Flutter');
  print(result); // 输出: Hello, Flutter!
}
```

### 特点

- ✅ 可以包含多行代码
- ✅ 可以有多个 return 语句
- ✅ 可以执行复杂的逻辑
- ✅ 适合处理复杂业务逻辑

---

## 2. 箭头函数（Arrow Function / Expression Function）

### 语法结构

```dart
// 箭头函数
String greet(String name) => 'Hello, $name!';
```

### 详细说明

- **`=>`**：箭头符号，表示这是一个表达式函数
- **单表达式**：箭头函数只能包含一个表达式，不能使用大括号 `{}`
- **自动返回**：箭头函数会自动返回表达式的值，不需要 `return` 关键字
- **等价写法**：上面的箭头函数等价于普通函数：

```dart
String greet(String name) {
  return 'Hello, $name!';
}
```

### 使用示例

```dart
void main() {
  String result = greet('Dart');
  print(result); // 输出: Hello, Dart!
}
```

### 特点

- ✅ 语法简洁，代码更短
- ✅ 适合简单的单行函数
- ❌ 只能包含一个表达式
- ❌ 不能使用多条语句
- ❌ 不能使用 `return` 关键字

### 更多箭头函数示例

```dart
// 计算两数之和
int add(int a, int b) => a + b;

// 判断是否为偶数
bool isEven(int number) => number % 2 == 0;

// 获取字符串长度
int getLength(String text) => text.length;

// 无返回值也可以使用箭头函数
void printMessage(String msg) => print(msg);
```

---

## 3. 可选参数（Optional Parameters）

### 语法结构

```dart
// 可选参数（使用方括号 []）
void printInfo(String name, [int? age]) {
  print('Name: $name');
  if (age != null) {
    print('Age: $age');
  }
}
```

### 详细说明

- **方括号 `[]`**：表示参数是可选的，可以传也可以不传
- **`int?`**：`?` 表示可空类型（nullable），因为可选参数可能为 `null`
- **参数检查**：使用 `if (age != null)` 来检查参数是否被传入

### 使用示例

```dart
void main() {
  // 只传必需参数
  printInfo('Alice');
  // 输出:
  // Name: Alice
  
  // 传必需参数和可选参数
  printInfo('Bob', 25);
  // 输出:
  // Name: Bob
  // Age: 25
}
```

### Dart 中的三种参数类型

#### 1. 必需参数（Required Parameters）

```dart
void greet(String name) {
  print('Hello, $name!');
}

// 调用时必须提供参数
greet('Flutter'); // ✅ 正确
// greet(); // ❌ 错误：缺少必需参数
```

#### 2. 可选位置参数（Optional Positional Parameters）

使用方括号 `[]` 包裹：

```dart
void printInfo(String name, [int? age, String? city]) {
  print('Name: $name');
  if (age != null) {
    print('Age: $age');
  }
  if (city != null) {
    print('City: $city');
  }
}

// 调用示例
printInfo('Alice');                    // 只传 name
printInfo('Bob', 25);                  // 传 name 和 age
printInfo('Charlie', 30, 'Beijing');   // 传所有参数
```

**注意**：
1. 可选位置参数必须按顺序传递，**不能跳过中间的参数**
2. **可选位置参数必须放在所有必需参数之后**，不能有必需参数在可选参数之后
3. 一旦开始使用可选位置参数，后面的所有位置参数都必须是可选的

##### 📌 可选参数可以使用默认值（不需要可空类型）

**问题**：可选参数是否必须使用可空类型 `?`？

**答案**：不是！如果参数有合理的默认值，可以使用默认值，这样就不需要可空类型了。

```dart
// ✅ 推荐：使用默认值
void calculate(int a, [int b = 0, int c = 0]) {
  print('a=$a, b=$b, c=$c');
}

calculate(1);        // a=1, b=0, c=0（b 和 c 使用默认值）
calculate(1, 2);     // a=1, b=2, c=0（c 使用默认值）
calculate(1, 2, 3);   // a=1, b=2, c=3

// ❌ 不推荐：如果不需要 null，使用可空类型会增加复杂度
void calculate2(int a, [int? b, int? c]) {
  int bValue = b ?? 0;  // 需要手动处理 null
  int cValue = c ?? 0;
  print('a=$a, b=$bValue, c=$cValue');
}
```

**对比**：

| 方式 | 类型 | 默认值处理 | 使用场景 | 推荐度 |
|------|------|-----------|---------|--------|
| 默认值 | `int b = 0` | 自动处理 | 有合理默认值时 | ⭐⭐⭐⭐⭐ |
| 可空类型 | `int? b` | 手动检查 | 需要区分"未提供"和"值为 null"时 | ⭐⭐⭐ |

##### ❌ 不能跳过中间的可选位置参数

**问题**：如何实现 `func(1, , 3)` 这样的调用（跳过中间的参数）？

**答案**：可选位置参数**不能跳过中间参数**。如果需要这种灵活性，使用可选命名参数。

```dart
// ❌ 不能这样做：跳过中间参数
void func(int a, [int? b, int? c]) {
  // ...
}

// func(1, , 3);        // ❌ 语法错误
// func(1, null, 3);    // ✅ 可以，但不优雅

// ✅ 解决方案：使用可选命名参数
void func2(int a, {int? b, int? c}) {
  // ...
}

func2(1, c: 3);        // ✅ 可以跳过 b，只传 c
func2(1, b: 2, c: 3);  // ✅ 也可以都传
```

**推荐做法**：如果需要跳过中间参数，使用可选命名参数。

##### ❌ 错误示例：不能在可选参数后面再加必需参数

```dart
// ❌ 错误：不能在可选参数后面再加必需参数
void badFunction(String name, [int? age], String city) {
  // 编译错误：可选位置参数后面不能有必需参数
}
```

**原因**：如果允许这样做，调用时会出现歧义：
```dart
badFunction('Alice', 'Beijing');  
// 编译器无法判断：'Beijing' 是传给 age 还是 city？
```

##### ✅ 解决方案

**方案 1：使用可选命名参数（推荐）**

```dart
// ✅ 正确：使用可选命名参数
void printInfo(String name, {int? age, required String city}) {
  print('Name: $name');
  if (age != null) {
    print('Age: $age');
  }
  print('City: $city');  // city 是必需的
}

// 调用示例
printInfo('Alice', city: 'Beijing');              // ✅ 只传必需参数
printInfo('Bob', age: 25, city: 'Shanghai');       // ✅ 传所有参数
```

**方案 2：重新设计参数顺序**

```dart
// ✅ 正确：把必需参数放在前面，可选参数放在后面
void printInfo(String name, String city, [int? age]) {
  print('Name: $name');
  print('City: $city');
  if (age != null) {
    print('Age: $age');
  }
}

// 调用示例
printInfo('Alice', 'Beijing');           // ✅ 只传必需参数
printInfo('Bob', 'Shanghai', 25);        // ✅ 传所有参数
```

**方案 3：使用多个函数**

```dart
// ✅ 正确：为不同场景提供不同的函数
void printInfo(String name, String city) {
  printInfoWithAge(name, city, null);
}

void printInfoWithAge(String name, String city, int? age) {
  print('Name: $name');
  print('City: $city');
  if (age != null) {
    print('Age: $age');
  }
}

// 调用示例
printInfo('Alice', 'Beijing');                    // ✅ 不传 age
printInfoWithAge('Bob', 'Shanghai', 25);          // ✅ 传 age
```

##### 📌 关于方括号 `[]` 的其他用途

在 Dart 中，方括号 `[]` 有多种用途，**根据上下文不同，含义也不同**：

1. **函数参数中的可选位置参数**（当前用法）
   ```dart
   void func(String name, [int? age]) { ... }
   ```
   这里的 `[]` 表示参数是可选的。

2. **列表字面量（List Literals）**
   ```dart
   List<int> numbers = [1, 2, 3, 4, 5];
   List<String> names = ['Alice', 'Bob', 'Charlie'];
   ```
   这里的 `[]` 用于创建列表。

3. **列表索引访问**
   ```dart
   List<int> numbers = [10, 20, 30];
   int first = numbers[0];  // 访问第一个元素：10
   numbers[1] = 25;         // 修改第二个元素
   ```
   这里的 `[]` 用于访问或修改列表中的元素。

4. **Map 字面量（虽然 Map 用花括号 `{}`，但访问时也用 `[]`）**
   ```dart
   Map<String, int> scores = {'Alice': 95, 'Bob': 87};
   int aliceScore = scores['Alice'];  // 访问：95
   scores['Charlie'] = 90;            // 添加新元素
   ```
   这里的 `[]` 用于访问或修改 Map 中的值。

**关键区别**：
- 在**函数参数列表**中：`[int? age]` → 表示可选位置参数
- 在**代码执行**中：`list[0]` → 表示访问列表/Map 的元素
- 在**变量初始化**中：`[1, 2, 3]` → 表示创建列表

**为什么可选参数需要 `?`（可空类型）？**

这是一个很好的问题！很多人会疑惑：既然 `[]` 已经表示参数可选了，为什么还要加 `?`？

### 📌 `[]` 和 `?` 是两个不同的概念

| 符号 | 作用层面 | 含义 |
|------|---------|------|
| `[]` | **语法层面** | 告诉编译器：这个参数在**调用时可以省略** |
| `?` | **类型层面** | 告诉编译器：这个变量的**类型允许为 null** |

### 🔍 详细解释

#### 1. `[]` 的作用：控制调用时的行为

```dart
void printInfo(String name, [int? age]) {
  // ...
}

// ✅ 可以这样调用（省略 age）
printInfo('Alice');

// ✅ 也可以这样调用（提供 age）
printInfo('Bob', 25);
```

`[]` 只是告诉编译器："调用这个函数时，可以省略这个参数"。

#### 2. `?` 的作用：控制类型系统

```dart
void printInfo(String name, [int? age]) {
  // 如果不传 age，它的值是什么？
  // 答案是：null
}

printInfo('Alice');  // age 的值是 null（不是 0，不是空字符串，就是 null）
```

当参数被省略时，Dart 会**自动将它的值设为 `null`**。因此，类型必须是可空的。

#### 3. 为什么必须同时使用？

**❌ 如果只用 `[]` 不用 `?`：**

```dart
void printInfo(String name, [int age]) {  // ❌ 编译错误！
  // 错误信息：
  // "The parameter 'age' can't have a value of 'null' because of its type 'int'"
}

printInfo('Alice');  // age 的值是 null，但类型是 int（不能为 null）
```

**原因**：
- `[]` 允许调用时省略参数
- 省略时，参数值自动为 `null`
- 但 `int` 类型不允许 `null` 值
- 所以类型必须是 `int?`（可空类型）

**✅ 正确的写法：**

```dart
void printInfo(String name, [int? age]) {  // ✅ 正确
  // age 的类型是 int?，可以为 null
}

printInfo('Alice');  // age 为 null，类型匹配 ✅
printInfo('Bob', 25); // age 为 25，类型匹配 ✅
```

### 💡 类比理解

想象一下：
- `[]` = "这个座位可以空着"（语法规则）
- `?` = "这个座位允许没有人"（类型规则）

如果座位可以空着，那座位本身必须允许"没有人"的状态。

### 🎯 实际例子对比

```dart
// 情况 1：可选参数（必须用 ?）
void func1(String name, [int? age]) {
  print('Name: $name');
  print('Age: ${age ?? "未提供"}');  // age 可能为 null
}

func1('Alice');        // age 为 null
func1('Bob', 25);      // age 为 25

// 情况 2：必需参数，但类型可空（不需要 []）
void func2(String name, int? age) {  // 注意：没有 []
  print('Name: $name');
  print('Age: ${age ?? "未提供"}');
}

func2('Alice', null);  // ✅ 必须传参数，但可以传 null
func2('Bob', 25);      // ✅ 传实际值
// func2('Charlie');    // ❌ 错误：缺少必需参数 age
```

**区别**：
- `[int? age]`：参数可以省略，省略时自动为 `null`
- `int? age`：参数必须提供，但可以传 `null` 值

### 📝 总结

| 写法 | 参数是否必需 | 值可以为 null | 使用场景 |
|------|------------|--------------|---------|
| `int age` | ✅ 必需 | ❌ 不能 | 必须有值 |
| `int? age` | ✅ 必需 | ✅ 可以 | 必须传，但可以传 null |
| `[int? age]` | ❌ 可选 | ✅ 可以 | 可以省略，省略时为 null |

**关键点**：
- `[]` 控制**调用行为**（能否省略）
- `?` 控制**类型系统**（能否为 null）
- 可选参数省略时值为 `null`，所以类型必须是可空的
- **这不是多此一举，而是 Dart 类型安全的要求**

#### 3. 可选命名参数（Optional Named Parameters）

使用花括号 `{}` 包裹：

```dart
void printInfo(String name, {int? age, String? city}) {
  print('Name: $name');
  if (age != null) {
    print('Age: $age');
  }
  if (city != null) {
    print('City: $city');
  }
}

// 调用示例
printInfo('Alice');                              // 只传 name
printInfo('Bob', age: 25);                       // 传 name 和 age
printInfo('Charlie', city: 'Beijing');           // 传 name 和 city
printInfo('David', age: 30, city: 'Shanghai');  // 传所有参数
```

**优势**：可以按名称传递，顺序不重要，可以只传需要的参数。

#### 4. 必需命名参数（Required Named Parameters）

使用 `required` 关键字：

```dart
void printInfo(String name, {required int age, String? city}) {
  print('Name: $name');
  print('Age: $age');
  if (city != null) {
    print('City: $city');
  }
}

// 调用示例
printInfo('Alice', age: 25);           // ✅ 正确：age 是必需的
// printInfo('Bob');                    // ❌ 错误：缺少必需的 age 参数
printInfo('Charlie', age: 30, city: 'Beijing'); // ✅ 正确
```

### ⚠️ 重要限制

#### 限制 1：可选位置参数必须在所有必需参数之后

**❌ 错误示例**：不能在可选参数后面再加必需参数

```dart
// ❌ 编译错误：可选位置参数后面不能有必需参数
void badFunction(String name, [int? age], String city) {
  // 错误信息：
  // "Optional positional parameters must be last in the parameter list"
}
```

**原因**：如果允许这样做，调用时会出现歧义：
```dart
badFunction('Alice', 'Beijing');  
// 编译器无法判断：'Beijing' 是传给 age 还是 city？
```

**✅ 解决方案**：

1. **使用可选命名参数（最推荐）**
   ```dart
   void printInfo(String name, {int? age, required String city}) {
     print('Name: $name');
     if (age != null) print('Age: $age');
     print('City: $city');
   }
   
   // 调用
   printInfo('Alice', city: 'Beijing');              // ✅ 只传必需参数
   printInfo('Bob', age: 25, city: 'Shanghai');      // ✅ 传所有参数
   ```

2. **重新设计参数顺序**
   ```dart
   void printInfo(String name, String city, [int? age]) {
     print('Name: $name');
     print('City: $city');
     if (age != null) print('Age: $age');
   }
   
   // 调用
   printInfo('Alice', 'Beijing');           // ✅ 只传必需参数
   printInfo('Bob', 'Shanghai', 25);        // ✅ 传所有参数
   ```

3. **使用多个函数**
   ```dart
   void printInfo(String name, String city) {
     print('Name: $name');
     print('City: $city');
   }
   
   void printInfoWithAge(String name, String city, int age) {
     print('Name: $name');
     print('City: $city');
     print('Age: $age');
   }
   ```

#### 限制 2：不能同时使用可选位置参数和可选命名参数

**Dart 不支持在同一个函数中同时使用可选位置参数 `[]` 和可选命名参数 `{}`**

```dart
// ❌ 错误：不能同时使用
void badFunction(String name, [int? age], {String? email}) {
  // 这会编译错误
}

// ✅ 正确：只使用可选命名参数
void goodFunction1(String name, {int? age, String? email}) {
  // ...
}

// ✅ 正确：只使用可选位置参数
void goodFunction2(String name, [int? age, String? email]) {
  // ...
}
```

**推荐做法**：如果需要灵活性，优先使用可选命名参数，因为：
- 可以按名称传递，顺序不重要
- 可以只传需要的参数
- 代码可读性更好
- 可以灵活指定哪些参数可选，哪些必需

---

## 综合示例

### 示例 1：普通函数 vs 箭头函数

```dart
// 普通函数 - 适合复杂逻辑
int calculate(int a, int b) {
  int sum = a + b;
  int product = a * b;
  return sum + product;
}

// 箭头函数 - 适合简单计算
int add(int a, int b) => a + b;
int multiply(int a, int b) => a * b;
```

### 示例 2：混合使用各种参数类型

```dart
// ⚠️ 注意：Dart 不支持同时使用可选位置参数和可选命名参数
// 如果需要灵活性，推荐使用可选命名参数

// 方式1：使用可选命名参数（推荐）
void createUser(
  String name, {
  int? age,
  String? email,
  String? phone,
}) {
  print('Name: $name');
  if (age != null) print('Age: $age');
  if (email != null) print('Email: $email');
  if (phone != null) print('Phone: $phone');
}

// 调用示例
createUser('Alice');
createUser('Bob', age: 25);
createUser('Charlie', age: 30, email: 'charlie@example.com');
createUser('David', age: 28, phone: '123456789', email: 'david@example.com');

// 方式2：只使用可选位置参数
void createUser2(String name, [int? age, String? email]) {
  print('Name: $name');
  if (age != null) print('Age: $age');
  if (email != null) print('Email: $email');
}

// 调用示例
createUser2('Alice');
createUser2('Bob', 25);
createUser2('Charlie', 30, 'charlie@example.com');
```

### 示例 3：在 Flutter 中的实际应用

```dart
// 普通函数 - 处理复杂逻辑
Widget buildButton(String text, VoidCallback onPressed) {
  return ElevatedButton(
    onPressed: onPressed,
    style: ElevatedButton.styleFrom(
      padding: EdgeInsets.all(16),
      backgroundColor: Colors.blue,
    ),
    child: Text(text),
  );
}

// 箭头函数 - 简单的 getter
String get greeting => 'Hello, Flutter!';
bool get isLoggedIn => _user != null;

// 可选参数 - 灵活的配置
void showDialog({
  required String title,
  String? message,
  VoidCallback? onConfirm,
  VoidCallback? onCancel,
}) {
  // 显示对话框的逻辑
}
```

---

## 总结对比

| 特性 | 普通函数 | 箭头函数 | 可选参数 |
|------|---------|---------|---------|
| 语法 | `{}` 大括号 | `=>` 箭头 | `[]` 或 `{}` |
| 多行代码 | ✅ 支持 | ❌ 不支持 | ✅ 支持 |
| return 语句 | ✅ 需要 | ❌ 自动返回 | ✅ 需要 |
| 适用场景 | 复杂逻辑 | 简单表达式 | 灵活调用 |
| 可读性 | 更清晰（复杂时） | 更简洁（简单时） | 更灵活 |

---

## 最佳实践

1. **简单计算**：使用箭头函数
   ```dart
   int double(int x) => x * 2;
   ```

2. **复杂逻辑**：使用普通函数
   ```dart
   String formatUser(User user) {
     // 多行处理逻辑
     String name = user.name.toUpperCase();
     String email = user.email.toLowerCase();
     return '$name <$email>';
   }
   ```

3. **可选参数**：根据使用场景选择
   - 位置参数：参数有逻辑顺序时使用
   - 命名参数：参数较多或需要灵活组合时使用

4. **可空类型**：可选参数应该使用可空类型 `?`

---

## 附录：方括号 `[]` 在 Dart 中的用途总结

方括号 `[]` 在 Dart 中根据**上下文不同**，有**不同的含义**：

| 上下文 | 用途 | 示例 | 说明 |
|--------|------|------|------|
| **函数参数** | 可选位置参数 | `void func(String name, [int? age])` | 表示参数是可选的，必须使用可空类型 `?` |
| **列表字面量** | 创建列表 | `List<int> nums = [1, 2, 3]` | 用于创建列表对象 |
| **列表索引** | 访问/修改元素 | `nums[0] = 10` | 用于访问或修改列表中的元素 |
| **Map 访问** | 访问/修改值 | `map['key'] = value` | 用于访问或修改 Map 中的值 |

### 为什么可选参数需要 `?`（可空类型）？

```dart
void printInfo(String name, [int? age]) {
  // age 是可选的，如果不传，它的值就是 null
  // 所以必须声明为 int?（可空类型），而不是 int
}

printInfo('Alice');      // age 为 null
printInfo('Bob', 25);    // age 为 25
```

**如果不使用 `?`**：
```dart
void printInfo(String name, [int age]) {  // ❌ 错误！
  // 编译错误：可选参数必须使用可空类型
}
```

**原因**：可选参数在未传入时，值为 `null`。Dart 的类型系统要求：
- 如果变量可能为 `null`，必须声明为可空类型（加 `?`）
- 如果变量不能为 `null`，不能声明为可空类型

### 快速识别方括号的含义

```dart
// 1. 函数定义中 → 可选位置参数
void func(String name, [int? age]) { ... }

// 2. 变量初始化中 → 列表字面量
List<int> numbers = [1, 2, 3];

// 3. 变量名后面 → 索引访问
int first = numbers[0];
numbers[0] = 10;

// 4. Map/变量名后面 → Map 访问
int score = scores['Alice'];
scores['Bob'] = 90;
```

**记忆技巧**：
- 在**参数列表**中：`[]` = 可选参数
- 在**赋值/初始化**中：`[]` = 创建列表
- 在**变量名后**：`[]` = 访问元素/值

