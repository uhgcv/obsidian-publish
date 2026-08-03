# Godot GDScript 学习笔记

> 面向独立游戏开发者学生 | Godot 4.x版本

---

## 目录

1. [GDScript概述与开发环境搭建](#1-gdscript概述与开发环境搭建)
2. [节点与场景：Godot核心哲学](#2-节点与场景godot核心哲学)
3. [变量与数据类型](#3-变量与数据类型)
4. [函数与控制流](#4-函数与控制流)
5. [信号系统：节点间通信的核心](#5-信号系统节点间通信的核心)
6. [引擎内置函数与生命周期](#6-引擎内置函数与生命周期)
7. [常用API与内置类型](#7-常用api与内置类型)
8. [进阶特性与设计模式](#8-进阶特性与设计模式)
9. [性能优化与实践技巧](#9-性能优化与实践技巧)
10. [独立游戏实战案例与学习资源](#10-独立游戏实战案例与学习资源)

---

## 1. GDScript概述与开发环境搭建

### 1.1 什么是GDScript？

GDScript是专为Godot游戏引擎量身定制的高级脚本语言，采用面向对象、指令式的编程范式，语法基于缩进结构，与Python类似。

*GDScript语法与Python高度相似，使用相同的`if`、`while`、`for`语法，但两者原理截然不同——GDScript是从Squirrel、Lua、Python等语言中汲取灵感的独立设计，而非Python的实现。与C#或C++相比，GDScript去除了花括号，采用缩进定义代码块，代码更为简洁。*

GDScript的核心哲学是 **“为游戏而生”** ——它的数据类型、函数和内置功能都直接对应Godot引擎的节点、场景和信号系统，让开发者以最直观的方式编写游戏逻辑。

### 1.2 Godot与GDScript的独特优势

| 优势 | 说明 |
|------|------|
| **深度引擎集成** | 可直接通过`$`符号访问场景树节点，比传统的字符串查找更安全高效 |
| **零成本上手** | 编辑器解压即用，无需安装配置，小型项目可在几小时内搭建原型 |
| **轻量化架构** | 安装体积不足500MB，基于C++底层兼顾效率与资源占用 |

*相比Unity动辄数十GB的安装包和复杂的项目配置，Godot的便携性让开发者可以在多台电脑间无缝切换开发环境。相比Unreal的花括号语法和庞大编译过程，GDScript的缩进式语法让脚本文件更短、编译和加载速度极快。*

### 1.3 安装与环境配置

1. **下载Godot 4.x稳定版**：从官网直接下载，解压即可运行
2. **创建新项目**：设置项目路径并选择渲染器（Forward+ / Mobile / Compatibility）
3. **创建第一个脚本**：选中节点（如Node2D），点击“附加脚本”按钮

> 便捷操作：`$NodePath` 是 `get_node("NodePath")` 的简写，`%UniqueNode` 是 `get_node("%UniqueNode")` 的简写。

---

## 2. 节点与场景：Godot核心哲学

### 2.1 节点（Node）

节点是Godot最基本的构建块，每个节点代表游戏中的特定功能或实体：

- **Sprite2D**：显示2D图像
- **CollisionShape2D/3D**：定义碰撞检测形状
- **CharacterBody2D**：实现可移动的物理角色
- **Area2D**：检测与其他物理体的重叠（触发器和攻击范围）
- **Timer**：计时器逻辑
- **AudioStreamPlayer**：音频播放

节点以树状结构组织，子节点的位置、旋转和缩放通常相对父节点而言。

*这与Unity的Prefab或Unreal的Actor继承体系不同——Godot强调通过组合小型功能节点来构建复杂物体，本质是 **“合成优于继承”** 的设计哲学，而非通过深层的类继承链来扩展功能。*

### 2.2 场景（Scene）

场景是节点的集合，可保存为`.tscn`文件并在运行时实例化。开发中将角色、关卡、UI等设计为独立场景，通过实例化和组合来构建复杂游戏：

```gdscript
# 动态实例化场景示例
var bullet_node = bullet_scene.instantiate()
bullet_node.position = position + Vector2(6, 6)
get_tree().current_scene.add_child(bullet_node)
```

*这种模块化设计与Unity的Prefab类似，但Godot的场景结构更为统一——在Godot中，每个单独的文件就是一个场景，角色是场景、关卡是场景、UI也是场景，使用相同的一套规则完成实例化和组合，学习成本更低。*

### 2.3 场景树（Scene Tree）

场景管理器将所有活动场景组织成树，提供`get_tree()`方法获取全局引用。常用操作包括：`get_tree().current_scene`获取当前场景，`get_tree().reload_current_scene()`重载场景。

---

## 3. 变量与数据类型

### 3.1 变量声明与赋值

使用`var`关键字声明变量：

```gdscript
var player_name = "Hero"        # 字符串类型（自动推断）
var player_score = 0            # 整数类型
var is_game_over = false        # 布尔类型
var enemy_count                 # 先声明后赋值（初始为null）
enemy_count = 10
```

*与C++/C#不同，GDScript变量默认采用动态类型，无需显式指定类型；但与Python不同的是，GDScript提供了可选的静态类型提示（Type Hints），可以在需要时添加类型来获得更好的编辑器支持和错误检查。*

### 3.2 类型系统

**内置基本类型**：

| 类型 | 示例 | 说明 |
|------|------|------|
| `int` | `10, -5, 0` | 64位有符号整型 |
| `float` | `3.14, -0.5, 100.0` | 64位浮点数 |
| `bool` | `true, false` | 布尔值（注意：小写，非`True/False`）|
| `String` | `"Hello", 'GDScript'` | 字符串 |
| `Array` | `[1, 2, 3], ["apple","banana"]` | 动态数组（不同于Python列表，无Tuple）|
| `Dictionary` | `{"name": "John", "age": 30}` | 字典（键值对集合）|
| `Vector2/Vector3` | `Vector2(100, 200)` | 内置向量类型 |

*与Python的关键区别：GDScript没有True/False而是true/false（小写），整数除法用`/`而非`//`，没有链式比较（`a < b < c`不可用），没有内置的元组类型，没有lambda表达式而是`func(x): return x+2`的多行写法。与C#相比：C#是强类型静态编译语言，类型定义更为严格；而GDScript允许混合使用动态和静态类型，开发自由度更高。*

### 3.3 类型提示（可选静态类型）

```gdscript
# 为变量添加类型提示
var health: int = 100
var speed: float = 5.5
var message: String = "Welcome!"

# 使用 := 进行类型推断
var position := Vector2(100, 200)  # 自动推断为Vector2

# 为函数参数和返回值添加类型
func add_numbers(a: int, b: int) -> int:
    return a + b
```

*类型提示相当于TypeScript在JavaScript中的角色——丰富了动态语言的工程能力。使用静态类型可以提升编辑器自动补全效果、减少运行时类型错误，并在编译期检测代码问题。这与Python的可选类型注解（Type Hints）思路一致，但GDScript的静态类型集成更深入，对于变量在栈上的分配更高效。*

### 3.4 常量与变量作用域

```gdscript
# 常量（UPPER_SNAKE_CASE命名）
const MAX_HEALTH = 100
const GRAVITY = 9.8
const GAME_TITLE = "My Game"

# 命名规范
# var playerScore（成员变量，camelCase）
# class PlayerController（类名，PascalCase）
# const MAX_SPEED（常量，UPPER_SNAKE_CASE）
```

*与Python不同，Python无const关键字，常量只是约定（全体大写）。GDScript的const在编译时求值，运行期不可修改，提供了真正的常量保护机制。变量的作用域规则与众多C风格语言类似，而非Python的LEGB规则。*

---

## 4. 函数与控制流

### 4.1 函数声明

使用`func`关键字声明函数：

```gdscript
# 简单函数
func say_hello():
    print("Hello, GDScript!")

# 带参数和返回值的函数
func add_numbers(a: int, b: int) -> int:
    return a + b

# 调用
say_hello()
var sum = add_numbers(5, 3)
```

*在Python中用`def`声明函数，C#/C++中用`[返回类型]函数名(参数)`。GDScript的`func`更接近Python，但用`:`定义代码块，用`return`返回值。与Python不同的是，GDScript没有lambda匿名函数，多行函数是唯一写法。*

### 4.2 条件语句

```gdscript
var score = 85
if score >= 90:
    print("Excellent!")
elif score >= 70:
    print("Good job!")
else:
    print("Keep trying!")
```

*语法与Python完全一致，使用`if-elif-else`结构。但在GDScript中，不能使用`a < b < c`的链式比较。*

### 4.3 循环语句

**for循环**（配合数组）：

```gdscript
var inventory = ["sword", "shield", "potion"]
for item in inventory:
    print(item)

# 使用range
for i in range(5):
    print(i)  # 0 1 2 3 4
```

**while循环**：

```gdscript
var count = 0
while count < 5:
    print("Count: ", count)
    count += 1

# break和continue控制
if count == 3:
    break          # 立即终止循环
elif count == 1:
    continue       # 跳过本次迭代剩余部分
```

*语法与Python完全相同。但与Python不同，GDScript不支持对序列使用`*`重复操作符（如`[1,2]*3`）。数组索引操作和迭代器风格与C#接近，但GDScript更简洁。*

### 4.4 match语句（模式匹配）

```gdscript
match value:
    0:
        print("zero")
    1, 2:
        print("one or two")
    _:
        print("other")
```

*这是GDScript独有的特性，类似Rust的`match`但更简单。Python中需要`if-elif`实现，而GDScript提供了更优雅的模式匹配语法，尤其适合处理枚举和状态机逻辑。*

---

## 5. 信号系统：节点间通信的核心

### 5.1 什么是信号？

信号是Godot内置的委托机制，允许一个节点对另一个节点的变化做出反应，而无需直接相互引用。信号遵循 **“向上通知，向下调用”** 的解耦原则。

*信号的本质是观察者模式的实现。与C#的事件（event）或Unity的SendMessage相比，GDScript信号的设计更为轻量：无需定义复杂的委托类型，直接用`signal`关键字声明即可。相比C++的普通回调函数指针或虚函数重写，信号带来了松耦合的模块通信方式。*

### 5.2 声明与使用信号

```gdscript
# 声明信号
signal health_depleted
signal score_changed(new_score: int)

# 发出信号
func take_damage(amount: int):
    health -= amount
    if health <= 0:
        health_depleted.emit()

# 连接信号
func _ready():
    # 方式1：代码连接
    player.health_depleted.connect(_on_player_died)
    
    # 方式2：编辑器连接（推荐）

# 处理信号
func _on_player_died():
    game_over()
```

*在Godot 3.x中，信号连接使用`connect("signal_name", target, "method_name")`的字符串方式；Godot 4.x改进为`signal.connect(callable)`的Callable直接绑定，更加优雅和类型安全。相比Python或C#的事件，GDScript的信号集成度更高，可以直接在编辑器中可视化连接。*

### 5.3 最佳实践

- **向上通知，向下调用**：子节点向上发出信号，父节点接收信号并调用适当的函数
- **场景间通信**：使用Autoload单例管理全局信号
- **避免深层次直接引用**：优先使用信号而非`get_parent()`链式调用

---

## 6. 引擎内置函数与生命周期

### 6.1 生命周期钩子

Godot在不同阶段自动调用以下回调函数（类似Unity的`Start`/`Update`/`FixedUpdate`）：

| 回调函数 | 调用时机 | 对应概念 |
|----------|----------|----------|
| `_init()` | 节点创建时 | 类似构造函数 |
| `_enter_tree()` | 节点进入场景树时 | 最早可用的初始化点 |
| `_ready()` | 节点及其所有子节点都进入场景树后（最常用）| 类似Unity `Start()` |
| `_process(delta)` | 每帧调用，帧速率不稳定 | 类似Unity `Update()` |
| `_physics_process(delta)` | 每物理帧调用（默认60Hz），用于物理逻辑 | 类似Unity `FixedUpdate()` |
| `_input(event)` | 每发生输入事件时调用 | 类似Unity输入处理 |
| `_exit_tree()` | 节点离开场景树时 | 清理逻辑 |

```gdscript
func _ready():
    # 初始化逻辑（最常用！）
    # 这里的node已经完整可用
    print("节点就绪")

func _process(delta: float):
    # delta是上一帧到当前帧的时间（秒），用于帧速率无关的移动
    position += velocity * delta

func _physics_process(delta: float):
    # 使用move_and_slide()等物理函数
    move_and_slide()
```

*`_physics_process`对应Unity的`FixedUpdate`——专门处理物理模拟，运行间隔固定；而`_process`对应Unity的`Update`，运行间隔随帧率变化。这与Unreal的`Tick()`函数类似，但Godot明确区分了普通帧更新和物理帧更新，让开发者能够更精确地控制游戏逻辑。*

### 6.2 节点访问简写

```gdscript
# $符号是 get_node() 的简写
$Player/Camera2D          # 等价于 get_node("Player/Camera2D")
%UniqueNode               # 获取唯一名称节点
```

---

## 7. 常用API与内置类型

### 7.1 @GDScript内置全局函数

GDScript提供大量内置实用函数和常量，可在任何脚本中直接访问：

```gdscript
# 数学函数
var r = randf()                     # 随机浮点数 (0.0-1.0)
var result = lerp(0, 100, 0.5)      # 线性插值 = 50
var pi_val = PI                     # 圆周率常量
var rad = deg_to_rad(180)           # 角度转弧度

# 类型转换
var num = int(3.14)                 # 转整数：3
var text = str(100)                 # 转字符串："100"

# 资源加载（编译时）
var scene = preload("res://main.tscn")

# 调试输出
print("Hello, World!")
assert(health > 0, "Health不可为负数")

# 时间相关
await get_tree().create_timer(1.5).timeout  # 等待1.5秒
```

*内置数学函数丰富程度接近Python的math模块，但分散在@GDScript和@GlobalScope中。与C#需要`Mathf`类调用不同，GDScript中大部分常用函数都是全局可访问的，降低了学习成本。*

### 7.2 内置类型详解

#### Array（数组）
```gdscript
var arr = [1, 2, 3]
arr.append(4)                # 添加元素
arr.remove_at(0)             # 移除索引0的元素
var size = arr.size()        # 获取大小（注意：.size()非 len()）
var contains = 2 in arr      # 检查包含
```

*在Python中获取数组长度使用`len(arr)`，但GDScript使用`arr.size()`的面向对象风格。*

#### Dictionary（字典）
```gdscript
var dict = {"name": "Hero", "level": 5}
dict["health"] = 100         # 添加键值对
if "name" in dict:           # 检查键是否存在
    print(dict["name"])
```

#### Vector2 / Vector3（向量）
```gdscript
var pos2 = Vector2(100, 200)
var pos3 = Vector3(10, 20, 30)
var direction = (target - pos).normalized()
var distance = pos.distance_to(target)
var dot_product = vec1.dot(vec2)
```

*无论是2D还是3D开发，向量数学运算都可以直接使用内置方法，性能接近原生C++水平。这在2D/3D游戏开发极其便利。*

### 7.3 值传递 vs 引用传递

**重要**：GDScript中内置基本类型（int、float、bool等）按值传递，每次赋值都会复制值。而`Object`、`Array`、`Dictionary`以及紧缩数组（`PackedByteArray`等）按引用传递，实例共享。

```gdscript
var a = [1, 2, 3]
var b = a        # 引用传递（共享）
b.append(4)      # a也变成了[1,2,3,4]

var x = 10
var y = x        # 值传递（独立副本）
y = 20           # x仍为10
```

*这与Python行为完全一致（可变与不可变类型），但与C/C++的指针/引用概念有所不同。C/C++中，开发者显式控制是否按引用传递（`&`运算符），GDScript则将选择权交由数据类型本身。*

---

## 8. 进阶特性与设计模式

### 8.1 类型系统进阶：静态类型的最佳实践

GDScript 2.0提供了成熟的静态类型系统：

```gdscript
# 类型推断（使用 := 运算符）
var health := 100           # 推断为 int
var position := Vector2()   # 推断为 Vector2

# 数组类型声明
var enemies: Array[CharacterBody2D] = []
var scores: Array[int] = [85, 92, 78]

# 强制返回类型
func get_health() -> int:
    return health
```

*这与TypeScript的可选静态类型高度相似。相比Python的Type Hints在运行时被忽略，GDScript可以在编译期进行更严格的检查，给开发者提供更好的错误预防能力。*

### 8.2 装饰器：将变量暴露到编辑器

```gdscript
# @export：在编辑器的Inspector面板中显示变量
@export var move_speed: float = 50.0
@export var player_name: String = "Hero"
@export var bullet_scene: PackedScene   # 拖拽场景文件到此变量

# @onready：等待节点就绪后再获取引用
@onready var sprite = $Sprite2D
@onready var animator = $AnimationPlayer
```

*约定优于配置：这正是Godot开发的核心流程。`@export`类似Unity的`[SerializeField]`属性或Unreal的`UPROPERTY(EditAnywhere)`。`@onready`则是Godot的简洁解决方案，因为直接在`_ready()`函数外使用`$NodePath`可能因节点未就绪而报错，用`@onready`可以优雅地延迟初始化。*

### 8.3 协同程序（await）

GDScript支持协程（coroutine），使用`await`关键字异步等待信号或函数完成：

```gdscript
# 等待计时器完成
await get_tree().create_timer(2.0).timeout
print("2秒后执行这里")

# 等待信号
await player.health_depleted
game_over()

# 等待另一个异步函数完成
func animate_door():
    $AnimationPlayer.play("door_open")
    await $AnimationPlayer.animation_finished
    print("门已打开")
```

*C#使用`async/await`和`Task`实现异步编程，代码复杂度较高；Python使用`async/await`但需要事件循环。而GDScript的`await`无须定义异步函数类型，可以直接等待任何信号或协程完成，学习成本最低。*

### 8.4 核心设计模式

#### 1. Autoload单例
```gdscript
# 在项目设置中添加autoload脚本（如GameManager）
# 全局可访问，无需实例化
GameManager.add_score(100)
```

#### 2. 有限状态机（FSM）
```gdscript
enum State {IDLE, RUN, JUMP, ATTACK}
var current_state = State.IDLE

func _process(delta):
    match current_state:
        State.IDLE:
            handle_idle()
        State.RUN:
            handle_run()
```

#### 3. 对象池模式
对于频繁生成/销毁的对象（子弹、粒子等），使用预分配的对象池，避免性能开销。

#### 4. 合成优于继承 + 信号解耦通信
无需创建深层继承链条，用信号实现子节点与父节点的松耦合通信。

*单例在Godot中被称为“Autoload”（自动加载），对应C#的Singleton模式。状态机是游戏开发中最常用的模式之一，`match`语句让其在GDScript中无比简洁。相比Unity的SendMessage或GetComponent链式调用，Godot的信号将所有跨对象通信统一以解耦方式处理。*

---

## 9. 性能优化与实践技巧

### 9.1 Godot的性能优势

Godot底层基于C++编写，确保运行效率。在2D渲染方面，自动绘制批处理（draw batching）减少了CPU绘制调用（draw calls）消耗，有效提升CPU效率。

- **视口裁剪**：智能识别屏幕范围外的场景元素，自动暂停其渲染和物理计算
- **对象复用**：使用对象池模式避免频繁实例化

### 9.2 编写高效GDScript的黄金法则

**1. 善用静态类型**
```gdscript
# ❌ 动态类型（运行时效率稍低）
func calculate_damage(amount):
    return amount * 2

# ✅ 静态类型（编译期优化）
func calculate_damage(amount: int) -> int:
    return amount * 2
```

*当类型在编译期已知时，GDScript会生成优化的操作码并执行更快，运行时性能更稳定。*

**2. 信号优于连续的`_process`检查**
```gdscript
# ❌ 不好的做法：每帧检查
func _process(delta):
    if has_powerup:
        apply_effect()

# ✅ 好的做法：信号驱动
signal powerup_collected

func _on_powerup_collected():
    apply_effect()
```

**3. 使用`@onready`缓存节点引用**
```gdscript
# 只写一次路径，避免每帧$查找
@onready var animator = $AnimationPlayer

func _process(delta):
    animator.play("walk")  # 高效引用
```

**4. 避免在`_process()`中频繁创建对象**
```gdscript
# ❌ 不好的做法
func _process(delta):
    var temp_array = []  # 每帧重新分配
    # ...

# ✅ 好的做法：复用
var temp_array = []
func _process(delta):
    temp_array.clear()
    # ...
```

**5. 释放未使用的节点和资源**
使用`queue_free()`延迟销毁节点，利用引擎的引用计数自动管理内存。

*GDScript没有垃圾回收（GC）机制——采用引用计数自动管理内存，避免了GC引起的帧率卡顿（stutter）。对比而言，C#的GC可能导致临时卡顿，这使GDScript在实时交互游戏中具有天然的稳定优势。*

---

## 10. 独立游戏实战案例与学习资源

### 10.1 实战项目案例

**角色控制（CharacterBody2D）**：

```gdscript
extends CharacterBody2D

@export var move_speed: float = 50.0
@export var animator: AnimatedSprite2D

func _physics_process(delta):
    # 获取输入方向
    var direction = Input.get_vector("left", "right", "up", "down")
    velocity = direction * move_speed
    
    # 根据速度播放动画
    if velocity == Vector2.ZERO:
        animator.play("idle")
    else:
        animator.play("run")
    
    # 移动并处理碰撞
    move_and_slide()
```

**攻击系统实例化**：

```gdscript
func _on_fire():
    if is_game_over:
        return
    var bullet_node = bullet_scene.instantiate()
    bullet_node.position = position + Vector2(6, 6)
    get_tree().current_scene.add_child(bullet_node)
```

### 10.2 独立游戏开发者经验

- **跨平台输入抽象**：统一处理PC键盘、手柄和移动端触摸输入
- **导出管线优化**：Godot的导出器支持一键打包到多平台
- **面向低端设备的性能优化**：使用视口裁剪和资源压缩

### 10.3 推荐学习资源

| 类型 | 资源 |
|------|------|
| **官方文档** | Godot 4.x 官方中文文档（docs.godotengine.org/zh-cn） |
| **Books** | 《Godot 4 for Beginners》(Packt, 2025) |
| **Books** | 《Game Development with Godot 4》(2025) |
| **Community** | Godot官方论坛、Reddit r/godot、中文Godot社区 |
| **Projects** | 先用现成的Godot 4示例项目理解场景组织架构和GDScript写法 |
| **Subscription** | Godot官方文档（F1快捷键编辑器随时查阅） |

---

## 附录：核心速查对照表

| 概念 | GDScript | Python | C#/C++ |
|------|----------|--------|---------|
| 函数定义 | `func name():` | `def name():` | `void name(){}` |
| 变量声明 | `var x = 1` | `x = 1` | `int x = 1;` |
| 常量 | `const X = 1` | *约定式 `X=1`* | `const int X = 1;` |
| 布尔值 | `true/false` | `True/False` | `true/false` |
| 字符串 | `"text"` or `'text'` | `"text"` or `'text'` | `"text"` |
| 数组长度 | `array.size()` | `len(array)` | `array.Length/Size()` |
| 继承 | `extends Class` | `class Child(Parent):` | `: ParentClass` |
| 类型系统 | 渐进类型（动态为主） | 动态（仅Type Hints） | 强类型静态 |
| 命名风格 | camelCase（变量/函数） | snake_case | PascalCase（C#） / camelCase（C++） |

---

> **最后的话**：学习GDScript不是为了学会“另一门语言的语法”，而是理解Godot的设计哲学——场景树 + 节点合成 + 信号解耦。对于独立游戏开发学生来说，从一个小项目（如2D平台跳跃或射击游戏）开始动手实践，远比背语法书有效。