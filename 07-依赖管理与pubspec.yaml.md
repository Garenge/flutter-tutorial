## 07 - 依赖管理与 pubspec.yaml

Flutter 项目中的所有依赖（包括 Flutter SDK 版本、第三方包、资源文件等），都是通过 `pubspec.yaml` 来管理的。本节我们来系统讲一下：

- **如何添加第三方依赖**
- **`pubspec.yaml` 每个常见字段的作用**
- **添加依赖后如何下载、升级、移除**

---

## 一、什么是 pubspec.yaml？

当你用 `flutter create` 创建项目时，根目录会自动生成一个 `pubspec.yaml` 文件，大致结构类似：

```yaml
name: my_app
description: A new Flutter project.
publish_to: "none"
version: 1.0.0+1

environment:
  sdk: ">=3.3.0 <4.0.0"

dependencies:
  flutter:
    sdk: flutter
  cupertino_icons: ^1.0.8

dev_dependencies:
  flutter_test:
    sdk: flutter
  flutter_lints: ^5.0.0

flutter:
  uses-material-design: true
```

你可以把 `pubspec.yaml` 理解成：**项目的配置清单 + 依赖清单**。

---

## 二、`pubspec.yaml` 常见字段解释

### 1. 顶部基础信息

- **`name`**：包/项目名称，在发布到 pub.dev 时会用到，本地项目也会用到（例如生成构建产物目录等）。
- **`description`**：项目描述，给人类看的，写清楚项目用途即可。
- **`publish_to`**：  
  - `"none"`：表示**不打算发布到 pub.dev**（本地项目常见设置）。  
  - 如果要发布到 pub.dev，可以删除或设置为对应地址。
- **`version`**：应用版本号，格式通常是 `主版本.次版本.修订版本+构建号`，例如 `1.0.0+1`。

### 2. `environment`

```yaml
environment:
  sdk: ">=3.3.0 <4.0.0"
```

- **作用**：限制 Dart SDK 的版本范围，保证你的代码和依赖包与当前 SDK 兼容。
- 一般不需要手动频繁修改，升级 Flutter SDK 时可以自动更新。

### 3. `dependencies`（运行时依赖）

```yaml
dependencies:
  flutter:
    sdk: flutter
  cupertino_icons: ^1.0.8
  http: ^1.2.0
```

- **`dependencies`**：项目运行时需要的包，会被打包到最终的应用中。
- 常见示例：
  - `http`：网络请求
  - `provider`：状态管理
  - `shared_preferences`：本地存储

#### 版本号写法说明

- **`^1.2.3`**：Caret 语义版本，表示「兼容 1.2.3 的最新小版本」，不会自动升级到 2.x。
- **`1.2.3`**：固定版本，只有 `1.2.3` 这一版。
- **`any`**：任何版本都行（不推荐，可能导致不可控的冲突）。

一般情况下，官方文档/ pub.dev 上会告诉你推荐使用的版本号（如 `^6.0.0`）。

### 4. `dev_dependencies`（开发/测试依赖）

```yaml
dev_dependencies:
  flutter_test:
    sdk: flutter
  flutter_lints: ^5.0.0
```

- **`dev_dependencies`**：只在开发、测试阶段使用的依赖，不会打包到最终应用中。
- 示例：
  - `flutter_test`：单元测试、Widget 测试
  - `flutter_lints`：代码规范检查

### 5. `flutter` 配置

```yaml
flutter:
  uses-material-design: true
  assets:
    - assets/images/
    - assets/config/app_config.json
  fonts:
    - family: Roboto
      fonts:
        - asset: assets/fonts/Roboto-Regular.ttf
```

- **`uses-material-design`**：为 `Icons` 等 Material 图标启用默认图标集。
- **`assets`**：声明项目中要打包的资源文件（图片、json、文本等）。  
  - 可以写文件夹：`assets/images/`（会打包此目录下所有文件）  
  - 也可以写具体文件：`assets/config/app_config.json`
- **`fonts`**：自定义字体配置。

注意：  
**YAML 非常依赖缩进（一般 2 个空格），不能用 Tab。缩进错误会导致整个文件解析失败。**

---

## 三、如何添加一个新的依赖？

下面以添加常用的 `http` 网络请求库为例。

### 步骤 1：在 pubspec.yaml 中添加依赖

在 `dependencies` 部分新增一行（注意缩进）：

```yaml
dependencies:
  flutter:
    sdk: flutter
  cupertino_icons: ^1.0.8
  http: ^1.2.0  # 新增的依赖
```

**小技巧：**

- 建议使用 VS Code/Cursor 的 **YAML 语法高亮 + 自动对齐**，避免缩进错误。
- 不要把 `http` 写到 `dev_dependencies`，否则运行时可能找不到。

### 步骤 2：安装依赖（拉取包）

在项目根目录执行（终端）：

```bash
flutter pub get
```

或者在 VS Code/Cursor 的命令面板里执行 **Flutter: Get Packages**（如果装了 Flutter 扩展）。

成功后，终端会看到类似：

```text
Running "flutter pub get" in my_app...
exit code 0
```

此时，`pubspec.lock` 文件会更新，记录每个依赖的**具体版本**。

### 步骤 3：在代码中使用依赖

```dart
import 'package:http/http.dart' as http;

Future<void> fetchData() async {
  final response = await http.get(Uri.parse('https://example.com'));
  print(response.body);
}
```

如果报错「找不到包」，检查：

1. `pubspec.yaml` 中是否在 `dependencies` 下正确写了依赖；
2. 是否已经执行过 `flutter pub get`；
3. 是否有缩进/冒号错误（IDE 中的报错信息一般会指出具体行）。

---

## 四、依赖的升级、降级与移除

### 1. 升级依赖

常见两种方式：

- **全量升级所有包**：

```bash
flutter pub upgrade
```

- **只升级某一个包**（例如升级 http）：

```bash
flutter pub upgrade http
```

如果你使用 caret 版本号（如 `^1.2.0`），`upgrade` 会在兼容范围内尽量升到最新版本。

### 2. 指定版本或降级

假设你想锁定 `http` 为 `1.1.0` 版本：

```yaml
dependencies:
  http: 1.1.0
```

然后重新执行：

```bash
flutter pub get
```

### 3. 移除依赖

1. 在 `pubspec.yaml` 中删除对应的依赖条目；
2. 运行：

```bash
flutter pub get
```

3. 删除代码中对应的 `import` 和使用代码。

---

## 五、添加本地 / Git / 路径依赖（了解即可）

除了从 pub.dev 安装常规包外，`pubspec.yaml` 也支持引用：

### 1. Git 仓库依赖

```yaml
dependencies:
  my_package:
    git:
      url: https://github.com/username/my_package.git
      ref: main  # 可选，指定分支/Tag/Commit
```

### 2. 本地路径依赖

```yaml
dependencies:
  my_local_package:
    path: ../my_local_package
```

适合多包工程、本地开发共享代码时使用。

---

## 六、常见错误与排查

- **YAML 缩进错误**  
  - 表现：运行 `flutter pub get` 报错，提示某行解析失败。  
  - 解决：检查是否用了 Tab、是否多了/少了空格，参考示例重新对齐。

- **版本冲突（version solving failed）**  
  - 表现：终端出现 `Because xxx depends on yyy >=2.0.0 which requires SDK version...` 等。  
  - 解决：
    - 根据提示升级 Flutter/Dart SDK；
    - 或调整依赖版本号，让多个包的版本范围能同时满足。

- **添加依赖后仍然报「找不到包」**  
  - 先确认：  
    - 依赖写在 `dependencies` 下；  
    - 已经执行 `flutter pub get`；  
    - 导包路径是否正确（`import 'package:xxx/xxx.dart';`）。

---

## 七、实践小练习

建议你自己动手做一个小练习，加深理解：

1. 在 `pubspec.yaml` 中添加以下依赖：

   ```yaml
   dependencies:
     http: ^1.2.0
     shared_preferences: ^2.3.0
   ```

2. 运行 `flutter pub get`，确认没有报错。
3. 新建一个页面：
   - 用 `http` 拉取一个公开 API（例如 `https://jsonplaceholder.typicode.com/todos/1`）；  
   - 把返回的数据用 `shared_preferences` 缓存到本地；  
   - 再次打开应用时，从本地读取上次的缓存数据展示出来。

通过这个小 Demo，你会同时熟悉：

- 如何在 `pubspec.yaml` 中添加依赖；
- 如何在代码中导入并使用第三方包；
- 如何在终端中执行 `flutter pub get / flutter pub upgrade`。

---

## 小结

- **`pubspec.yaml` 是 Flutter 项目的「总配置 + 依赖清单」**；
- 新增依赖的流程：**写入 `pubspec.yaml` → 执行 `flutter pub get` → 在代码中 `import` 使用**；
- 注意 YAML 的缩进和版本号写法，出现错误先看终端/IDE 的报错提示。

## 下一步

学习页面导航，查看 [导航路由](./08-导航路由.md)！
