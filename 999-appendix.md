# (APPENDIX) 附录 {#appendix .unnumbered} 

# 附录-有趣的“懒人”包

## 自动生成正则表达式


```r
# remotes::install_github("daranzolin/inferregex")
library(inferregex)

s <- "abcd-9999-ab9"
infer_regex(s)[1,2]
```

```
## [1] "^[a-z]{4}-\\d{4}-[a-z]{2}\\d$"
```

## [janitor包](https://github.com/sfirke/janitor)

janitor has simple little tools for examining and cleaning dirty data.

### 整理列名

```r
library(tidyverse)
```

```
## -- Attaching packages ---------------------------- tidyverse 1.3.1 --
```

```
## v ggplot2 3.3.5     v purrr   0.3.4
## v tibble  3.1.6     v dplyr   1.0.8
## v tidyr   1.2.0     v stringr 1.4.0
## v readr   2.1.2     v forcats 0.5.1
```

```
## -- Conflicts ------------------------------- tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(janitor)
```

```
## 
## 载入程辑包：'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

```r
fake_raw <- tibble::tribble(
  ~id, ~`count/num`, ~W.t, ~Case, ~`time--d`, ~`%percent`,
  1L, "china", 3L, "w", 5L, 25L,
  2L, "us", 4L, "f", 6L, 34L,
  3L, "india", 5L, "q", 8L, 78L
)
fake_raw
```

```
## # A tibble: 3 x 6
##      id `count/num`   W.t Case  `time--d` `%percent`
##   <int> <chr>       <int> <chr>     <int>      <int>
## 1     1 china           3 w             5         25
## 2     2 us              4 f             6         34
## 3     3 india           5 q             8         78
```

```r
fake_raw %>%
  janitor::clean_names()
```

```
## Warning in FUN(X[[i]], ...): strings not representable in native
## encoding will be translated to UTF-8
```

```
## Warning in FUN(X[[i]], ...): unable to translate '<U+00C4>' to native
## encoding
```

```
## Warning in FUN(X[[i]], ...): unable to translate '<U+00D6>' to native
## encoding
```

```
## Warning in FUN(X[[i]], ...): unable to translate '<U+00E4>' to native
## encoding
```

```
## Warning in FUN(X[[i]], ...): unable to translate '<U+00F6>' to native
## encoding
```

```
## Warning in FUN(X[[i]], ...): unable to translate '<U+00DF>' to native
## encoding
```

```
## Warning in FUN(X[[i]], ...): unable to translate '<U+00C6>' to native
## encoding
```

```
## Warning in FUN(X[[i]], ...): unable to translate '<U+00E6>' to native
## encoding
```

```
## Warning in FUN(X[[i]], ...): unable to translate '<U+00D8>' to native
## encoding
```

```
## Warning in FUN(X[[i]], ...): unable to translate '<U+00F8>' to native
## encoding
```

```
## Warning in FUN(X[[i]], ...): unable to translate '<U+00C5>' to native
## encoding
```

```
## Warning in FUN(X[[i]], ...): unable to translate '<U+00E5>' to native
## encoding
```

```
## # A tibble: 3 x 6
##      id count_num   w_t case  time_d percent_percent
##   <int> <chr>     <int> <chr>  <int>           <int>
## 1     1 china         3 w          5              25
## 2     2 us            4 f          6              34
## 3     3 india         5 q          8              78
```

### 统计分析


```r
mtcars %>%
  janitor::tabyl(cyl)
```

```
##  cyl  n percent
##    4 11 0.34375
##    6  7 0.21875
##    8 14 0.43750
```

### 查看指定列有重复的行


```r
df <- tribble(
  ~id, ~date, ~store_id, ~sales,
  1, "2020-03-01", 1, 100,
  2, "2020-03-01", 2, 100,
  3, "2020-03-01", 3, 150,
  4, "2020-03-02", 1, 110,
  5, "2020-03-02", 3, 101
)
df
```

```
## # A tibble: 5 x 4
##      id date       store_id sales
##   <dbl> <chr>         <dbl> <dbl>
## 1     1 2020-03-01        1   100
## 2     2 2020-03-01        2   100
## 3     3 2020-03-01        3   150
## 4     4 2020-03-02        1   110
## 5     5 2020-03-02        3   101
```

```r
df %>%
  janitor::get_dupes(store_id)
```

```
## # A tibble: 4 x 5
##   store_id dupe_count    id date       sales
##      <dbl>      <int> <dbl> <chr>      <dbl>
## 1        1          2     1 2020-03-01   100
## 2        1          2     4 2020-03-02   110
## 3        3          2     3 2020-03-01   150
## 4        3          2     5 2020-03-02   101
```

## [自动生成模型公式](https://github.com/datalorax/equatiomatic)


```r
# remotes::install_github("datalorax/equatiomatic")
library(equatiomatic)

mod1 <- lm(mpg ~ cyl + disp, mtcars)
extract_eq(mod1)
```

$$
\operatorname{mpg} = \alpha + \beta_{1}(\operatorname{cyl}) + \beta_{2}(\operatorname{disp}) + \epsilon
$$

$$
\operatorname{mpg} = \alpha + \beta_{1}(\operatorname{cyl}) + \beta_{2}(\operatorname{disp}) + \epsilon
$$

## [自动生成论文报告](https://github.com/easystats/report)


```r
library(report)
model <- lm(Sepal.Length ~ Species, data = iris)
report(model)
```

```
## Registered S3 method overwritten by 'parameters':
##   method                         from      
##   format.parameters_distribution datawizard
```

```
## We fitted a linear model (estimated using OLS) to predict Sepal.Length with Species (formula: Sepal.Length ~ Species). The model explains a statistically significant and substantial proportion of variance (R2 = 0.62, F(2, 147) = 119.26, p < .001, adj. R2 = 0.61). The model's intercept, corresponding to Species = setosa, is at 5.01 (95% CI [4.86, 5.15], t(147) = 68.76, p < .001). Within this model:
## 
##   - The effect of Species [versicolor] is statistically significant and positive (beta = 0.93, 95% CI [0.73, 1.13], t(147) = 9.03, p < .001; Std. beta = 1.12, 95% CI [0.88, 1.37])
##   - The effect of Species [virginica] is statistically significant and positive (beta = 1.58, 95% CI [1.38, 1.79], t(147) = 15.37, p < .001; Std. beta = 1.91, 95% CI [1.66, 2.16])
## 
## Standardized parameters were obtained by fitting the model on a standardized version of the dataset. 95% Confidence Intervals (CIs) and p-values were computed using the Wald approximation.
```

## [自动评估模型](https://easystats.github.io/performance/)


```r
library(performance)

# defining a model
model <- lm(mpg ~ wt + am + gear + vs * cyl, data = mtcars)

# checking model assumptions 
check_model(model)  # 注意生成图片时的窗口不能过小
```

<img src="999-appendix_files/figure-html/unnamed-chunk-7-1.png" width="672" />

## [自动生成统计表格](https://github.com/ddsjoberg/gtsummary)


```r
library(gtsummary)
```

```
## #BlackLivesMatter
```

```r
# 生成的表格可以直接复制进论文即可
gtsummary::trial %>% 
  dplyr::select(trt, age, grade, response) %>% 
  gtsummary::tbl_summary(
    by = trt,
    missing = "no"
  ) %>% 
  gtsummary::add_p() %>% 
  gtsummary::add_overall() %>% 
  gtsummary::add_n() %>% 
  gtsummary::bold_labels()
```

```{=html}
<div id="xnrdwuctcq" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#xnrdwuctcq .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#xnrdwuctcq .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#xnrdwuctcq .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#xnrdwuctcq .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#xnrdwuctcq .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#xnrdwuctcq .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#xnrdwuctcq .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#xnrdwuctcq .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#xnrdwuctcq .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#xnrdwuctcq .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#xnrdwuctcq .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#xnrdwuctcq .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
}

#xnrdwuctcq .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#xnrdwuctcq .gt_from_md > :first-child {
  margin-top: 0;
}

#xnrdwuctcq .gt_from_md > :last-child {
  margin-bottom: 0;
}

#xnrdwuctcq .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#xnrdwuctcq .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#xnrdwuctcq .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#xnrdwuctcq .gt_row_group_first td {
  border-top-width: 2px;
}

#xnrdwuctcq .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#xnrdwuctcq .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#xnrdwuctcq .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#xnrdwuctcq .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#xnrdwuctcq .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#xnrdwuctcq .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#xnrdwuctcq .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#xnrdwuctcq .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#xnrdwuctcq .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#xnrdwuctcq .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-left: 4px;
  padding-right: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#xnrdwuctcq .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#xnrdwuctcq .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#xnrdwuctcq .gt_left {
  text-align: left;
}

#xnrdwuctcq .gt_center {
  text-align: center;
}

#xnrdwuctcq .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#xnrdwuctcq .gt_font_normal {
  font-weight: normal;
}

#xnrdwuctcq .gt_font_bold {
  font-weight: bold;
}

#xnrdwuctcq .gt_font_italic {
  font-style: italic;
}

#xnrdwuctcq .gt_super {
  font-size: 65%;
}

#xnrdwuctcq .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 75%;
  vertical-align: 0.4em;
}

#xnrdwuctcq .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#xnrdwuctcq .gt_slash_mark {
  font-size: 0.7em;
  line-height: 0.7em;
  vertical-align: 0.15em;
}

#xnrdwuctcq .gt_fraction_numerator {
  font-size: 0.6em;
  line-height: 0.6em;
  vertical-align: 0.45em;
}

#xnrdwuctcq .gt_fraction_denominator {
  font-size: 0.6em;
  line-height: 0.6em;
  vertical-align: -0.05em;
}
</style>
<table class="gt_table">
  
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1"><strong>Characteristic</strong></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>N</strong></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>Overall</strong>, N = 200<sup class="gt_footnote_marks">1</sup></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>Drug A</strong>, N = 98<sup class="gt_footnote_marks">1</sup></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>Drug B</strong>, N = 102<sup class="gt_footnote_marks">1</sup></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>p-value</strong><sup class="gt_footnote_marks">2</sup></th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td class="gt_row gt_left" style="font-weight: bold;">Age</td>
<td class="gt_row gt_center">189</td>
<td class="gt_row gt_center">47 (38, 57)</td>
<td class="gt_row gt_center">46 (37, 59)</td>
<td class="gt_row gt_center">48 (39, 56)</td>
<td class="gt_row gt_center">0.7</td></tr>
    <tr><td class="gt_row gt_left" style="font-weight: bold;">Grade</td>
<td class="gt_row gt_center">200</td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center">0.9</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">I</td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center">68 (34%)</td>
<td class="gt_row gt_center">35 (36%)</td>
<td class="gt_row gt_center">33 (32%)</td>
<td class="gt_row gt_center"></td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">II</td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center">68 (34%)</td>
<td class="gt_row gt_center">32 (33%)</td>
<td class="gt_row gt_center">36 (35%)</td>
<td class="gt_row gt_center"></td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">III</td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center">64 (32%)</td>
<td class="gt_row gt_center">31 (32%)</td>
<td class="gt_row gt_center">33 (32%)</td>
<td class="gt_row gt_center"></td></tr>
    <tr><td class="gt_row gt_left" style="font-weight: bold;">Tumor Response</td>
<td class="gt_row gt_center">193</td>
<td class="gt_row gt_center">61 (32%)</td>
<td class="gt_row gt_center">28 (29%)</td>
<td class="gt_row gt_center">33 (34%)</td>
<td class="gt_row gt_center">0.5</td></tr>
  </tbody>
  
  <tfoot class="gt_footnotes">
    <tr>
      <td class="gt_footnote" colspan="6"><sup class="gt_footnote_marks">1</sup> Median (IQR); n (%)</td>
    </tr>
    <tr>
      <td class="gt_footnote" colspan="6"><sup class="gt_footnote_marks">2</sup> Wilcoxon rank sum test; Pearson's Chi-squared test</td>
    </tr>
  </tfoot>
</table>
</div>
```


```r
# 生成的表格可以直接复制进论文即可
t1 <-
  glm(response ~ trt + age + grade, trial, family = binomial) %>%
  gtsummary::tbl_regression(exponentiate = TRUE)

t2 <-
  survival::coxph(survival::Surv(ttdeath, death) ~ trt + grade + age, trial) %>%
  gtsummary::tbl_regression(exponentiate = TRUE)



gtsummary::tbl_merge(
  tbls = list(t1, t2),
  tab_spanner = c("**Tumor Response**", "**Time to Death**")
)
```

```{=html}
<div id="ufezlsehrh" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#ufezlsehrh .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#ufezlsehrh .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#ufezlsehrh .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#ufezlsehrh .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#ufezlsehrh .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#ufezlsehrh .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#ufezlsehrh .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#ufezlsehrh .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#ufezlsehrh .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#ufezlsehrh .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#ufezlsehrh .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#ufezlsehrh .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
}

#ufezlsehrh .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#ufezlsehrh .gt_from_md > :first-child {
  margin-top: 0;
}

#ufezlsehrh .gt_from_md > :last-child {
  margin-bottom: 0;
}

#ufezlsehrh .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#ufezlsehrh .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#ufezlsehrh .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#ufezlsehrh .gt_row_group_first td {
  border-top-width: 2px;
}

#ufezlsehrh .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#ufezlsehrh .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#ufezlsehrh .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#ufezlsehrh .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#ufezlsehrh .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#ufezlsehrh .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#ufezlsehrh .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#ufezlsehrh .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#ufezlsehrh .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#ufezlsehrh .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-left: 4px;
  padding-right: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#ufezlsehrh .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#ufezlsehrh .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#ufezlsehrh .gt_left {
  text-align: left;
}

#ufezlsehrh .gt_center {
  text-align: center;
}

#ufezlsehrh .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#ufezlsehrh .gt_font_normal {
  font-weight: normal;
}

#ufezlsehrh .gt_font_bold {
  font-weight: bold;
}

#ufezlsehrh .gt_font_italic {
  font-style: italic;
}

#ufezlsehrh .gt_super {
  font-size: 65%;
}

#ufezlsehrh .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 75%;
  vertical-align: 0.4em;
}

#ufezlsehrh .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#ufezlsehrh .gt_slash_mark {
  font-size: 0.7em;
  line-height: 0.7em;
  vertical-align: 0.15em;
}

#ufezlsehrh .gt_fraction_numerator {
  font-size: 0.6em;
  line-height: 0.6em;
  vertical-align: 0.45em;
}

#ufezlsehrh .gt_fraction_denominator {
  font-size: 0.6em;
  line-height: 0.6em;
  vertical-align: -0.05em;
}
</style>
<table class="gt_table">
  
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="2" colspan="1"><strong>Characteristic</strong></th>
      <th class="gt_center gt_columns_top_border gt_column_spanner_outer" rowspan="1" colspan="3">
        <span class="gt_column_spanner"><strong>Tumor Response</strong></span>
      </th>
      <th class="gt_center gt_columns_top_border gt_column_spanner_outer" rowspan="1" colspan="3">
        <span class="gt_column_spanner"><strong>Time to Death</strong></span>
      </th>
    </tr>
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>OR</strong><sup class="gt_footnote_marks">1</sup></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>95% CI</strong><sup class="gt_footnote_marks">1</sup></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>p-value</strong></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>HR</strong><sup class="gt_footnote_marks">1</sup></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>95% CI</strong><sup class="gt_footnote_marks">1</sup></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>p-value</strong></th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td class="gt_row gt_left">Chemotherapy Treatment</td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">Drug A</td>
<td class="gt_row gt_center">—</td>
<td class="gt_row gt_center">—</td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center">—</td>
<td class="gt_row gt_center">—</td>
<td class="gt_row gt_center"></td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">Drug B</td>
<td class="gt_row gt_center">1.13</td>
<td class="gt_row gt_center">0.60, 2.13</td>
<td class="gt_row gt_center">0.7</td>
<td class="gt_row gt_center">1.30</td>
<td class="gt_row gt_center">0.88, 1.92</td>
<td class="gt_row gt_center">0.2</td></tr>
    <tr><td class="gt_row gt_left">Age</td>
<td class="gt_row gt_center">1.02</td>
<td class="gt_row gt_center">1.00, 1.04</td>
<td class="gt_row gt_center">0.10</td>
<td class="gt_row gt_center">1.01</td>
<td class="gt_row gt_center">0.99, 1.02</td>
<td class="gt_row gt_center">0.3</td></tr>
    <tr><td class="gt_row gt_left">Grade</td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">I</td>
<td class="gt_row gt_center">—</td>
<td class="gt_row gt_center">—</td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center">—</td>
<td class="gt_row gt_center">—</td>
<td class="gt_row gt_center"></td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">II</td>
<td class="gt_row gt_center">0.85</td>
<td class="gt_row gt_center">0.39, 1.85</td>
<td class="gt_row gt_center">0.7</td>
<td class="gt_row gt_center">1.21</td>
<td class="gt_row gt_center">0.73, 1.99</td>
<td class="gt_row gt_center">0.5</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">III</td>
<td class="gt_row gt_center">1.01</td>
<td class="gt_row gt_center">0.47, 2.15</td>
<td class="gt_row gt_center">>0.9</td>
<td class="gt_row gt_center">1.79</td>
<td class="gt_row gt_center">1.12, 2.86</td>
<td class="gt_row gt_center">0.014</td></tr>
  </tbody>
  
  <tfoot class="gt_footnotes">
    <tr>
      <td class="gt_footnote" colspan="7"><sup class="gt_footnote_marks">1</sup> OR = Odds Ratio, CI = Confidence Interval, HR = Hazard Ratio</td>
    </tr>
  </tfoot>
</table>
</div>
```

## [把统计结果写在图上](https://indrajeetpatil.github.io/statsExpressions/)


```r
library(statsExpressions)
library(ggplot2)

ggplot(mtcars, aes(x = mpg, y = wt)) +
  geom_point() +
  geom_smooth(method = "lm") +
  labs(
    title = "Spearman's rank correlation coefficient",
    subtitle = expr_corr_test(mtcars, mpg, wt, type = "nonparametric")
  )
```

## 画图配色

### 第三方主题-[ggthem](https://rdrr.io/github/cttobin/ggthemr/)


```r
# devtools::install_github('cttobin/ggthemr')
library(tidyverse)
library(ggthemr)
ggthemr("dust")

mtcars %>% 
  mutate(cyl = factor(cyl)) %>% 
  ggplot(aes(x = mpg, fill = cyl, color = cyl)) +
    geom_density(alpha = 0.75) +
    labs(fill = "Cylinders", color = "Cylinders",
         x = "MPG", y = "Density") +
    legend_top()
```

<img src="999-appendix_files/figure-html/unnamed-chunk-11-1.png" width="672" />

```r
ggthemr_reset()  # 重置为默认主题
```

### 画图颜色-[scales](https://github.com/r-lib/scales)


```r
library(scales)
```

```
## 
## 载入程辑包：'scales'
```

```
## The following object is masked from 'package:purrr':
## 
##     discard
```

```
## The following object is masked from 'package:readr':
## 
##     col_factor
```

```r
show_col(viridis_pal()(10))
```

<img src="999-appendix_files/figure-html/unnamed-chunk-12-1.png" width="672" />

推荐适用专业的配色网站 [colorbrewer](https://colorbrewer2.org/#type=sequential&scheme=BuGn&n=3)先看看颜色，再选择。

## 其它有用的包

1. 整理Rmarkdown


```r
# remotes::install_github("tjmahr/WrapRmd")
# remotes::install_github("fkeck/quickview")
# remotes::install_github("mwip/beautifyR")
```

