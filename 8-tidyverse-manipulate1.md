---
output:
  html_document: default
  pdf_document: default
---

# 数据操作-part1 {#tidyverse-manipulate1}




```r
path <- "datas/new/Students-Grade.xlsx"
df <- map_dfr(set_names(excel_sheets(path)), 
              ~ read_xlsx(path, .x), id = "sheet")
df
```

```
## # A tibble: 20 x 6
##    班级  姓名   性别   语文  数学  英语
##    <chr> <chr>  <chr> <dbl> <dbl> <dbl>
##  1 六1班 何娜   女       87    92    79
##  2 六1班 黄才菊 女       95    77    75
##  3 六1班 陈芳妹 女       79    87    66
##  4 六1班 陈学勤 男       82    79    66
##  5 六2班 黄祖娜 女       94    88    75
##  6 六2班 徐雅琦 女       92    86    72
##  7 六2班 徐达政 男       90    86    72
##  8 六2班 陈华健 男       92    84    70
##  9 六3班 江佳欣 女       80    69    75
## 10 六3班 何诗婷 女       76    53    72
## 11 六3班 林可莉 女       72    52    72
## 12 六3班 雷帆   男       78    56    66
## 13 六4班 周婵   女       92    94    77
## 14 六4班 李小龄 男       90    87    69
## 15 六4班 陈丽丽 女       87    93    61
## 16 六4班 杨昌晟 男       84    85    64
## 17 六5班 符苡榕 女       85    89    76
## 18 六5班 陆曼   女       88    84    69
## 19 六5班 容唐   女       83    71    56
## 20 六5班 蒙丽梅 女       72    72    64
```

## 基本数据操作

1. 对所有列的操作有`across()`函数，可以搭配`mutate()`和`summarise()`函数对行进行多种操作。基本格式为`across(.cols = everything(), .fns = NULL, ..., .names)`。

   - .cols：根据**选择列**的语法选定要操作的列的范围。
   - .fns：为应用到选定列上的函数：
   
      - NULL：不对列做变换
      - 一个函数，如`mean`
      - 一个`purrr`风格的匿名函数，如~ .x * 19
      - 多个函数或匿名函数构成的**列表**
   - .names：设置输出列名的格式，默认为`{col}_{fn}`


```r
# 1. 应用函数到所有列
df %>% 
  mutate(across(everything(), as.character))
```

```
## # A tibble: 20 x 6
##    班级  姓名   性别  语文  数学  英语 
##    <chr> <chr>  <chr> <chr> <chr> <chr>
##  1 六1班 何娜   女    87    92    79   
##  2 六1班 黄才菊 女    95    77    75   
##  3 六1班 陈芳妹 女    79    87    66   
##  4 六1班 陈学勤 男    82    79    66   
##  5 六2班 黄祖娜 女    94    88    75   
##  6 六2班 徐雅琦 女    92    86    72   
##  7 六2班 徐达政 男    90    86    72   
##  8 六2班 陈华健 男    92    84    70   
##  9 六3班 江佳欣 女    80    69    75   
## 10 六3班 何诗婷 女    76    53    72   
## 11 六3班 林可莉 女    72    52    72   
## 12 六3班 雷帆   男    78    56    66   
## 13 六4班 周婵   女    92    94    77   
## 14 六4班 李小龄 男    90    87    69   
## 15 六4班 陈丽丽 女    87    93    61   
## 16 六4班 杨昌晟 男    84    85    64   
## 17 六5班 符苡榕 女    85    89    76   
## 18 六5班 陆曼   女    88    84    69   
## 19 六5班 容唐   女    83    71    56   
## 20 六5班 蒙丽梅 女    72    72    64
```

```r
# 2. 应用函数到指定列
as.tibble(iris) %>% 
  mutate(across(contains("Length")|contains("Width"), ~ .x * 10))
```

```
## Warning: `as.tibble()` was deprecated in tibble 2.0.0.
## Please use `as_tibble()` instead.
## The signature and semantics have changed, see `?as_tibble`.
## This warning is displayed once every 8 hours.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was generated.
```

```
## # A tibble: 150 x 5
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species
##           <dbl>       <dbl>        <dbl>       <dbl> <fct>  
##  1           51          35           14           2 setosa 
##  2           49          30           14           2 setosa 
##  3           47          32           13           2 setosa 
##  4           46          31           15           2 setosa 
##  5           50          36           14           2 setosa 
##  6           54          39           17           4 setosa 
##  7           46          34           14           3 setosa 
##  8           50          34           15           2 setosa 
##  9           44          29           14           2 setosa 
## 10           49          31           15           1 setosa 
## # ... with 140 more rows
```

2. 对所有行操作的函数有`if_any()`和`if_all()`函数，搭配`filter()`函数可以根据多列的值对行进行筛选的效果。基本格式为`if_any(.cols, .fns, ...)`。

   - .cols：选则列的范围
   - .fns：在.cols的范围内，筛选判断条件为.fns的行。


```r
# 1. 筛选所有值都满足某条件的行
df %>% 
  filter(if_all(4:6, ~ .x > 75))
```

```
## # A tibble: 3 x 6
##   班级  姓名   性别   语文  数学  英语
##   <chr> <chr>  <chr> <dbl> <dbl> <dbl>
## 1 六1班 何娜   女       87    92    79
## 2 六4班 周婵   女       92    94    77
## 3 六5班 符苡榕 女       85    89    76
```

```r
# 2. 筛选存在值满足某条件的行
df %>% 
  filter(if_any(where(is.numeric), ~ .x > 90)) # 筛选存在大于90的行
```

```
## # A tibble: 7 x 6
##   班级  姓名   性别   语文  数学  英语
##   <chr> <chr>  <chr> <dbl> <dbl> <dbl>
## 1 六1班 何娜   女       87    92    79
## 2 六1班 黄才菊 女       95    77    75
## 3 六2班 黄祖娜 女       94    88    75
## 4 六2班 徐雅琦 女       92    86    72
## 5 六2班 陈华健 男       92    84    70
## 6 六4班 周婵   女       92    94    77
## 7 六4班 陈丽丽 女       87    93    61
```

```r
df %>% 
  filter(if_any(where(is.character), is.na))
```

```
## # A tibble: 0 x 6
## # ... with 6 variables: 班级 <chr>, 姓名 <chr>, 性别 <chr>,
## #   语文 <dbl>, 数学 <dbl>, 英语 <dbl>
```


3. `set_names()`为所有列设置新列名；`rename()`只修改部分列名


```r
df <- read_excel("datas/new/ExamDatas_NAs.xlsx")

# 修改全部列名
df %>% 
  set_names(" 班级", " 姓名", " 性别", " 语文",
            " 数学", " 英语", " 品德", " 科学")
```

```
## # A tibble: 50 x 8
##    ` 班级` ` 姓名` ` 性别` ` 语文` ` 数学` ` 英语` ` 品德` ` 科学`
##    <chr>   <chr>   <chr>     <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
##  1 六1班   何娜    女           87      92      79       9      10
##  2 六1班   黄才菊  女           95      77      75      NA       9
##  3 六1班   陈芳妹  女           79      87      66       9      10
##  4 六1班   陈学勤  男           NA      79      66       9      10
##  5 六1班   陈祝贞  女           76      79      67       8      10
##  6 六1班   何小薇  女           83      73      65       8       9
##  7 六1班   雷旺    男           NA      80      68       8       9
##  8 六1班   陈欣越  男           57      80      60       9       9
##  9 六1班   黄亦婷  女           77      NA      54       8      10
## 10 六1班   陈媚    女           68      55      66       8       9
## # ... with 40 more rows
```

```r
# 修改部分列名
df %>% 
  rename(数学 = math, 科学 = science)
```

```
## # A tibble: 50 x 8
##    class name   sex   chinese  数学 english moral  科学
##    <chr> <chr>  <chr>   <dbl> <dbl>   <dbl> <dbl> <dbl>
##  1 六1班 何娜   女         87    92      79     9    10
##  2 六1班 黄才菊 女         95    77      75    NA     9
##  3 六1班 陈芳妹 女         79    87      66     9    10
##  4 六1班 陈学勤 男         NA    79      66     9    10
##  5 六1班 陈祝贞 女         76    79      67     8    10
##  6 六1班 何小薇 女         83    73      65     8     9
##  7 六1班 雷旺   男         NA    80      68     8     9
##  8 六1班 陈欣越 男         57    80      60     9     9
##  9 六1班 黄亦婷 女         77    NA      54     8    10
## 10 六1班 陈媚   女         68    55      66     8     9
## # ... with 40 more rows
```

4. 重新编码：


```r
# 1. 两种类别情况：if_else
df %>% 
  mutate(if_else(sex == "男", "M", "F"))
```

```
## # A tibble: 50 x 9
##    class name   sex   chinese  math english moral science
##    <chr> <chr>  <chr>   <dbl> <dbl>   <dbl> <dbl>   <dbl>
##  1 六1班 何娜   女         87    92      79     9      10
##  2 六1班 黄才菊 女         95    77      75    NA       9
##  3 六1班 陈芳妹 女         79    87      66     9      10
##  4 六1班 陈学勤 男         NA    79      66     9      10
##  5 六1班 陈祝贞 女         76    79      67     8      10
##  6 六1班 何小薇 女         83    73      65     8       9
##  7 六1班 雷旺   男         NA    80      68     8       9
##  8 六1班 陈欣越 男         57    80      60     9       9
##  9 六1班 黄亦婷 女         77    NA      54     8      10
## 10 六1班 陈媚   女         68    55      66     8       9
## # ... with 40 more rows, and 1 more variable:
## #   `if_else(sex == "男", "M", "F")` <chr>
```

```r
# 2. 多种类别情况：case_when
df %>% 
  mutate(math = case_when(math >= 75 ~ "High",
                   math >= 60 ~ "Middle",
                   TRUE       ~ "Low"))
```

```
## # A tibble: 50 x 8
##    class name   sex   chinese math   english moral science
##    <chr> <chr>  <chr>   <dbl> <chr>    <dbl> <dbl>   <dbl>
##  1 六1班 何娜   女         87 High        79     9      10
##  2 六1班 黄才菊 女         95 High        75    NA       9
##  3 六1班 陈芳妹 女         79 High        66     9      10
##  4 六1班 陈学勤 男         NA High        66     9      10
##  5 六1班 陈祝贞 女         76 High        67     8      10
##  6 六1班 何小薇 女         83 Middle      65     8       9
##  7 六1班 雷旺   男         NA High        68     8       9
##  8 六1班 陈欣越 男         57 High        60     9       9
##  9 六1班 黄亦婷 女         77 Low         54     8      10
## 10 六1班 陈媚   女         68 Low         66     8       9
## # ... with 40 more rows
```

5. 删除行

   - 删除重复行：`distinct()`函数
   - 删除有缺失值的行：`drop_na()`函数
   

```r
df %>% 
  distinct()
```

```
## # A tibble: 50 x 8
##    class name   sex   chinese  math english moral science
##    <chr> <chr>  <chr>   <dbl> <dbl>   <dbl> <dbl>   <dbl>
##  1 六1班 何娜   女         87    92      79     9      10
##  2 六1班 黄才菊 女         95    77      75    NA       9
##  3 六1班 陈芳妹 女         79    87      66     9      10
##  4 六1班 陈学勤 男         NA    79      66     9      10
##  5 六1班 陈祝贞 女         76    79      67     8      10
##  6 六1班 何小薇 女         83    73      65     8       9
##  7 六1班 雷旺   男         NA    80      68     8       9
##  8 六1班 陈欣越 男         57    80      60     9       9
##  9 六1班 黄亦婷 女         77    NA      54     8      10
## 10 六1班 陈媚   女         68    55      66     8       9
## # ... with 40 more rows
```

```r
df %>% 
  drop_na()
```

```
## # A tibble: 30 x 8
##    class name   sex   chinese  math english moral science
##    <chr> <chr>  <chr>   <dbl> <dbl>   <dbl> <dbl>   <dbl>
##  1 六1班 何娜   女         87    92      79     9      10
##  2 六1班 陈芳妹 女         79    87      66     9      10
##  3 六1班 陈祝贞 女         76    79      67     8      10
##  4 六1班 何小薇 女         83    73      65     8       9
##  5 六1班 陈欣越 男         57    80      60     9       9
##  6 六1班 陈媚   女         68    55      66     8       9
##  7 六2班 黄祖娜 女         94    88      75    10      10
##  8 六2班 陈华健 男         92    84      70     9      10
##  9 六2班 杨远芸 女         93    80      68     9      10
## 10 六2班 林师满 男         70    74      25     8      10
## # ... with 20 more rows
```
6. 分组：

::: {.rmdtip data-latex="{提示}"}
**分组**是一种强大的数据思维，当我们需要分组并分别操作每组数据时，应优先采用`group_by()`+操作，而不是*分割数据 + 循环迭代*。
:::
   

```r
df_grp <- df %>% 
  group_by(class) %>% 
  summarise(
    across(where(is.numeric),
           list(sum = sum, mean = mean, min = min),
           na.rm = TRUE)
  ) %>% 
  pivot_longer(
    cols = -class,
    names_to = c("Subject", ".value"),  # 此处为.value而不是.values
    names_sep = "_"
  )
df_grp
```

```
## # A tibble: 30 x 5
##    class Subject   sum  mean   min
##    <chr> <chr>   <dbl> <dbl> <dbl>
##  1 六1班 chinese   622 77.8     57
##  2 六1班 math      702 78       55
##  3 六1班 english   666 66.6     54
##  4 六1班 moral      76  8.44     8
##  5 六1班 science    95  9.5      9
##  6 六2班 chinese   746 82.9     66
##  7 六2班 math      570 71.2     41
##  8 六2班 english   468 52       25
##  9 六2班 moral      69  8.62     6
## 10 六2班 science    73  9.12     7
## # ... with 20 more rows
```

## 其它数据操作

### 按行汇总

通常的数据逻辑都是按列进行汇总，按行汇总时比较困难。

`dplyr`包中中`rowwise()`函数为数据框创建按行方式进行汇总的方式：`rowwise()`不是真的改变数据框，只是创建了按行元信息，改变了数据框的逻辑。


```r
# 行逻辑，对每行所选列的数据进行操作
df %>% 
  rowwise() %>% 
  mutate(total = sum(c(chinese, math, english), na.rm = TRUE))
```

```
## # A tibble: 50 x 9
## # Rowwise: 
##    class name   sex   chinese  math english moral science total
##    <chr> <chr>  <chr>   <dbl> <dbl>   <dbl> <dbl>   <dbl> <dbl>
##  1 六1班 何娜   女         87    92      79     9      10   258
##  2 六1班 黄才菊 女         95    77      75    NA       9   247
##  3 六1班 陈芳妹 女         79    87      66     9      10   232
##  4 六1班 陈学勤 男         NA    79      66     9      10   145
##  5 六1班 陈祝贞 女         76    79      67     8      10   222
##  6 六1班 何小薇 女         83    73      65     8       9   221
##  7 六1班 雷旺   男         NA    80      68     8       9   148
##  8 六1班 陈欣越 男         57    80      60     9       9   197
##  9 六1班 黄亦婷 女         77    NA      54     8      10   131
## 10 六1班 陈媚   女         68    55      66     8       9   189
## # ... with 40 more rows
```

```r
# 列逻辑，对所选列的所有数据进行操作
df %>% 
  mutate(total = sum(c(chinese, math, english), na.rm = TRUE))
```

```
## # A tibble: 50 x 9
##    class name   sex   chinese  math english moral science total
##    <chr> <chr>  <chr>   <dbl> <dbl>   <dbl> <dbl>   <dbl> <dbl>
##  1 六1班 何娜   女         87    92      79     9      10  9739
##  2 六1班 黄才菊 女         95    77      75    NA       9  9739
##  3 六1班 陈芳妹 女         79    87      66     9      10  9739
##  4 六1班 陈学勤 男         NA    79      66     9      10  9739
##  5 六1班 陈祝贞 女         76    79      67     8      10  9739
##  6 六1班 何小薇 女         83    73      65     8       9  9739
##  7 六1班 雷旺   男         NA    80      68     8       9  9739
##  8 六1班 陈欣越 男         57    80      60     9       9  9739
##  9 六1班 黄亦婷 女         77    NA      54     8      10  9739
## 10 六1班 陈媚   女         68    55      66     8       9  9739
## # ... with 40 more rows
```

### 整洁计算

我们在`tidyverse`框架下自定义函数时，需要注意**整洁计算**（tidy evaluation）框架。

所谓**整洁计算**，有两种基本的形式：

1. 数据屏蔽：使得可以不用带数据框（环境变量）名字，就能适用数据框内的变量（数据变量），便于在数据集内进行计算操作。

2. 整洁选择：即可以使用各种选择列的语法，便于选择数据集中的列。

数据屏蔽带了了代码的简洁，但作为函数的参数间接使用时，正常情况下是环境变量，如果希望作为数据变量使用，则需要用两个大括号括起来：


```r
var_summary <- function(data, var){
  data %>% 
    summarise(n = n(), mean = mean({{var}}))
}

mtcars %>% 
  group_by(cyl) %>% 
  var_summary(mpg)
```

```
## # A tibble: 3 x 3
##     cyl     n  mean
##   <dbl> <int> <dbl>
## 1     4    11  26.7
## 2     6     7  19.7
## 3     8    14  15.1
```


```r
my_summarise <- function(data, mean_var, sd_var){
  data %>% 
    summarise("mean_{{mean_var}}" := mean({{mean_var}}),  # :=代表
              "sd_{{sd_var}}" := mean({{sd_var}}))
}
mtcars %>% 
  group_by(cyl) %>% 
  my_summarise(mpg, disp)
```

```
## # A tibble: 3 x 3
##     cyl mean_mpg sd_disp
##   <dbl>    <dbl>   <dbl>
## 1     4     26.7    105.
## 2     6     19.7    183.
## 3     8     15.1    353.
```

[搞清楚符号`:=`的含义]{.todo}
