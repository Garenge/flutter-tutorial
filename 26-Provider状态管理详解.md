## 26 - Provider 状态管理详解

在 `06-状态管理` 这一章中，我们简单示例了 `Provider` 的用法，但代码被拆成了几个片段，不太直观。本节我们专门开一章，把 `Provider` 的使用串成一个完整的例子，并解释背后的概念。

---

## 一、Provider 是什么？

- **本质**：一个基于 `InheritedWidget` 的封装，用来在 Widget 树中「向下传递状态」。
- **特点**：
  - 不用一层层通过构造函数传参；
  - 支持细粒度刷新（只重建用到状态的 Widget）；
  - 和 `ChangeNotifier` 搭配使用最常见。

推荐的基本组合是：**`ChangeNotifier` + `ChangeNotifierProvider` + `Consumer`**。

---

## 二、核心角色总览

以一个计数器为例，有三个重要角色：

1. **`CounterModel extends ChangeNotifier`**  
   - 保存数据（`_count`）  
   - 提供修改方法（`increment`）  
   - 修改后调用 `notifyListeners()` 通知 UI 更新

2. **`ChangeNotifierProvider`**  
   - 把 `CounterModel` 实例「挂」到 Widget 树上，供后代 Widget 使用。
   - 一般包在 `runApp` 或 `MaterialApp` 外层：
     ```dart
     void main() {
       runApp(
         ChangeNotifierProvider(
           create: (_) => CounterModel(),
           child: const MyApp(),
         ),
       );
     }
     ```

3. **`Consumer<CounterModel>` / `context.watch<CounterModel>()`**  
   - 订阅 `CounterModel` 的变化，当它 `notifyListeners()` 时自动重建 UI。

---

## 三、完整示例：用 Provider 实现计数器

下面是一个可直接运行的完整示例（假设你已经在 `pubspec.yaml` 中添加了 `provider: ^6.0.0`）。

```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';

// 1. 定义状态 Model
class CounterModel extends ChangeNotifier {
  int _count = 0;
  int get count => _count;

  void increment() {
    _count++;
    notifyListeners(); // 通知所有监听它的 Widget 重建
  }
}

// 2. 在根部提供 Provider
void main() {
  runApp(
    ChangeNotifierProvider(
      create: (_) => CounterModel(),
      child: const MyApp(),
    ),
  );
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Provider Demo',
      theme: ThemeData(primarySwatch: Colors.blue),
      home: const CounterPage(),
    );
  }
}

class CounterPage extends StatelessWidget {
  const CounterPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Provider 计数器')),
      body: Center(
        // 3. 使用 Consumer 订阅 CounterModel
        child: Consumer<CounterModel>(
          // 注意：这里的泛型 <CounterModel> 决定 builder 的第二个参数类型
          builder: (context, counter, child) {
            return Text(
              'Count: ${counter.count}',
              style: const TextStyle(fontSize: 32),
            );
          },
        ),
      ),
      floatingActionButton: FloatingActionButton(
        // 4. 通过 context 读取并修改状态
        onPressed: () {
          // read 表示「只读取一次，不随状态变化而重建」
          context.read<CounterModel>().increment();
        },
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

你可以把这段代码直接复制到一个新的 Flutter 项目里运行，观察点击按钮后文本如何更新。

---

## 四、`Consumer` 的三个参数到底是什么？

在 `06-状态管理` 中我们看到过这段代码：

```dart
Consumer<CounterModel>(
  builder: (context, counter, child) {
    return Text('Count: ${counter.count}');
  },
)
```

`builder` 的函数类型本质上是：

```dart
Widget Function(
  BuildContext context,
  CounterModel value,
  Widget? child,
)
```

结合上面的例子，三个参数分别是：

- **`context`**：  
  当前 Widget 的 `BuildContext`，和你在 `build` 方法里拿到的是同一个。  
  可以用它来：
  - 访问主题：`Theme.of(context)`  
  - 路由跳转：`Navigator.of(context)`  
  - 访问其它 Provider：`context.watch<SomeModel>()` 等。

- **`counter`**（也可以叫 `value`、`model`，只是变量名）：
  - 类型是 `CounterModel`，**实际上就是上面 `ChangeNotifierProvider(create: (_) => CounterModel())` 创建并提供的那个实例**。
  - 当你在 `CounterModel` 中调用 `notifyListeners()` 时，`Consumer` 会重新执行 `builder`，并把同一个 `counter` 实例（但里面的数据已经更新）传进来。

- **`child`**：
  - 一个可选的「不随状态变化而重建」的子 Widget。
  - 通常在这种写法中使用：
    ```dart
    Consumer<CounterModel>(
      builder: (context, counter, child) {
        return Column(
          children: [
            child!, // 这个部分不会因为状态变化而重建
            Text('Count: ${counter.count}'),
          ],
        );
      },
      child: const Text('我是静态的，不会因为计数变化而重建'),
    )
    ```
  - 在我们最开始那个简化版示例里并没有用到 `child`，所以可以先忽略。

---

## 五、`watch` / `read` / `select` 的区别

除了 `Consumer`，在有了 `provider` 包之后，`BuildContext` 还多了这几个扩展方法：

- **`context.watch<T>()`**  
  - 订阅 `T` 的变化，`T.notifyListeners()` 时，使用 `watch` 的 Widget 会重建。
  - 示例：
    ```dart
    @override
    Widget build(BuildContext context) {
      final counter = context.watch<CounterModel>();
      return Text('Count: ${counter.count}');
    }
    ```

- **`context.read<T>()`**  
  - 只读取一次 `T`，**不会因为 `notifyListeners()` 而导致当前 Widget 重建**。
  - 常用于按钮点击等「触发动作但自己不需要重建」的场景：
    ```dart
    onPressed: () {
      context.read<CounterModel>().increment();
    }
    ```

- **`context.select<T, R>(R selector(T value))`**  
  - 从 `T` 中「挑选」某个字段或计算结果 `R`，只有当 `R` 发生变化时才重建当前 Widget。
  - 示例：只关心 `CounterModel.count`，而不是整个 Model：
    ```dart
    @override
    Widget build(BuildContext context) {
      final count = context.select<CounterModel, int>((m) => m.count);
      return Text('Count: $count');
    }
    ```

简单记忆：

- **展示数据**：`watch` / `select`  
- **触发动作**：`read`  
- **更灵活的局部重建**：`Consumer` / `Selector`

---

## 六、多个 Provider 一起用：`MultiProvider`

当你的应用里有多个状态对象时（例如 `CounterModel`、`UserModel`、`ThemeModel` 等），可以用 `MultiProvider` 把它们统一包起来：

```dart
void main() {
  runApp(
    MultiProvider(
      providers: [
        ChangeNotifierProvider(create: (_) => CounterModel()),
        ChangeNotifierProvider(create: (_) => UserModel()),
      ],
      child: const MyApp(),
    ),
  );
}
```

此时，在子树中你既可以：

- `context.watch<CounterModel>()`  
- 也可以 `context.watch<UserModel>()`

每个 Model 的 `notifyListeners()` 只会影响到监听了对应类型的 Widget。

---

## 七、常见坑与排查思路

- **界面不刷新**：
  - 是否继承了 `ChangeNotifier`？
  - 修改状态后是否调用了 `notifyListeners()`？
  - 使用 UI 的地方是不是 `watch` / `Consumer`，而不是 `read`？

- **拿不到 Provider，报错 `ProviderNotFoundException`**：
  - 是否真的用 `ChangeNotifierProvider`/`MultiProvider` 把对应部分包在上层了？
  - 当前 Widget 的 `context` 是否在 Provider「下面」？（比如在 `main` 里用了单独的 `runApp(MyApp())`，而不是用 Provider 包起来）

- **无意义的重复重建，性能变差**：
  - 不需要订阅变化的地方使用 `read`；
  - UI 只关心一个字段时使用 `select`；
  - 复杂布局里可以拆小部件，用多个 `Consumer` 局部刷新。

---

## 小结

- `Provider` 通过 `ChangeNotifierProvider` 提供状态，再通过 `Consumer` / `watch` / `read` 等方式在子 Widget 中使用。
- `Consumer` 的 `builder(context, value, child)` 中：
  - `context` 是当前 `BuildContext`；
  - `value` 就是对应类型的状态对象（例如 `CounterModel`）；
  - `child` 可以用来承载不需要重建的静态部分。
- 建议先从「一个 `ChangeNotifier` + 一个 `ChangeNotifierProvider` + 一个 `Consumer`」这种最小组合练熟，再逐步引入 `MultiProvider` 和 `select` 等高级用法。

