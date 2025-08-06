# 一、什么是元分析？
- One of its founding fathers, Gene V. Glass, described meta-analysis as an “analysis of analyses” (Glass, 1976). 
- 在传统研究中，分析的单位是一定数量的人、样本、国家或物体。在元分析中，**原始研究**本身成为我们分析的要素。
- 元分析旨在以**定量**的方式结合先前研究的结果。元分析的目标是将所选研究中报告的定量结果整合到一个数值估计中。这个估计总结了所有个体研究的结果。
```
根据定义，还存在一种第四种证据综合方法，称为个体参与者数据（IPD）荟萃分析（Richard D. Riley、Lambert 和 Abo-Zaid 2010；Richard D. Riley、Tierney 和 Stewart 2021）。
传统上，荟萃分析基于已发表文献中发现的研究的汇总结果（例如，均值和标准差，或比例）。
在个体参与者数据荟萃分析中，收集所有研究的原始数据，并将它们合并到一个大的数据集中。（但是原始数据非常难以获得，因此IPD荟萃分析的应用相对较少）
```
# 二、R语言介绍
## R Studio的安装与界面介绍

### R Studio的安装
#### 1. 安装R语言
在安装R Studio之前，需要先安装R语言。可以从[R官方网站](https://cran.r-project.org/)下载适合你操作系统（Windows、MacOS或Linux）的R安装包。

#### 2. 安装R Studio
- 访问[R Studio官方下载页面](https://www.rstudio.com/products/rstudio/download/)。
- 在页面中找到适合你操作系统的R Studio Desktop版本，点击下载。
- 下载完成后，运行安装程序，按照安装向导的提示一步步完成安装。

### R Studio界面介绍
R Studio的默认界面主要由四个窗格组成，以下是R Studio界面示例图：
![R Studio界面示例](https://bookdown.org/MathiasHarrer/Doing_Meta_Analysis_in_R/images/rstudio_1_col_sep.png)

#### 1. 控制台（Console）
- **位置**：通常位于左下方。
- **功能**：是与R语言交互的主要区域，可以直接在控制台中输入R代码并立即执行。例如，输入 `2 + 3` 并按下回车键，控制台会输出计算结果 `[1] 5`。

#### 2. 脚本编辑器（Script Editor）
- **位置**：通常位于左上方。
- **功能**：用于编写和编辑R脚本。可以将一系列的R代码保存到脚本文件中，方便后续修改和重复使用。编写好脚本后，可以选择部分代码按 `Ctrl + Enter`（Windows/Linux）或 `Command + Enter`（MacOS）执行选中的代码。

#### 3. 环境与历史记录（Environment & History）
- **位置**：通常位于右上方。
- **功能**：
  - **环境（Environment）**：显示当前工作环境中所有的变量、函数等对象。可以查看对象的名称、类型和值等信息。
  - **历史记录（History）**：记录了在控制台中执行过的所有命令，方便回顾和重新执行之前的命令。

#### 4. 文件、绘图、包与帮助（Files, Plots, Packages & Help）
- **位置**：通常位于右下方。
- **功能**：
  - **文件（Files）**：可以浏览和管理文件系统中的文件和文件夹，方便打开、保存和组织R脚本、数据文件等。
  - **绘图（Plots）**：显示使用R语言绘制的各种图形，如散点图、折线图等。
  - **包（Packages）**：列出已安装的R包，并可以进行包的安装、更新和卸载操作。
  - **帮助（Help）**：提供R语言函数和包的帮助文档，输入函数名（如 `help(mean)` 或 `?mean`）即可查看该函数的详细使用说明。
#### 5. 如何运行R代码
- 打开R Studio。
- 在脚本编辑器中编写R代码。
- 选择部分代码或整个脚本，按 `Ctrl + Enter`（Windows/Linux）或 `Command + Enter`（MacOS）执行选中的代码。
- 执行完成后，结果会显示在控制台中。
- 可以在控制台中直接输入R表达式并立即计算，也可以在脚本中使用 `print()` 函数将结果输出到控制台。

## Package
- 在R Studio中，Package是用于扩展R语言功能的集合。
- 要安装和加载Package，需要使用 `install.packages()` 函数安装Package，**一定要在Console中安装！！！**，包的名字要带`“”`,记得**回车**哦！
- 然后使用 `library()` 函数加载Package。
- 以下是一些元分析必备的R包，安装并加载这些包的代码示例如下：
  ```r
  # 安装metafor包，用于进行各种元分析
  install.packages("metafor")
  library(metafor)
  
  # 安装meta包，提供元分析的基础功能
  install.packages("meta")
  library(meta)
  
  # 安装tidyverse包，包含了一系列用于数据处理和可视化的包
  install.packages("tidyverse")
  library(tidyverse)
  
  # 安装ggplot2包，用于数据可视化
  install.packages("ggplot2")
  library(ggplot2)
  ```

  ## 数据准备          
### 数据导入的方法与操作指南

#### 1. 从CSV文件导入数据
CSV（Comma-Separated Values）是一种常见的文本文件格式，用于存储表格数据。可以使用 `read.csv()` 函数导入CSV文件。

**代码示例：**
```r
# 设置工作目录
setwd("/Users/yourname/Documents/data")  # 替换为你的数据文件所在路径

# 导入CSV文件
mydata <- read.csv("data.csv", header = TRUE, stringsAsFactors = FALSE)

# 查看数据前6行
head(mydata)
```

**操作界面示范：**
1. 在R Studio的文件窗格（右下方）中找到你的CSV文件
2. 点击该文件，会弹出一个对话框，选择"Import Dataset" -> "From Text (base)..."
3. 在弹出的导入对话框中，可以预览数据并设置导入选项
4. 点击"Import"按钮完成导入

#### 2. 从Excel文件导入数据
对于Excel文件（.xlsx或.xls格式），可以使用`readxl`包来导入数据。

**代码示例：**
```r
# 安装并加载readxl包
install.packages("readxl")
library(readxl)

# 导入Excel文件
mydata <- read_excel("data.xlsx", sheet = 1)  # sheet参数指定工作表，默认为第1个工作表

# 查看数据结构
dim(mydata)  # 查看数据的行数和列数
str(mydata)  # 查看数据的结构
```

**操作界面示范：**
1. 安装并加载`readxl`包
2. 在R Studio的文件窗格中找到你的Excel文件
3. 点击该文件，会弹出一个对话框，选择"Import Dataset" -> "From Excel..."
4. 在弹出的导入对话框中，选择要导入的工作表并设置导入选项
5. 点击"Import"按钮完成导入

![Excel文件导入界面](https://i.imgur.com/8L5zRvH.png)

#### 3. 从文本文件导入数据
对于以制表符、空格或其他分隔符分隔的文本文件，可以使用`read.table()`函数导入数据。

**代码示例：**
```r
# 导入以制表符分隔的文本文件
mydata <- read.table("data.txt", header = TRUE, sep = "\t")

# 导入以空格分隔的文本文件
mydata <- read.table("data.txt", header = TRUE, sep = " ")

# 导入以分号分隔的文本文件
mydata <- read.table("data.txt", header = TRUE, sep = ";")
```

#### 4. 从SPSS/SAS/Stata文件导入数据
对于其他统计软件格式的文件，可以使用`haven`包来导入数据。

**代码示例：**
```r
# 安装并加载haven包
install.packages("haven")
library(haven)

# 导入SPSS文件
mydata <- read_spss("data.sav")

# 导入SAS文件
mydata <- read_sas("data.sas7bdat")

# 导入Stata文件
mydata <- read_dta("data.dta")
```
#### 5. 直接在R中手动输入数据
如果数据量较小，也可以直接在R中手动输入数据。

**代码示例：**
```r
# 创建一个数据框
mydata <- data.frame(
  study = c("Study1", "Study2", "Study3", "Study4"),
  n = c(50, 60, 70, 80),
  mean = c(10.2, 12.5, 9.8, 11.3),
  sd = c(2.1, 1.8, 2.3, 1.9)
)

# 查看数据
print(mydata)
```

#### 数据导入后的检查与预处理
数据导入后，建议进行以下检查和预处理：

```r
# 查看数据的前几行和后几行
head(mydata)
tail(mydata)

# 查看数据的结构
dim(mydata)  # 行数和列数
str(mydata)  # 各列的类型和示例值

# 检查缺失值
sum(is.na(mydata))  # 总缺失值数量
colSums(is.na(mydata))  # 每列的缺失值数量

# 处理缺失值（例如删除含有缺失值的行）
mydata <- na.omit(mydata)

# 重命名列名
colnames(mydata) <- c("研究名称", "样本量", "均值", "标准差")
```
## 数据操作（Data manipulation）

### 数据转换（class conversion）
`glimpse()` 是 `dplyr` 包中的一个函数，可用于快速查看数据框的结构和内容。以下是使用 `glimpse()` 函数的示例代码：
```
library(dplyr)
glimpse(你的数据名称)
```
`glimpse()` 函数会展示数据集中每列存储的数据类型。在 R 语言里，这些数据类型被称为类（class），不同的缩写代表不同的数据类型：

- `<num>` 代表数值型（numeric）。所有以数字形式存储的数据都属于此类
- `<chr>` 代表字符型（character）。所有以文本形式存储的数据都属于此类。
- `<log>` 代表逻辑型（logical）。这类变量是二进制的，意味着它们表示某个条件为 `TRUE` 或 `FALSE`。
- `<factor>` 代表因子型（factor）。因子以数字形式存储，每个数字代表变量的一个不同水平。例如，一个变量的可能因子水平为 1 = “低”，2 = “中”，3 = “高”。

除了使用 `glimpse()` 函数，我们还可以使用 `class()` 函数来检查某一列的类别。在数据框中，我们可以通过在数据框名称后添加 `$` 运算符，再加上列名来直接访问该列。
例如：
```
class(远远做的数据集$研究名称)

```
运行结果为：
```
[1] "character"
```
这表示 `研究名称` 列是字符型（character）的。

这时候我们需要转换数据啦！
```
# 转换为因子型
远远做的数据集$研究名称 <- as.factor(远远做的数据集$研究名称)

# 转换为数值型
远远做的数据集$样本量 <- as.numeric(远远做的数据集$样本量)

# 转换为逻辑型
远远做的数据集$结果 <- as.logical(远远做的数据集$结果)
```
我来设计一个问题：如何将农业水稻数据集中产量一列，用逻辑来表示，大于1000为TRUE，小于1000为FALSE？
```
# 转换为逻辑型
农业水稻数据集$产量 <- as.logical(农业水稻数据集$产量 > 1000)
```
运行结果为：
```
[1] TRUE FALSE TRUE FALSE TRUE TRUE FALSE TRUE FALSE TRUE
```
这表示，产量大于1000的为TRUE，小于1000的为FALSE。

### 数据切片（Data Slicing）
让我们先创建一个农业方面简单的数据集，用于演示数据切片操作。以下是创建数据集的代码示例：
```
# 创建一个数据框
农业水稻数据集 <- data.frame(
  研究名称 = c("研究1", "研究2", "研究3", "研究4"),
  样本量 = c(50, 60, 70, 80),
  产量 = c(1000, 1200, 900, 1100)
)
```
运行结果为：
```
  研究名称 样本量 产量
1    研究1    50 1000
2    研究2    60 1200
3    研究3    70  900
4    研究4    80 1100
```

以下是使用上面创建的 `农业水稻数据集` 进行数据切片的几种方法：
- **直接用`$`运算符**：
- **用`[ ]`运算符**
  - **用`[行, 列]`的格式**：获取第2行第3列的数据（注意：行和列的索引从1开始）
    ```r
    农业水稻数据集[2, 3]
    ```
    运行结果为：
    ```
    [1] 1200
    ```
  - **用`[行, ]`或`[, 列]`的格式**：获取第3行所有列的数据和第1列所有行的数据
    ```r
    农业水稻数据集[3, ]
    农业水稻数据集[, 1]
    ```
    运行结果为：
    ```
    [1] 研究3
    ```
    运行结果为：
    ```
    [1] 研究1 研究2 研究3 研究4
    ```
- **用`subset()`函数**：获取 `产量` 大于1000的数据
  ```r
  subset(农业水稻数据集, 产量 > 1000)
  ```
- **用`dplyr`包中的`filter()`函数**：获取 `样本量` 大于60的数据
  ```r
  library(dplyr)
  filter(农业水稻数据集, 样本量 > 60)
  ```
  运行结果为：
  ```
  研究名称 样本量 产量
1    研究2    60 1200
2    研究3    70  900

### 使用 `c()` 函数结合 `[ ]` 运算符，其实会英文很重要，c就是concatenate的缩写，意思是拼接哦（后面我也会多采用英文）
- **选择多行数据**：获取第1行和第3行的数据
  ```r
  农业水稻数据集[c(1, 3), ]
  ```
  运行结果预期为：
  ```
    研究名称 样本量 产量
  1    研究1    50 1000
  2    研究3    70  900
  ```
- **选择多列数据**：获取第1列和第3列的数据
  ```r
  农业水稻数据集[, c(1, 3)]
  ```
  运行结果预期为：
  ```
    研究名称 产量
  1    研究1 1000
  2    研究2 1200
  3    研究3  900
  4    研究4 1100
  ```
- **选择多行多列数据**：获取第2行和第4行的第2列和第3列数据
  ```r
  农业水稻数据集[c(2, 4), c(2, 3)]
  ```
  运行结果预期为：
  ```
    样本量 产量
  1    60 1200
  2    80 1100
  ```
在 R 语言里，`%in%` 是一个常用的运算符，用于判断某个元素是否存在于一个向量中。以下是使用 `%in%` 运算符的示例，我们依旧使用之前创建的 `农业水稻数据集`：

### 使用 `%in%` 运算符筛选数据
- **筛选特定研究名称的数据**：筛选出 `研究名称` 为 "研究1" 或 "研究3" 的数据
  ```r
  农业水稻数据集[农业水稻数据集$研究名称 %in% c("研究1", "研究3"), ]
  ```
  运行结果预期为：
  ```
    研究名称 样本量 产量
  1    研究1    50 1000
  2    研究3    70  900
  ```
- **筛选特定产量的数据**：筛选出 `产量` 为 1000 或 1200 的数据
  ```r
  农业水稻数据集[农业水稻数据集$产量 %in% c(1000, 1200), ]
  ```
  运行结果预期为：
  ```
    研究名称 样本量 产量
  1    研究1    50 1000
  2    研究2    60 1200
  ```
# 三、效应值（Effect Size）
## Measures & Effect Sizes in Observational Designs

