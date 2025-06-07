# 高级编程 - 作业 5
<p align="center"><b>作业 5 - 2022 年春季学期 <br> 截止日期：伊朗历法尔瓦丁月 31 日星期六 - 晚上 11:59</b></p>

## 大纲

在本次作业中，我们将探讨 C++ 中的继承和多态性。我们将使用各种原料制作不同种类的意式浓缩咖啡饮品。如你所知，所有意式浓缩咖啡饮品（如卡布奇诺、摩卡、拿铁等）的配方中都含有意式浓缩咖啡，它们的区别在于其他原料，如牛奶、奶泡、水、巧克力等。

我们将实现一个名为 `Ingredients` 的基类，并从该基类派生出所有所需的原料类。
我们还将实现另一个名为 `EspressoBased` 的类，并从该类派生出意式浓缩咖啡饮品类。

<br>
<p align="center">
<img src="resources/coffee.jpeg" alt="minor"
title="coffee" width="300" align="middle" />
</p>
<br>
在上面的图片中，你可以看到最著名的意式浓缩咖啡饮品的配方。花点时间看看这张图片，在本次作业中你需要制作其中的几种咖啡。

</br>

# 原料类
定义一个名为 `Ingredient` 的**抽象类**，并为该类添加以下函数：

```cpp
class Ingredient
{
public:
    double get_price_unit();
    size_t get_units();
    std::string get_name();

    double price();
    

protected:
    Ingredient(double price_unit, size_t units);

    double price_unit;
    size_t units;
    std::string name;
};
```

这个类的函数很直观。对于成员变量：`price_unit` 表示该原料每单位的价格。`units` 表示我们需要该原料的数量。`price()` 函数将计算该原料的最终价格。`name` 是原料的名称，可以是意式浓缩咖啡、牛奶、巧克力等。

由于原料的名称尚未确定，将 `get_name()` 设为纯虚函数，从而使该类成为抽象类。

***注意***：你不需要在 `.cpp` 文件中定义函数，所有实现都放在 `ingredient.h` 文件中。

***问题***：你认为为什么构造函数和变量被定义为 `protected` 而不是 `private`？在完成代码后，在报告中回答这个问题。

</br>

# 具体原料类
现在我们将使用上述类来实现实际需要的原料类。在本次作业中，我们需要 8 种原料，分别是：
- **肉桂** - *每单位价格：5*
- **巧克力** - *每单位价格：5*
- **糖** - *每单位价格：1*
- **饼干** - *每单位价格：10*
- **意式浓缩咖啡** - *每单位价格：15*
- **牛奶** - *每单位价格：10*
- **奶泡** - *每单位价格：5*
- **水** - *每单位价格：1*

从基类（`Ingredient` 类）派生出每个原料的类，并为其分配必要的值，以使测试用例能够通过。如下所示：

```cpp
class Cinnamon
{
public:
    Cinnamon(size_t units) : Ingredient{5, units}
    {
        this->name = "Cinnamon";
    }

    virtual std::string get_name() {return this->name;}
};

```

***注意！！！*** 你是否觉得为每种原料重复编写相同的代码是错误且浪费时间的？你说得没错！查看挑战部分，以避免为每个类重复编写代码。

***注意***：将这些类的所有实现都放在 `sub_ingredient.h` 文件中（无需将每个类分隔到不同的文件中）。

</br> 

# 意式浓缩咖啡饮品基类
使用以下代码片段定义一个名为 `EspressoBased` 的**抽象类**：

```cpp
class EspressoBased
{
public:
    virtual std::string get_name() = 0;
    virtual double price() = 0;

    void brew();
    std::vector<Ingredient*>& get_ingredients();

    ~EspressoBased();

protected:
    EspressoBased();
    EspressoBased(const EspressoBased& esp);
    void operator=(const EspressoBased& esp);

    std::vector<Ingredient*> ingredients;
    std::string name;

};
```
我们将使用这个抽象类作为基类来创建咖啡饮品类。如大纲部分的图片所示，每种意式浓缩咖啡饮品由不同的原料制成。这些原料将存储在 `ingredients` 变量中。
`get_ingredients()` 函数将返回这个变量。`price()` 函数将计算咖啡的最终价格，`name` 是咖啡的名称。

为该类实现默认构造函数和拷贝构造函数。由于我们使用了动态指针，需要定义析构函数来删除 `ingredients` 向量中的所有指针。使用以下代码实现：

```cpp
EspressoBased::~EspressoBased()
{
    for(const auto& i : ingredients)
        delete i;
    ingredients.clear();
}
```

**`brew()` 函数**：这个函数必须展示制作咖啡的步骤。你对这个函数的实现越好，在本次作业中获得的分数就越高。你甚至可以使用 UI 库来实现。你可以在下面的动图中看到这个函数的一个良好输出示例（这个输出是使用终端 UI 库实现的）。

<br>
<p align="center">
<img src="resources/brew.gif" alt="minor"
title="brew" width="500" align="middle" />
</p>
<br>

在本次作业中，如果你想在这部分使用外部库，可以修改 `Dockerfile`、`CmakeLists.txt` 或添加新文件。

***注意***：将这个类实现在 `espresso_based.h/.cpp` 文件中。

***问题***：如果你将析构函数 `~EspressoBased()` 定义在 `protected` 部分会发生什么？在报告中解释你的答案。

</br>

# 卡布奇诺类
从上述抽象类派生出 `Cappuccino` 类，并使用制作卡布奇诺所需的原料。制作一杯卡布奇诺需要 2 单位的意式浓缩咖啡、2 单位的牛奶和 1 单位的奶泡。

```cpp
class Cappuccino
{
public:
    Cappuccino();
    Cappuccino(const Cappuccino& cap);
    ~Cappuccino();
    void operator=(const Cappuccino& cap);

    virtual std::string get_name();
    virtual double price();

    void add_side_item(Ingredient* side);
    std::vector<Ingredient*>& get_side_items();

private:
    std::vector<Ingredient*> side_items;

};
```
除了制作卡布奇诺所需的基本原料外，顾客可能还会要求添加一些配料（如糖、额外的牛奶等）。`add_side_item()` 函数负责添加这些配料（计算咖啡的最终价格时，要同时考虑基本原料和配料）。

为这个类实现默认构造函数、拷贝构造函数和赋值运算符。由于我们使用了动态指针，还需要实现析构函数来删除它们。使用以下代码实现：

```cpp
Cappuccino::~Cappuccino()
{
    for(const auto& i : side_items)
        delete i;
    side_items.clear();
}
```

***注意***：将这个类实现在 `cappuccino.h/.cpp` 文件中。

</br>

# 摩卡类
从 `EspressoBased` 抽象类派生出 `Mocha` 类，就像派生 `Cappuccino` 类一样，使用制作摩卡所需的原料。制作一杯摩卡需要 2 单位的意式浓缩咖啡、2 单位的牛奶、1 单位的奶泡和 1 单位的巧克力。

</br>

# 挑战
- 在作业的“具体原料类”部分，你可能已经注意到，为了实现原料类，你需要一遍又一遍地重复几乎相同的代码。幸运的是，有一种方法可以避免这种重复，那就是使用 `宏`。你已经知道如何在 C++ 中使用最简单的宏，如下所示：

  ```cpp
  #define PI 3.1413
  ```

  但你可以用宏做更多的事情，所以去搜索一下相关内容。在 `sub_ingredients.h` 文件中定义一个名为 `DEFCLASS` 的宏，这样只需使用以下代码就能定义所有所需的类：

  ```cpp
  DEFCLASS(Cinnamon, 5);
  DEFCLASS(Chocolate, 5);
  DEFCLASS(Sugar, 1);
  DEFCLASS(Cookie, 10);
  DEFCLASS(Espresso, 15);
  DEFCLASS(Milk, 10);
  DEFCLASS(MilkFoam, 5);
  DEFCLASS(Water, 1);
  ```

  上述代码应该使用你的宏实现所有原料类。`DEFCLASS` 的输入参数为：1. 类的名称；2. 原料的每单位价格。

  ***请注意***：虽然在现代 C++ 中不鼓励使用宏，但对于像你这样的专业程序员来说，了解如何使用宏非常重要。所以学习使用宏，但除非绝对必要，否则尽量不要使用它。

</br>

# 最后
如前所述，除非另有说明，否则不要修改已有的其他文件。如果你想测试代码，可以使用 `main.cpp` 文件中的 `debug` 部分。

```cpp
if (true) // 将其改为 false 以运行单元测试  
{ 
    // 调试部分 
}  
else  
{  
    ::testing::InitGoogleTest(&argc, argv);  
    std::cout << "正在运行测试 ..." << std::endl;  
    int ret{RUN_ALL_TESTS()};  
    if (!ret)  
        std::cout << "<<<成功>>>" << std::endl;  
    else  
        std::cout << "失败" << std::endl;  
}  
return 0;
```
<br/>
<p align="center"><b>祝你好运</b></p>