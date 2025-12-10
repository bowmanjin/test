---
date: 2014-03-10
linktitle: code 
slug: code-test
menu:
  main:
    parent: tutorials
prev: /tutorials/mathjax
title: 代码测试
categories: [
    "Docs", "文学","艺术"
]
weight: 10

---





以下是 **10种常见编程语言** 的实用代码片段，均为经典入门场景（含语法特性、核心功能），可直接用于 Hugo 博客（配合代码高亮标识即可生效）：

### Python（数据处理+列表推导）

```python
# 功能：计算1-10的偶数平方和（体现列表推导、内置函数）
def calculate_even_square_sum():
    # 列表推导式筛选偶数并计算平方
    even_squares = [x*x for x in range(1, 11) if x % 2 == 0]
    return sum(even_squares)

# 调用函数并打印结果
result = calculate_even_square_sum()
print(f"1-10偶数的平方和：{result}")  # 输出：220
```

### JavaScript（DOM操作+箭头函数）

```javascript
// 功能：点击按钮修改页面文本（体现DOM操作、箭头函数、事件监听）
document.addEventListener('DOMContentLoaded', () => {
  // 获取按钮和文本元素
  const btn = document.getElementById('changeTextBtn');
  const textElem = document.getElementById('targetText');
  
  // 箭头函数绑定点击事件
  btn.addEventListener('click', () => {
    const newText = `更新时间：${new Date().toLocaleString()}`;
    textElem.textContent = newText; // 修改文本内容
    textElem.style.color = '#2c3e50'; // 修改样式
  });
});

// HTML结构（需配合使用）
/*
<button id="changeTextBtn">点击更新</button>
<p id="targetText">等待更新...</p>
*/
```

### Go（结构体+接口）

```go
// 功能：定义动物接口及实现（体现Go的接口隐式实现特性）
package main

import "fmt"

// 定义接口
type Animal interface {
	Sound() string // 接口方法
}

// 定义结构体
type Dog struct {
	Name string
}

type Cat struct {
	Name string
}

// Dog实现Animal接口
func (d Dog) Sound() string {
	return fmt.Sprintf("%s：汪汪汪", d.Name)
}

// Cat实现Animal接口
func (c Cat) Sound() string {
	return fmt.Sprintf("%s：喵喵喵", c.Name)
}

func main() {
	animals := []Animal{Dog{"旺财"}, Cat{"咪酱"}}
	for _, animal := range animals {
		fmt.Println(animal.Sound()) // 多态调用
	}
}
```

### Java（类与继承）

```java
// 功能：抽象类与子类实现（体现面向对象核心特性）
abstract class Shape {
    // 抽象方法（子类必须实现）
    public abstract double calculateArea();
}

// 圆形子类
class Circle extends Shape {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double calculateArea() {
        return Math.PI * radius * radius; // 圆面积公式
    }
}

// 矩形子类
class Rectangle extends Shape {
    private double width;
    private double height;

    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    @Override
    public double calculateArea() {
        return width * height; // 矩形面积公式
    }
}

public class Main {
    public static void main(String[] args) {
        Shape circle = new Circle(5);
        Shape rectangle = new Rectangle(4, 6);
        
        System.out.printf("圆面积：%.2f\n", circle.calculateArea()); // 78.54
        System.out.printf("矩形面积：%.2f\n", rectangle.calculateArea()); // 24.00
    }
}
```

### C++（模板函数+STL）

```cpp
// 功能：模板函数+向量容器（体现泛型编程与STL使用）
#include <iostream>
#include <vector>
#include <algorithm> // 用于sort函数

// 模板函数：打印任意类型的向量
template <typename T>
void printVector(const std::vector<T>& vec) {
    for (const auto& elem : vec) {
        std::cout << elem << " ";
    }
    std::cout << std::endl;
}

int main() {
    // 整数向量
    std::vector<int> nums = {3, 1, 4, 1, 5, 9};
    std::cout << "排序前：";
    printVector(nums);
    
    // STL排序函数
    std::sort(nums.begin(), nums.end());
    std::cout << "排序后：";
    printVector(nums);
    
    return 0;
}
```

### C#（LINQ查询+面向对象）

```csharp
// 功能：LINQ查询筛选对象列表（体现C# LINQ特性）
using System;
using System.Collections.Generic;
using System.Linq;

// 定义学生类
public class Student {
    public string Name { get; set; }
    public int Age { get; set; }
    public double Score { get; set; }
}

class Program {
    static void Main() {
        // 学生数据列表
        List<Student> students = new List<Student> {
            new Student { Name = "张三", Age = 18, Score = 92.5 },
            new Student { Name = "李四", Age = 19, Score = 88.0 },
            new Student { Name = "王五", Age = 18, Score = 95.0 }
        };

        // LINQ查询：筛选18岁且分数≥90的学生
        var qualifiedStudents = from s in students
                               where s.Age == 18 && s.Score >= 90
                               select new { s.Name, s.Score };

        // 打印结果
        Console.WriteLine("18岁高分学生：");
        foreach (var student in qualifiedStudents) {
            Console.WriteLine($"姓名：{student.Name}，分数：{student.Score}");
        }
    }
}
```

### PHP（数组操作+文件读取）

```php
<?php
// 功能：数组处理+读取文件内容（体现PHP数组特性与文件操作）
header("Content-Type: text/html; charset=utf-8");

// 关联数组：存储用户信息
$users = [
    ["name" => "小明", "gender" => "男", "age" => 22],
    ["name" => "小红", "gender" => "女", "age" => 20]
];

// 遍历数组并打印
echo "<h3>用户列表</h3>";
foreach ($users as $user) {
    echo "姓名：{$user['name']}，性别：{$user['gender']}，年龄：{$user['age']}<br>";
}

// 读取文件内容（需确保文件存在）
$filePath = "test.txt";
if (file_exists($filePath)) {
    $content = file_get_contents($filePath);
    echo "<h3>文件内容</h3>";
    echo nl2br($content); // 保留换行符
} else {
    echo "文件不存在！";
}
?>
```

### Ruby（迭代器+哈希表）

```ruby
# 功能：哈希表操作+迭代器（体现Ruby简洁语法）
# 定义哈希表：存储商品信息
products = {
  "apple" => { name: "苹果", price: 5.99, stock: 100 },
  "banana" => { name: "香蕉", price: 3.99, stock: 150 },
  "orange" => { name: "橙子", price: 4.99, stock: 80 }
}

# 迭代哈希表：打印库存≥100的商品
puts "库存充足的商品："
products.each do |code, info|
  if info[:stock] >= 100
    puts "#{info[:name]} - 价格：#{info[:price]}元，库存：#{info[:stock]}件"
  end
end

# 计算所有商品总价（假设各买一件）
total_price = products.sum { |code, info| info[:price] }
puts "\n各买一件的总价：#{total_price.round(2)}元"
```

### TypeScript（接口+类型定义）

```typescript
// 功能：接口定义+类型约束（体现TS静态类型特性）
// 定义用户接口
interface User {
  id: number;
  username: string;
  email: string;
  isActive?: boolean; // 可选属性
}

// 定义函数：格式化用户信息
function formatUser(user: User): string {
  const status = user.isActive ? "活跃" : "未活跃";
  return `ID：${user.id}，用户名：${user.username}，邮箱：${user.email}，状态：${status}`;
}

// 实例化用户对象
const user1: User = {
  id: 1,
  username: "johndoe",
  email: "john@example.com",
  isActive: true
};

// 调用函数并打印
console.log(formatUser(user1));
// 输出：ID：1，用户名：johndoe，邮箱：john@example.com，状态：活跃
```

### HTML+CSS+JS（前端交互组件）

```html
<!-- 功能：简单计数器组件（体现前端三剑客配合） -->
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>计数器</title>
    <style>
        .counter {
            display: flex;
            align-items: center;
            gap: 10px;
            font-size: 18px;
            padding: 20px;
            max-width: 300px;
            margin: 20px auto;
            border: 1px solid #eee;
            border-radius: 8px;
        }
        .btn {
            padding: 8px 16px;
            border: none;
            border-radius: 4px;
            background: #2c3e50;
            color: white;
            cursor: pointer;
        }
        .btn:hover {
            background: #34495e;
        }
        #count {
            font-weight: bold;
            color: #e74c3c;
        }
    </style>
</head>
<body>
    <div class="counter">
        <button class="btn" id="decrease">-</button>
        <span>当前计数：<span id="count">0</span></span>
        <button class="btn" id="increase">+</button>
    </div>

    <script>
        let count = 0;
        const countElem = document.getElementById('count');
        const increaseBtn = document.getElementById('increase');
        const decreaseBtn = document.getElementById('decrease');

        // 增加计数
        increaseBtn.addEventListener('click', () => {
            count++;
            countElem.textContent = count;
        });

        // 减少计数（不小于0）
        decreaseBtn.addEventListener('click', () => {
            if (count > 0) count--;
            countElem.textContent = count;
        });
    </script>
</body>
</html>


```

```
不指定编程语言类型
# 功能：计算1-10的偶数平方和（体现列表推导、内置函数）
def calculate_even_square_sum():
    # 列表推导式筛选偶数并计算平方
    even_squares = [x*x for x in range(1, 11) if x % 2 == 0]
    return sum(even_squares)

# 调用函数并打印结果
result = calculate_even_square_sum()
print(f"1-10偶数的平方和：{result}")  # 输出：220

```




以下是 **完整的标准 Markdown 语法示例**（含核心语法+常用扩展语法），所有示例均兼容 Hugo（及主流 Markdown 渲染器），可直接用于博客写作、文档编写，配合之前的代码高亮配置可完美渲染：

### 一、基础语法

#### 1. 标题（六级）

语法：`# 一级标题` ~ `###### 六级标题`（`#` 后需加空格）

```markdown
# 一级标题（最大）
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题（最小）
```

渲染效果：

> 一级标题（最大）  
> 二级标题  
> 三级标题  
> 四级标题  
> 五级标题  
> 六级标题（最小）

#### 2. 段落与换行

- 段落：直接输入文字，段落间空一行分隔；
- 换行：行尾加 2 个空格 + 回车（或直接空一行）。

```markdown
这是第一个段落，包含普通文本内容。
行尾加两个空格  （这里有两个空格）
实现强制换行。

这是第二个段落（与上一段空一行分隔），Markdown 会自动处理段落间距。
```

渲染效果：

> 这是第一个段落，包含普通文本内容。  
> 行尾加两个空格  
> 实现强制换行。
>
> 这是第二个段落（与上一段空一行分隔），Markdown 会自动处理段落间距。

#### 3. 文本强调（粗体/斜体/删除线）

```markdown
*斜体文本*（单星号）
_斜体文本_（单下划线）
**粗体文本**（双星号）
__粗体文本__（双下划线）
***粗斜体文本***（三星号）
___粗斜体文本___（三下划线）
~~删除线文本~~（双波浪线）
```

渲染效果：

> *斜体文本*  
> _斜体文本_  
> **粗体文本**  
> __粗体文本__  
> ***粗斜体文本***  
> ___粗斜体文本___  
> ~~删除线文本~~

#### 4. 列表（有序/无序/嵌套）

##### （1）无序列表

语法：`-`/`*`/`+` + 空格（三者效果一致）

```markdown
- 无序列表项 1（减号）
* 无序列表项 2（星号）
+ 无序列表项 3（加号）
```

渲染效果：

> - 无序列表项 1（减号）
>
> * 无序列表项 2（星号）
>
> + 无序列表项 3（加号）

##### （2）有序列表

语法：`数字 + . + 空格`（数字无需连续，渲染时自动排序）

```markdown
1. 有序列表项 1
2. 有序列表项 2
5. 有序列表项 3（数字不连续，渲染仍为 3）
```

渲染效果：

> 1. 有序列表项 1
> 2. 有序列表项 2
> 3. 有序列表项 3（数字不连续，渲染仍为 3）

##### （3）嵌套列表（缩进 4 空格或 1 个制表符）

```markdown
- 外层无序列表
    1. 内层有序列表 1
    2. 内层有序列表 2
        - 内层嵌套无序列表
- 外层无序列表 2
```

渲染效果：

> - 外层无序列表
>   1. 内层有序列表 1
>   2. 内层有序列表 2
>      - 内层嵌套无序列表
> - 外层无序列表 2

#### 5. 链接（内联/引用/锚点）

##### （1）内联链接（最常用）

语法：`[链接文本](链接地址 "可选标题")`（标题hover时显示）

```markdown
[Hugo 官方文档](https://gohugo.io/ "Hugo 官方网站")
[我的博客](https://example.com)（无标题）
```

渲染效果：

> [Hugo 官方文档](https://gohugo.io/ "Hugo 官方网站")  
> [我的博客](https://example.com)（无标题）

##### （2）引用链接（复用链接）

语法：先定义 `[链接标识]: 链接地址 "可选标题"`，再使用 `[链接文本][链接标识]`

```markdown
这是 [Hugo 文档][hugo-docs]，这是 [GitHub][github]。

[hugo-docs]: https://gohugo.io/ "Hugo Docs"
[github]: https://github.com/ "GitHub"
```

渲染效果：

> 这是 [Hugo 文档][hugo-docs]，这是 [GitHub][github]。
>
> [hugo-docs]: https://gohugo.io/ "Hugo Docs"
> [github]: https://github.com/ "GitHub"

##### （3）锚点链接（跳转到页面内标题）

语法：`[跳转文本](#标题文本)`（标题文本需小写，空格替换为 `-`）

```markdown
[跳转到一级标题](#一级标题)
[跳转到列表部分](#4-列表有序无序嵌套)
```

渲染效果：点击后跳转到页面内对应标题位置（Hugo 自动生成锚点）。

#### 6. 图片（内联/引用）

语法：`![alt文本](图片地址 "可选标题")`（alt文本为图片加载失败时显示的内容）

```markdown
# 内联图片（网络图片）
![Hugo -logo](https://gohugo.io/images/hugo-logo-wide.svg "Hugo 官方 Logo")

# 内联图片（本地图片，Hugo 中需放在 static 目录）
![本地图片示例](./images/photo.jpg "本地图片")

# 引用图片（复用地址）
![GitHub Logo][github-logo]
[github-logo]: https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png
```

渲染效果：显示图片，hover 显示标题，加载失败显示 alt 文本。

#### 7. 引用（块引用）

语法：`>` + 空格（支持嵌套，加多个 `>`）

```markdown
> 这是一级引用（单箭头）
> 引用内容可以换行，只需在每行开头加 >
>> 这是二级引用（双箭头，嵌套）
>>> 这是三级引用（三箭头）
>
> 引用中可以包含其他语法：
> - 无序列表
> **粗体文本**
```

渲染效果：

> 这是一级引用（单箭头）
> 引用内容可以换行，只需在每行开头加 >
>
> > 这是二级引用（双箭头，嵌套）
> >
> > > 这是三级引用（三箭头）
>
> 引用中可以包含其他语法：
>
> - 无序列表
>   **粗体文本**

#### 8. 代码（行内/代码块）

##### （1）行内代码

语法：`` `代码内容` ``（反引号包裹）

```markdown
行内代码示例：`print("Hello Markdown")`、`const x = 10`
```

渲染效果：行内代码示例：`print("Hello Markdown")`、`const x = 10`

##### （2）代码块（支持语法高亮）

语法：``` + 语言标识（如 python、javascript），结束用 ```

```markdown
```go
// Go 语言代码块（配合 Hugo 高亮）
package main
import "fmt"
func main() {
    fmt.Println("Hello Code Block!")
}
```

```
渲染效果：带语法高亮的代码块（需配合之前的 Chroma CSS）。

#### 9. 分隔线
语法：`---`/`***`/`___`（单独一行，前后空行，三者效果一致）
```markdown
文本上方的分隔线
---
文本中间的分隔线
***
文本下方的分隔线
___
```

渲染效果：

> 文本上方的分隔线
> ---
>
> 文本中间的分隔线
>
> ***
>
> 文本下方的分隔线
>
> ___

### 二、扩展语法（Hugo 兼容）

#### 10. 表格（支持对齐）

语法：`|` 分隔列，第二行用 `:` 控制对齐（`:---` 左对齐、`---:右对齐`、`:---:居中对齐`）

```markdown
| 姓名   | 年龄 | 分数  |
|--------|------|-------|
| 张三   | 18   | 92.5  |
| 李四   | 19   | 88.0  |
| 王五   | 18   | 95.0  |

# 带对齐的表格
| 左对齐 | 右对齐 | 居中对齐 |
|:-------|-------:|:--------:|
| 文本1  | 数值1  | 内容1    |
| 文本2  | 数值2  | 内容2    |
```

渲染效果：

| 姓名 | 年龄 | 分数 |
| ---- | ---- | ---- |
| 张三 | 18   | 92.5 |
| 李四 | 19   | 88.0 |
| 王五 | 18   | 95.0 |

| 左对齐 | 右对齐 | 居中对齐 |
| :----- | -----: | :------: |
| 文本1  |  数值1 |  内容1   |
| 文本2  |  数值2 |  内容2   |

#### 11. 任务列表（复选框）

语法：`- [ ] 未完成` / `- [x] 已完成`（`[ ]` 中需加空格，`x` 不区分大小写）

```markdown
- [x] 完成 Markdown 语法学习
- [x] 编写代码片段
- [ ] 发布 Hugo 博客
- [ ] 优化网站样式
```

渲染效果：

> - [x] 完成 Markdown 语法学习
> - [x] 编写代码片段
> - [ ] 发布 Hugo 博客
> - [ ] 优化网站样式

#### 12. 脚注（注释说明）

语法：`[^脚注标识]` 定义引用，页面底部用 `[^脚注标识]: 脚注内容` 定义说明

```markdown
这是需要注释的文本[^1]，这是另一个注释[^note]。

[^1]: 这是第一个脚注的详细说明（支持换行和链接：[Hugo](https://gohugo.io)）
[^note]: 这是第二个脚注的说明，标识可以是字母或数字
```

渲染效果：

> 这是需要注释的文本[^1]，这是另一个注释[^note]。
>
> [^1]: 这是第一个脚注的详细说明（支持换行和链接：[Hugo](https://gohugo.io)）
> [^note]: 这是第二个脚注的说明，标识可以是字母或数字

#### 13. 定义列表（术语+解释）

语法：`术语` 单独一行，下一行用 `:` + 空格 写解释（支持嵌套）

```markdown
Markdown
: 一种轻量级标记语言，用于快速编写文档
: 兼容大部分博客平台和静态站点生成器（如 Hugo）

Hugo
: 静态站点生成器
    - 基于 Go 语言开发
    - 渲染速度快
    - 内置代码高亮
```

渲染效果：

> Markdown
> : 一种轻量级标记语言，用于快速编写文档
> : 兼容大部分博客平台和静态站点生成器（如 Hugo）
>
> Hugo
> : 静态站点生成器
>     - 基于 Go 语言开发
>     - 渲染速度快
>     - 内置代码高亮

#### 14. 转义字符（避免语法冲突）

当需要显示 Markdown 语法符号（如 `#`、`*`、`[` 等）时，用 `\` 转义：

```markdown
\# 这不是标题（# 被转义）
\* 这不是斜体（* 被转义）
\[链接文本\](https://example.com)（方括号被转义，显示原始文本）
```

渲染效果：

> # 这不是标题（# 被转义）
>
> * 这不是斜体（* 被转义）
>   [链接文本](https://example.com)（方括号被转义，显示原始文本）

### 三、Hugo 兼容说明

1. 以上所有语法（包括扩展语法如表格、脚注、任务列表）均在 Hugo 中默认支持，无需额外配置；
2. 代码块的语法高亮需配合之前提到的 Chroma CSS 引入，否则仅显示纯文本代码块；
3. 部分主题可能对表格、任务列表的样式有自定义，可通过修改主题 CSS 调整；
4. 若需使用更冷门的扩展语法（如数学公式、Mermaid 图表），Hugo 需安装对应插件或启用扩展渲染器（如 Goldmark）。

将这些语法示例直接复制到 Hugo 的 `content` 目录下的 `.md` 文件中，即可自动渲染为美观的网页内容，配合之前的代码高亮配置，完美适配博客写作需求！




## 常用配置格式


### 1. JSON 配置示例（应用配置）  
适用于 Web 应用、API 服务的配置，结构清晰且易解析。  
```json
{
  "server": {
    "port": 8080,
    "host": "0.0.0.0",
    "timeout": 30000,
    "enableCORS": true
  },
  "database": {
    "type": "mysql",
    "host": "localhost",
    "port": 3306,
    "username": "root",
    "password": "123456",
    "database": "myapp",
    "pool": {
      "maxSize": 20,
      "minIdle": 5
    }
  },
  "logging": {
    "level": "INFO",
    "file": "/var/log/myapp/app.log",
    "maxSize": "10MB",
    "maxBackups": 5
  },
  "features": {
    "enableAuth": true,
    "enableCache": false,
    "cacheTTL": 3600
  }
}
```

### 2. YAML 配置示例（K8s/应用配置）  
简洁易读，常用于 Kubernetes、Ansible 或后端框架配置。  
```yaml
# 应用服务配置
server:
  port: 8080
  host: 0.0.0.0
  timeout: 30s
  cors:
    enabled: true
    allowedOrigins: ["https://example.com", "http://localhost:3000"]

database:
  type: postgres
  host: db.example.com
  port: 5432
  credentials:
    username: app_user
    password: ${DB_PASSWORD}  # 环境变量引用
  database: app_db
  ssl: true

logging:
  level: DEBUG
  outputs:
    - type: file
      path: /var/log/app/app.log
    - type: console
      format: json

features:
  - name: "payment"
    enabled: true
    version: "v2"
  - name: "notification"
    enabled: false
```

### 3. TOML 配置示例（项目/工具配置）  
GitHub、Cargo（Rust）等工具常用，结构直观且支持注释。  
```toml
# 项目元信息
[package]
name = "my_app"
version = "1.0.0"
authors = ["Your Name <you@example.com>"]
edition = "2021"

# 服务器配置
[server]
port = 8080
host = "0.0.0.0"
timeout = 30_000  # 数字支持下划线分隔
enable_cors = true

# 数据库配置
[database]
type = "mysql"
host = "localhost"
port = 3306
username = "root"
password = "123456"

[database.pool]
max_size = 20
min_idle = 5

# 多环境配置
[env.production]
log_level = "WARN"

[env.development]
log_level = "DEBUG"
```

### 4. INI 配置示例（传统应用/工具配置）  
Windows 应用、Python 配置文件常用，分段式结构。  
```ini
[Server]
Port=8080
Host=0.0.0.0
Timeout=30000
EnableCORS=true

[Database]
Type=mysql
Host=localhost
Port=3306
Username=root
Password=123456
Database=myapp

[Database.Pool]
MaxSize=20
MinIdle=5

[Logging]
Level=INFO
File=/var/log/myapp/app.log
MaxSize=10MB
MaxBackups=5

; 注释示例
; 功能开关
[Features]
EnableAuth=1
EnableCache=0
CacheTTL=3600
```

### 5. CONF 配置示例（Nginx/服务配置）  
常用于 Nginx、Apache 等服务器配置，指令式结构。  
```nginx
# Nginx 服务器配置示例
server {
    listen 80;
    server_name example.com www.example.com;

    # 根目录与索引页
    root /var/www/example.com;
    index index.html index.htm;

    # 日志配置
    access_log /var/log/nginx/example.com.access.log;
    error_log /var/log/nginx/example.com.error.log warn;

    # 反向代理配置
    location /api/ {
        proxy_pass http://127.0.0.1:8080/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_connect_timeout 30s;
    }

    # 静态资源缓存
    location ~* \.(jpg|jpeg|png|css|js)$ {
        expires 7d;
        add_header Cache-Control "public, max-age=604800";
    }
}
```

### 6. XML 配置示例（Spring/Java 应用配置）  
常用于 Java 框架（如 Spring）、Web.xml 或数据交换。  
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- Spring 数据源配置示例 -->
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 数据源配置 -->
    <bean id="dataSource" class="com.zaxxer.hikari.HikariDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/myapp"/>
        <property name="username" value="root"/>
        <property name="password" value="123456"/>
        <property name="maximumPoolSize" value="20"/>
        <property name="minimumIdle" value="5"/>
    </bean>

    <!-- 服务器配置 -->
    <bean id="serverConfig" class="com.example.ServerConfig">
        <property name="port" value="8080"/>
        <property name="host" value="0.0.0.0"/>
        <property name="timeout" value="30000"/>
        <property name="enableCORS" value="true"/>
    </bean>

    <!-- 日志配置 -->
    <bean id="logConfig" class="com.example.LogConfig">
        <property name="level" value="INFO"/>
        <property name="filePath" value="/var/log/myapp/app.log"/>
        <property name="maxSize" value="10MB"/>
    </bean>
</beans>
```

这些示例覆盖了主流配置格式的常见场景，可直接作为项目配置的模板参考，根据实际需求调整参数即可。





# 额外测试

> 测试文字


## 列表
1. 将这些语法示例直接
2. 将这些语法示例直接
3. 将这些语法示例直接


- 将这些语法示例直接
- 将这些语法示例直接
- 将这些语法示例直接
- 将这些语法示例直接
