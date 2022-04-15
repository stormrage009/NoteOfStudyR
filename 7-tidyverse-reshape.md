---
output:
  html_document: default
  pdf_document: default
---

# 数据重塑 {#tidyverse-reshape}



## 什么是整洁的数据

根据Hadley的理念，不整洁的数据具有以下特点：

- 首行（列名）是值，不是变量名

- 多个变量放在一行

- 变量既放在行也放在列

- 多种类型的观测单元在同一单元格

- 一个观测单元放在多个表

对应的，整洁的数据具有如下特点：

- 每个**变量**构成一列

- 每个**观测**构成一行

- 每个观测的每个变量**值**构成一个单元格


下表是一个不整洁数据的例子，1. observation列中有两个变量数据；2. 列名中的A/B应是分类变量species的两个水平值；3. 测量值列count和dbh应各占一行。


```r
dt <- tribble(
  ~observation, ~A_count, ~B_count, ~A_dbh, ~B_dbh,
  "Richmond(Sam)", 7,        2,       100,    110 , 
  "Windsor(Ash)" , 10,       5,        80,     87 ,
  "Bilpin(Jules)", 5,        8,        95,     90 ,
)
dt
```

```
## # A tibble: 3 x 5
##   observation   A_count B_count A_dbh B_dbh
##   <chr>           <dbl>   <dbl> <dbl> <dbl>
## 1 Richmond(Sam)       7       2   100   110
## 2 Windsor(Ash)       10       5    80    87
## 3 Bilpin(Jules)       5       8    95    90
```

如何对其进行整洁化处理呢，我们需要借助`dplyr`中的两个强大的函数，`pivot_longer()`和`pivot_wider()`。


```r
tidy_dt <- dt %>% 
  pivot_longer(
    cols = -observation,
    names_to = c("species", ".value"),   # .value对应列名中相同的部分。
    names_sep = "_"
  ) %>% 
  separate(observation, into = c("site", "surveyor"))  # 将第一列分开
```

```
## Warning: Expected 2 pieces. Additional pieces discarded in 6 rows [1,
## 2, 3, 4, 5, 6].
```

```r
tidy_dt
```

```
## # A tibble: 6 x 5
##   site     surveyor species count   dbh
##   <chr>    <chr>    <chr>   <dbl> <dbl>
## 1 Richmond Sam      A           7   100
## 2 Richmond Sam      B           2   110
## 3 Windsor  Ash      A          10    80
## 4 Windsor  Ash      B           5    87
## 5 Bilpin   Jules    A           5    95
## 6 Bilpin   Jules    B           8    90
```

:::: {.rmdnote data-latex="{注意}"}
数据的变换中，最重要的是区分**变量、观测和值**

1. 宽表变长表：`pivot_longer()`：表比较“宽”，本来应该是值的，却出现在变量名中，此时需要将“值”变换到值中，并新起一个列名（新存为一列）。如上例中，A/B应为specis的值。在看一个例子：


```r
load("datas/new/family.rda")
family
```

```
## # A tibble: 5 x 5
##   family dob_child1 dob_child2 gender_child1 gender_child2
##    <int> <date>     <date>             <int>         <int>
## 1      1 1998-11-26 2000-01-29             1             2
## 2      2 1996-06-22 NA                     2            NA
## 3      3 2002-07-11 2004-04-05             2             2
## 4      4 2004-10-10 2009-08-27             1             1
## 5      5 2000-12-05 2005-02-28             2             1
```

```r
tidy_family <- family %>% 
  pivot_longer(
    cols = -family,
    names_to = c(".value", "child"),
    names_sep = "_",
    values_drop_na = TRUE
  )
tidy_family
```

```
## # A tibble: 9 x 4
##   family child  dob        gender
##    <int> <chr>  <date>      <int>
## 1      1 child1 1998-11-26      1
## 2      1 child2 2000-01-29      2
## 3      2 child1 1996-06-22      2
## 4      3 child1 2002-07-11      2
## 5      3 child2 2004-04-05      2
## 6      4 child1 2004-10-10      1
## 7      4 child2 2009-08-27      1
## 8      5 child1 2000-12-05      2
## 9      5 child2 2005-02-28      1
```

2. 长表变宽表：有时候需要将分类变量的若干水平值，变成变量名（列名）。看一个例子：


```r
# 例1
us_rent_income %>% 
  pivot_wider(
    names_from = variable, 
    values_from = c(estimate, moe)
    )
```

```
## # A tibble: 52 x 6
##    GEOID NAME       estimate_income estimate_rent moe_income moe_rent
##    <chr> <chr>                <dbl>         <dbl>      <dbl>    <dbl>
##  1 01    Alabama              24476           747        136        3
##  2 02    Alaska               32940          1200        508       13
##  3 04    Arizona              27517           972        148        4
##  4 05    Arkansas             23789           709        165        5
##  5 06    California           29454          1358        109        3
##  6 08    Colorado             32401          1125        109        5
##  7 09    Connectic~           35326          1123        195        5
##  8 10    Delaware             31560          1076        247       10
##  9 11    District ~           43198          1424        681       17
## 10 12    Florida              25952          1077         70        3
## # ... with 42 more rows
```

```r
# 例2
contacts <- tribble( ~field, ~value,
                    " 姓名", " 张三",
                    " 公司", " 百度",
                    " 姓名", " 李四",
                    " 公司", " 腾讯",
                    "Email", "Lisi@163.com",
                    " 姓名", " 王五")

contacts <- contacts %>% 
  mutate(ID = cumsum(field == " 姓名"))
contacts
```

```
## # A tibble: 6 x 3
##   field   value             ID
##   <chr>   <chr>          <int>
## 1 " 姓名" " 张三"            1
## 2 " 公司" " 百度"            1
## 3 " 姓名" " 李四"            2
## 4 " 公司" " 腾讯"            2
## 5 "Email" "Lisi@163.com"     2
## 6 " 姓名" " 王五"            3
```

```r
contacts_wider <- contacts %>% 
  pivot_wider(
    names_from = field,
    values_from = value
  ) %>% 
  select(-ID)
contacts_wider
```

```
## # A tibble: 3 x 3
##   ` 姓名` ` 公司` Email       
##   <chr>   <chr>   <chr>       
## 1 " 张三" " 百度" <NA>        
## 2 " 李四" " 腾讯" Lisi@163.com
## 3 " 王五"  <NA>   <NA>
```
::::

## 综合实例


```r
world_bank_pop
```

```
## # A tibble: 1,056 x 20
##    country indicator      `2000` `2001` `2002` `2003`  `2004`  `2005`
##    <chr>   <chr>           <dbl>  <dbl>  <dbl>  <dbl>   <dbl>   <dbl>
##  1 ABW     SP.URB.TOTL    4.24e4 4.30e4 4.37e4 4.42e4 4.47e+4 4.49e+4
##  2 ABW     SP.URB.GROW    1.18e0 1.41e0 1.43e0 1.31e0 9.51e-1 4.91e-1
##  3 ABW     SP.POP.TOTL    9.09e4 9.29e4 9.50e4 9.70e4 9.87e+4 1.00e+5
##  4 ABW     SP.POP.GROW    2.06e0 2.23e0 2.23e0 2.11e0 1.76e+0 1.30e+0
##  5 AFG     SP.URB.TOTL    4.44e6 4.65e6 4.89e6 5.16e6 5.43e+6 5.69e+6
##  6 AFG     SP.URB.GROW    3.91e0 4.66e0 5.13e0 5.23e0 5.12e+0 4.77e+0
##  7 AFG     SP.POP.TOTL    2.01e7 2.10e7 2.20e7 2.31e7 2.41e+7 2.51e+7
##  8 AFG     SP.POP.GROW    3.49e0 4.25e0 4.72e0 4.82e0 4.47e+0 3.87e+0
##  9 AGO     SP.URB.TOTL    8.23e6 8.71e6 9.22e6 9.77e6 1.03e+7 1.09e+7
## 10 AGO     SP.URB.GROW    5.44e0 5.59e0 5.70e0 5.76e0 5.75e+0 5.69e+0
## # ... with 1,046 more rows, and 12 more variables: `2006` <dbl>,
## #   `2007` <dbl>, `2008` <dbl>, `2009` <dbl>, `2010` <dbl>,
## #   `2011` <dbl>, `2012` <dbl>, `2013` <dbl>, `2014` <dbl>,
## #   `2015` <dbl>, `2016` <dbl>, `2017` <dbl>
```

观察world_bank_pop数据可以发现：

- 年份跨越多个列，其应该是”值“。

- indicator列中:

   - SP.POP.GROW为人口增长，SP.POP.TOTL为总人口。
   - SP.URB.GROW为城市人口增长，SP.URB.TOTL为城市总人口。
   - 这一列应为两个变量：地区（POP和URB）及变量（GROW和TOTL）。

- 根据以上思路对数据进行规整。


```r
# 将年份放入值中作为新的一列
tidy_world_bank_pop <- world_bank_pop %>% 
  pivot_longer(
    col = -c(country, indicator),
    names_to = "year",
    values_to = "value"
  )

# 对indicator列进行处理-SP字符是共用的，可以省去
tidy_world_bank_pop <- tidy_world_bank_pop %>% 
  separate(indicator, 
           into = c(NA, "area", "variable"))
# variable列包含两个分类变量，
# 需要将分类变量variable的水平值变为列名，并将value列的值对应上去
tidy_world_bank_pop <- tidy_world_bank_pop %>% 
  pivot_wider(
    names_from = variable,
    values_from = value
  )
tidy_world_bank_pop
```

```
## # A tibble: 9,504 x 5
##    country area  year   TOTL    GROW
##    <chr>   <chr> <chr> <dbl>   <dbl>
##  1 ABW     URB   2000  42444  1.18  
##  2 ABW     URB   2001  43048  1.41  
##  3 ABW     URB   2002  43670  1.43  
##  4 ABW     URB   2003  44246  1.31  
##  5 ABW     URB   2004  44669  0.951 
##  6 ABW     URB   2005  44889  0.491 
##  7 ABW     URB   2006  44881 -0.0178
##  8 ABW     URB   2007  44686 -0.435 
##  9 ABW     URB   2008  44375 -0.698 
## 10 ABW     URB   2009  44052 -0.731 
## # ... with 9,494 more rows
```

最终完成规整后的数据：

- 每一列代表一个变量。

- 每一行为一个观测。

- 每一个值构成一个单元格。

## 方形化

[将一个深度嵌套的列表（通常来自JSON和XML）规整为一个整齐的行列数据集。目前暂时不会接触此类型的数据，所以暂不学习。]{.todo}
