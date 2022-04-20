---
output:
  html_document: default
  pdf_document: default
---

# 数据连接 {#tidyverse-join}



## 合并行/列

合并数据框最基本的思路：

- 合并行：下方堆叠新行，根据列名匹配。此方法需要列名相同，不同的列名将作为新列（适用NA填充）。使用`dplyr`包中的`bind_rows()`函数。

- 合并列：在右侧拼接新列，根据位置匹配行。此方法要求行数必须相同。使用`dplyr`包中的`bind_cols()`函数。


```r
# 合并行
bind_rows(
  sample_n(iris, 2),
  sample_n(iris, 2),
  sample_n(iris, 2)
)
```

```
##   Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
## 1          5.6         2.9          3.6         1.3 versicolor
## 2          6.7         3.0          5.2         2.3  virginica
## 3          6.3         3.4          5.6         2.4  virginica
## 4          4.6         3.4          1.4         0.3     setosa
## 5          6.1         3.0          4.9         1.8  virginica
## 6          5.2         2.7          3.9         1.4 versicolor
```

```r
# 合并列
one <- mtcars[1:4, 1:3]
two <- mtcars[1:4, 4:5]
bind_cols(one, two)
```

```
##                 mpg cyl disp  hp drat
## Mazda RX4      21.0   6  160 110 3.90
## Mazda RX4 Wag  21.0   6  160 110 3.90
## Datsun 710     22.8   4  108  93 3.85
## Hornet 4 Drive 21.4   6  258 110 3.08
```

::: {.rmdtip data-latex="{提示}"}
使用`map_dfr()`和`map_dfc()`函数可以在批量读入数据的同时做合并行/列的操作。
:::


## 多个数据表连接

使用`purrr`包中的`reduce()`函数。


```r
# 多个文件
files <- list.files("datas/new/achieves", pattern = "xlsx", full.names = TRUE)
map(files, read_xlsx) %>% 
  reduce(full_join, by = "人名")
```

```
## # A tibble: 7 x 4
##   人名  `4月业绩` `3月业绩` `5月业绩`
##   <chr>     <dbl>     <dbl>     <dbl>
## 1 小红         70        NA        90
## 2 小白         60        NA        NA
## 3 小张         50        90        NA
## 4 小王         40        NA        NA
## 5 小明         NA        80        NA
## 6 小李         NA        85        80
## 7 小花         NA        NA       100
```

```r
# 一个文件多个sheet
path <- "datas/new/Mar-May-performance.xlsx"
map(excel_sheets(path), ~ read_xlsx(path, .x)) %>% 
  reduce(full_join, by = "人名")
```

```
## # A tibble: 7 x 4
##   人名  `3月业绩` `4月业绩` `5月业绩`
##   <chr>     <dbl>     <dbl>     <dbl>
## 1 小明         80        NA        NA
## 2 小李         85        NA        80
## 3 小张         90        50        NA
## 4 小红         NA        70        90
## 5 小白         NA        60        NA
## 6 小王         NA        40        NA
## 7 小花         NA        NA       100
```

## 集合运算

集合运算是针对所有行进行的运算，通过对每一行变量进行对比，实现不同条件的返回值。本方法需要所比较的两个表具有相同的变量，具体用法如下：


```r
intersect(x, y)  # 返回x和y共同包含的观测
union(x, y)  # 返回x和y中所有的（唯一）元素
setdiff(x, y)  # 返回在x中但不在y中的观测
```
