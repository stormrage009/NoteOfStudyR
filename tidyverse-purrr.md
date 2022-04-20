---
output:
  html_document: default
  pdf_document: default
---

# 函数式编程 {#sec:purrr}



`tidyverse`包对数据操作的核心思维是**向量化**，即使用自定义函数（或现成函数）解决分解的小问题，进而使用泛函式循环迭代解决完整问题。关键有三：

1. 将**向量化**的编程思维，纳入数据框或更高级的数据结构中：

   - 对想做的操作自定义操作函数（或使用现成函数）。
   
   - 将上一步的函数（自定义/现成）应用到数据框的多个列上，对列进行修改或汇总。
   
2. 将复杂的操作分解为若干基本数据操作的能力：

   - 复杂的数据操作都可以分解为若干简单的基本数据操作，数据连接、数据重塑（长宽变换/拆分合并列）、筛选行、排序行、选择列、修改列、分组汇总。
   
   - 完成问题的梳理和分解后，使用“管道”流以此对数据进行相应的基本操作即可。
   
3. 接受数据分解的操作思维：

   - `across()`函数可以同时操作多个列，实际只需要将对一个列的操作实现后，就可以使用该函数对所有需要操作的列进行循环迭代，在将结果进行合并即可。
   
   - 事实上，`tidyverse`中有很多函数可以帮助我们实现**分解+分别操作+合并结果**的过程，我们只需要关心**分别操作**的部分就好。

`tidyverse`核心包的数据操作过程如下图（图\@ref(fig:tidyverse-flow)）所示。


```r
knitr::include_graphics("images/tidyverse-flow.jpg", dpi = FALSE)
```

<div class="figure" style="text-align: center">
<img src="images/tidyverse-flow.jpg" alt="tidyverse核心包工作流"  />
<p class="caption">(\#fig:tidyverse-flow)tidyverse核心包工作流</p>
</div>

:::: {.rmdnote data-latex="{注意}"}
- 数据经过管道默认传递给函数的第一个参数，此时通常可以省略。

- 如果不是在第一个参数的位置使用数据，则需要用符号`.`代替，此时该符号不能被省略。

- 对于`.`的用法，建议单独使用`.`符号时代替数据，而使用`.x`用于`purrr`中的匿名函数。


```r
mtcars %>% 
  group_split(cyl) %>%  # 数据mtcars作为第一个参数，省略。
  # 数据首先作为第一个参数传入map函数，省略。
  # 然后传入lm函数中，作为第二个参数，不能省略，且为purrr风格匿名函数，使用.x
  map(~ lm(mpg ~ wt, data = .x))  
```

```
## [[1]]
## 
## Call:
## lm(formula = mpg ~ wt, data = .x)
## 
## Coefficients:
## (Intercept)           wt  
##      39.571       -5.647  
## 
## 
## [[2]]
## 
## Call:
## lm(formula = mpg ~ wt, data = .x)
## 
## Coefficients:
## (Intercept)           wt  
##       28.41        -2.78  
## 
## 
## [[3]]
## 
## Call:
## lm(formula = mpg ~ wt, data = .x)
## 
## Coefficients:
## (Intercept)           wt  
##      23.868       -2.192
```
::::

## `purrr

### map()系列函数


```r
knitr::include_graphics("images/map_function3.png")
```

<img src="images/map_function3.png" width="320" style="display: block; margin: auto;" />

### 匿名函数


```r
exams <- list(
  student1 = round(runif(10, 50, 100)),
  student2 = round(runif(10, 50, 100)),
  student3 = round(runif(10, 50, 100)),
  student4 = round(runif(10, 50, 100)),
  student5 = round(runif(10, 50, 100))
)
exams
```

```
## $student1
##  [1] 75 69 69 81 70 58 89 75 53 79
## 
## $student2
##  [1] 69 70 91 82 67 57 79 64 80 53
## 
## $student3
##  [1]  51  84  63  86  67 100  76  86  59  60
## 
## $student4
##  [1]  67  95  66  91  96  55  73  53  96 100
## 
## $student5
##  [1] 80 82 59 71 96 94 73 87 68 62
```

对于exams数据框，我们有几种办法对其中每个向量进行去中心化。

1. 定义去中心化的function()。


```r
my_fun <- function(x){
  x - mean(x)
}
exams %>% 
  map_df(my_fun)
```

```
## # A tibble: 10 x 5
##    student1 student2 student3 student4 student5
##       <dbl>    <dbl>    <dbl>    <dbl>    <dbl>
##  1     3.20    -2.20   -22.2     -12.2     2.80
##  2    -2.80    -1.20    10.8      15.8     4.8 
##  3    -2.80    19.8    -10.2     -13.2   -18.2 
##  4     9.2     10.8     12.8      11.8    -6.2 
##  5    -1.80    -4.20    -6.2      16.8    18.8 
##  6   -13.8    -14.2     26.8     -24.2    16.8 
##  7    17.2      7.8      2.80     -6.2    -4.20
##  8     3.20    -7.2     12.8     -26.2     9.8 
##  9   -18.8      8.8    -14.2      16.8    -9.2 
## 10     7.2    -18.2    -13.2      20.8   -15.2
```

2. 如果不想命名函数，则可以使用匿名函数。


```r
# 形式1：常规写法
exams %>% 
  map_df(function(x) x - mean(x))
```

```
## # A tibble: 10 x 5
##    student1 student2 student3 student4 student5
##       <dbl>    <dbl>    <dbl>    <dbl>    <dbl>
##  1     3.20    -2.20   -22.2     -12.2     2.80
##  2    -2.80    -1.20    10.8      15.8     4.8 
##  3    -2.80    19.8    -10.2     -13.2   -18.2 
##  4     9.2     10.8     12.8      11.8    -6.2 
##  5    -1.80    -4.20    -6.2      16.8    18.8 
##  6   -13.8    -14.2     26.8     -24.2    16.8 
##  7    17.2      7.8      2.80     -6.2    -4.20
##  8     3.20    -7.2     12.8     -26.2     9.8 
##  9   -18.8      8.8    -14.2      16.8    -9.2 
## 10     7.2    -18.2    -13.2      20.8   -15.2
```

```r
# 形式2：简便写法，使用符号~代替function()
exams %>% 
  map_df(~ .x - mean(.x))
```

```
## # A tibble: 10 x 5
##    student1 student2 student3 student4 student5
##       <dbl>    <dbl>    <dbl>    <dbl>    <dbl>
##  1     3.20    -2.20   -22.2     -12.2     2.80
##  2    -2.80    -1.20    10.8      15.8     4.8 
##  3    -2.80    19.8    -10.2     -13.2   -18.2 
##  4     9.2     10.8     12.8      11.8    -6.2 
##  5    -1.80    -4.20    -6.2      16.8    18.8 
##  6   -13.8    -14.2     26.8     -24.2    16.8 
##  7    17.2      7.8      2.80     -6.2    -4.20
##  8     3.20    -7.2     12.8     -26.2     9.8 
##  9   -18.8      8.8    -14.2      16.8    -9.2 
## 10     7.2    -18.2    -13.2      20.8   -15.2
```

:::: {.rmdnote data-latex="{注意}"}
上面的例子中，符号`~`告诉map()后面的函数是一个匿名函数，`.x`对应函数的参数，是一个占位符。例如，以下代码是找出每位学生有多少成绩是在80以上的。


```r
exams %>% 
  map_int(function(x){
    length(x[x>80])  # 这里的x指代exams中的student1~student5
  })
```

```
## student1 student2 student3 student4 student5 
##        2        2        4        5        4
```

以上代码可以简写为：


```r
exams %>% 
  map_int(~ length(.x[.x>80]))
```

```
## student1 student2 student3 student4 student5 
##        2        2        4        5        4
```

虽然之前已经说过，但还是需要强调，为了便于区分，建议单独使用`.`符号时代替数据，而使用`.x`用于`purrr`中的匿名函数。
::::
