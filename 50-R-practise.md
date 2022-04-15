---
output:
  html_document: default
  pdf_document: default
---

# 练习 {#sec:R-practise}



## 1-旋转数据框


```r
d <- tibble::tribble(
  ~name, ~chinese, ~math, ~physics, ~english, ~music, ~sport,
  "Alice", 88L, 63L, 98L, 89L, 85L, 72L,
  "Bob", 85L, 75L, 85L, 82L, 73L, 83L,
  "Carlo", 95L, 98L, 75L, 75L, 68L, 84L
)
d
```

```
## # A tibble: 3 x 7
##   name  chinese  math physics english music sport
##   <chr>   <int> <int>   <int>   <int> <int> <int>
## 1 Alice      88    63      98      89    85    72
## 2 Bob        85    75      85      82    73    83
## 3 Carlo      95    98      75      75    68    84
```

```r
d %>% 
  pivot_longer(
    col = -name,
    names_to = "subject",
    values_to = "score"
  ) %>% 
  pivot_wider(
    names_from = name,
    values_from = score
  )
```

```
## # A tibble: 6 x 4
##   subject Alice   Bob Carlo
##   <chr>   <int> <int> <int>
## 1 chinese    88    85    95
## 2 math       63    75    98
## 3 physics    98    85    75
## 4 english    89    82    75
## 5 music      85    73    68
## 6 sport      72    83    84
```

## 2-排序


```r
d <- 
  tibble::tribble(
  ~name, ~score,
   "a1",     2,
   "a2",     5,
   "a3",     3,
   "a4",     7,
   "a5",     6,
  "all",    23
  )
d
```

```
## # A tibble: 6 x 2
##   name  score
##   <chr> <dbl>
## 1 a1        2
## 2 a2        5
## 3 a3        3
## 4 a4        7
## 5 a5        6
## 6 all      23
```

```r
d %>% 
  arrange(desc(score)) %>% 
  arrange(name %in% c("all"))
```

```
## # A tibble: 6 x 2
##   name  score
##   <chr> <dbl>
## 1 a4        7
## 2 a5        6
## 3 a2        5
## 4 a3        3
## 5 a1        2
## 6 all      23
```

## 3-统计并筛选

统计每位同学，成绩高于各科均值的个数。


```r
d <- tibble::tribble(
  ~name, ~chinese, ~engish, ~physics, ~sport, ~music,
  "Aice", 85, 56, 56, 54, 78,
  "Bob", 75, 78, 77, 56, 69,
  "Cake", 69, 41, 88, 89, 59,
  "Dave", 90, 66, 74, 82, 60,
  "Eve", 68, 85, 75, 69, 21,
  "Fod", 77, 74, 62, 74, 88,
  "Gimme", 56, 88, 75, 69, 34
)
d
```

```
## # A tibble: 7 x 6
##   name  chinese engish physics sport music
##   <chr>   <dbl>  <dbl>   <dbl> <dbl> <dbl>
## 1 Aice       85     56      56    54    78
## 2 Bob        75     78      77    56    69
## 3 Cake       69     41      88    89    59
## 4 Dave       90     66      74    82    60
## 5 Eve        68     85      75    69    21
## 6 Fod        77     74      62    74    88
## 7 Gimme      56     88      75    69    34
```


```r
d %>% 
  mutate(
    across(-name, list(RC = ~ . > mean(.)))
  ) %>% 
  rowwise() %>% 
  mutate(
    num_above_mean = sum(c_across(ends_with("_RC")))
  ) %>% 
  ungroup() %>% 
  select(-ends_with("_RC"))
```

```
## # A tibble: 7 x 7
##   name  chinese engish physics sport music num_above_mean
##   <chr>   <dbl>  <dbl>   <dbl> <dbl> <dbl>          <int>
## 1 Aice       85     56      56    54    78              2
## 2 Bob        75     78      77    56    69              4
## 3 Cake       69     41      88    89    59              3
## 4 Dave       90     66      74    82    60              4
## 5 Eve        68     85      75    69    21              2
## 6 Fod        77     74      62    74    88              4
## 7 Gimme      56     88      75    69    34              2
```

## 4-按条件分组


```r
data <- tribble(
  ~id, ~corr, ~period,
  1, 0, "a",
  1, 0, "b",
  2, 0, "a",
  2, 1, "b",
  3, 1, "a",
  3, 0, "b",
  4, 1, "a",
  4, 1, "b"
)
data
```

```
## # A tibble: 8 x 3
##      id  corr period
##   <dbl> <dbl> <chr> 
## 1     1     0 a     
## 2     1     0 b     
## 3     2     0 a     
## 4     2     1 b     
## 5     3     1 a     
## 6     3     0 b     
## 7     4     1 a     
## 8     4     1 b
```

按id分组，

- 如果corr中都是0 就”none”

- 如果corr中都是1 就”both”

- 如果corr中只有一个1，就输出1对应period


```r
my_function <- function(corr, period){
  sum <- sum(corr)
  
  if (sum == 0){
    res <- "none"
  }
  
  if (sum == 2){
    res <- "both"
  }
  
  if (sum == 1){
    res <- period[corr == 1]
  }
  return(res)
}

data %>% 
  group_by(id) %>% 
  summarise(resp_period = my_function(corr, period))
```

```
## # A tibble: 4 x 2
##      id resp_period
##   <dbl> <chr>      
## 1     1 none       
## 2     2 b          
## 3     3 a          
## 4     4 both
```

## 
