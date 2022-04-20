---
output:
  html_document: default
  pdf_document: default
---

# 正则表达式 {#sec:re}



正则表达是处理字符串的，它描述了一种字符串的蒲培模式，通常被用来检索、替换那些符合某个模式或规则的文本，例如电话号码、网络地址、邮箱地址和日期等。

## 使用正则表达式进行模式匹配

### 基础匹配

使用`str_view()`函数查看字符串是否匹配pattern。匹配到的模式会高亮显示。


```r
x <- c("apple", "banana", "pear")
str_view(string = x, pattern = "an")
```

```{=html}
<div id="htmlwidget-37e31dc4b178800a1aac" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-37e31dc4b178800a1aac">{"x":{"html":"<ul>\n  <li>apple<\/li>\n  <li>b<span class='match'>an<\/span>ana<\/li>\n  <li>pear<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
```

使用正则表达式，按以下条件匹配`stringr::words`单词库中的所有单词：

1. 以y开头的单词

2. 以x结尾的单词

3. 长度正好为3个字符的单词

4. 具有7个或更多字符的单词


```r
words <- stringr::words
str_view(words, "^y", match = TRUE)
```

```{=html}
<div id="htmlwidget-40913a347177f8196ce0" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-40913a347177f8196ce0">{"x":{"html":"<ul>\n  <li><span class='match'>y<\/span>ear<\/li>\n  <li><span class='match'>y<\/span>es<\/li>\n  <li><span class='match'>y<\/span>esterday<\/li>\n  <li><span class='match'>y<\/span>et<\/li>\n  <li><span class='match'>y<\/span>ou<\/li>\n  <li><span class='match'>y<\/span>oung<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
```

```r
str_view(words, "x$", match = TRUE)
```

```{=html}
<div id="htmlwidget-171dd97a8e6c264b23e2" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-171dd97a8e6c264b23e2">{"x":{"html":"<ul>\n  <li>bo<span class='match'>x<\/span><\/li>\n  <li>se<span class='match'>x<\/span><\/li>\n  <li>si<span class='match'>x<\/span><\/li>\n  <li>ta<span class='match'>x<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
```

```r
str_view(words, "^...$", match = TRUE)
```

```{=html}
<div id="htmlwidget-0bbf1360580094bdd6b6" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-0bbf1360580094bdd6b6">{"x":{"html":"<ul>\n  <li><span class='match'>act<\/span><\/li>\n  <li><span class='match'>add<\/span><\/li>\n  <li><span class='match'>age<\/span><\/li>\n  <li><span class='match'>ago<\/span><\/li>\n  <li><span class='match'>air<\/span><\/li>\n  <li><span class='match'>all<\/span><\/li>\n  <li><span class='match'>and<\/span><\/li>\n  <li><span class='match'>any<\/span><\/li>\n  <li><span class='match'>arm<\/span><\/li>\n  <li><span class='match'>art<\/span><\/li>\n  <li><span class='match'>ask<\/span><\/li>\n  <li><span class='match'>bad<\/span><\/li>\n  <li><span class='match'>bag<\/span><\/li>\n  <li><span class='match'>bar<\/span><\/li>\n  <li><span class='match'>bed<\/span><\/li>\n  <li><span class='match'>bet<\/span><\/li>\n  <li><span class='match'>big<\/span><\/li>\n  <li><span class='match'>bit<\/span><\/li>\n  <li><span class='match'>box<\/span><\/li>\n  <li><span class='match'>boy<\/span><\/li>\n  <li><span class='match'>bus<\/span><\/li>\n  <li><span class='match'>but<\/span><\/li>\n  <li><span class='match'>buy<\/span><\/li>\n  <li><span class='match'>can<\/span><\/li>\n  <li><span class='match'>car<\/span><\/li>\n  <li><span class='match'>cat<\/span><\/li>\n  <li><span class='match'>cup<\/span><\/li>\n  <li><span class='match'>cut<\/span><\/li>\n  <li><span class='match'>dad<\/span><\/li>\n  <li><span class='match'>day<\/span><\/li>\n  <li><span class='match'>die<\/span><\/li>\n  <li><span class='match'>dog<\/span><\/li>\n  <li><span class='match'>dry<\/span><\/li>\n  <li><span class='match'>due<\/span><\/li>\n  <li><span class='match'>eat<\/span><\/li>\n  <li><span class='match'>egg<\/span><\/li>\n  <li><span class='match'>end<\/span><\/li>\n  <li><span class='match'>eye<\/span><\/li>\n  <li><span class='match'>far<\/span><\/li>\n  <li><span class='match'>few<\/span><\/li>\n  <li><span class='match'>fit<\/span><\/li>\n  <li><span class='match'>fly<\/span><\/li>\n  <li><span class='match'>for<\/span><\/li>\n  <li><span class='match'>fun<\/span><\/li>\n  <li><span class='match'>gas<\/span><\/li>\n  <li><span class='match'>get<\/span><\/li>\n  <li><span class='match'>god<\/span><\/li>\n  <li><span class='match'>guy<\/span><\/li>\n  <li><span class='match'>hit<\/span><\/li>\n  <li><span class='match'>hot<\/span><\/li>\n  <li><span class='match'>how<\/span><\/li>\n  <li><span class='match'>job<\/span><\/li>\n  <li><span class='match'>key<\/span><\/li>\n  <li><span class='match'>kid<\/span><\/li>\n  <li><span class='match'>lad<\/span><\/li>\n  <li><span class='match'>law<\/span><\/li>\n  <li><span class='match'>lay<\/span><\/li>\n  <li><span class='match'>leg<\/span><\/li>\n  <li><span class='match'>let<\/span><\/li>\n  <li><span class='match'>lie<\/span><\/li>\n  <li><span class='match'>lot<\/span><\/li>\n  <li><span class='match'>low<\/span><\/li>\n  <li><span class='match'>man<\/span><\/li>\n  <li><span class='match'>may<\/span><\/li>\n  <li><span class='match'>mrs<\/span><\/li>\n  <li><span class='match'>new<\/span><\/li>\n  <li><span class='match'>non<\/span><\/li>\n  <li><span class='match'>not<\/span><\/li>\n  <li><span class='match'>now<\/span><\/li>\n  <li><span class='match'>odd<\/span><\/li>\n  <li><span class='match'>off<\/span><\/li>\n  <li><span class='match'>old<\/span><\/li>\n  <li><span class='match'>one<\/span><\/li>\n  <li><span class='match'>out<\/span><\/li>\n  <li><span class='match'>own<\/span><\/li>\n  <li><span class='match'>pay<\/span><\/li>\n  <li><span class='match'>per<\/span><\/li>\n  <li><span class='match'>put<\/span><\/li>\n  <li><span class='match'>red<\/span><\/li>\n  <li><span class='match'>rid<\/span><\/li>\n  <li><span class='match'>run<\/span><\/li>\n  <li><span class='match'>say<\/span><\/li>\n  <li><span class='match'>see<\/span><\/li>\n  <li><span class='match'>set<\/span><\/li>\n  <li><span class='match'>sex<\/span><\/li>\n  <li><span class='match'>she<\/span><\/li>\n  <li><span class='match'>sir<\/span><\/li>\n  <li><span class='match'>sit<\/span><\/li>\n  <li><span class='match'>six<\/span><\/li>\n  <li><span class='match'>son<\/span><\/li>\n  <li><span class='match'>sun<\/span><\/li>\n  <li><span class='match'>tax<\/span><\/li>\n  <li><span class='match'>tea<\/span><\/li>\n  <li><span class='match'>ten<\/span><\/li>\n  <li><span class='match'>the<\/span><\/li>\n  <li><span class='match'>tie<\/span><\/li>\n  <li><span class='match'>too<\/span><\/li>\n  <li><span class='match'>top<\/span><\/li>\n  <li><span class='match'>try<\/span><\/li>\n  <li><span class='match'>two<\/span><\/li>\n  <li><span class='match'>use<\/span><\/li>\n  <li><span class='match'>war<\/span><\/li>\n  <li><span class='match'>way<\/span><\/li>\n  <li><span class='match'>wee<\/span><\/li>\n  <li><span class='match'>who<\/span><\/li>\n  <li><span class='match'>why<\/span><\/li>\n  <li><span class='match'>win<\/span><\/li>\n  <li><span class='match'>yes<\/span><\/li>\n  <li><span class='match'>yet<\/span><\/li>\n  <li><span class='match'>you<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
```

```r
str_view(words, ".{7,}", match = TRUE)
```

```{=html}
<div id="htmlwidget-9b8e32ef18473f706a07" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-9b8e32ef18473f706a07">{"x":{"html":"<ul>\n  <li><span class='match'>absolute<\/span><\/li>\n  <li><span class='match'>account<\/span><\/li>\n  <li><span class='match'>achieve<\/span><\/li>\n  <li><span class='match'>address<\/span><\/li>\n  <li><span class='match'>advertise<\/span><\/li>\n  <li><span class='match'>afternoon<\/span><\/li>\n  <li><span class='match'>against<\/span><\/li>\n  <li><span class='match'>already<\/span><\/li>\n  <li><span class='match'>alright<\/span><\/li>\n  <li><span class='match'>although<\/span><\/li>\n  <li><span class='match'>america<\/span><\/li>\n  <li><span class='match'>another<\/span><\/li>\n  <li><span class='match'>apparent<\/span><\/li>\n  <li><span class='match'>appoint<\/span><\/li>\n  <li><span class='match'>approach<\/span><\/li>\n  <li><span class='match'>appropriate<\/span><\/li>\n  <li><span class='match'>arrange<\/span><\/li>\n  <li><span class='match'>associate<\/span><\/li>\n  <li><span class='match'>authority<\/span><\/li>\n  <li><span class='match'>available<\/span><\/li>\n  <li><span class='match'>balance<\/span><\/li>\n  <li><span class='match'>because<\/span><\/li>\n  <li><span class='match'>believe<\/span><\/li>\n  <li><span class='match'>benefit<\/span><\/li>\n  <li><span class='match'>between<\/span><\/li>\n  <li><span class='match'>brilliant<\/span><\/li>\n  <li><span class='match'>britain<\/span><\/li>\n  <li><span class='match'>brother<\/span><\/li>\n  <li><span class='match'>business<\/span><\/li>\n  <li><span class='match'>certain<\/span><\/li>\n  <li><span class='match'>chairman<\/span><\/li>\n  <li><span class='match'>character<\/span><\/li>\n  <li><span class='match'>Christmas<\/span><\/li>\n  <li><span class='match'>colleague<\/span><\/li>\n  <li><span class='match'>collect<\/span><\/li>\n  <li><span class='match'>college<\/span><\/li>\n  <li><span class='match'>comment<\/span><\/li>\n  <li><span class='match'>committee<\/span><\/li>\n  <li><span class='match'>community<\/span><\/li>\n  <li><span class='match'>company<\/span><\/li>\n  <li><span class='match'>compare<\/span><\/li>\n  <li><span class='match'>complete<\/span><\/li>\n  <li><span class='match'>compute<\/span><\/li>\n  <li><span class='match'>concern<\/span><\/li>\n  <li><span class='match'>condition<\/span><\/li>\n  <li><span class='match'>consider<\/span><\/li>\n  <li><span class='match'>consult<\/span><\/li>\n  <li><span class='match'>contact<\/span><\/li>\n  <li><span class='match'>continue<\/span><\/li>\n  <li><span class='match'>contract<\/span><\/li>\n  <li><span class='match'>control<\/span><\/li>\n  <li><span class='match'>converse<\/span><\/li>\n  <li><span class='match'>correct<\/span><\/li>\n  <li><span class='match'>council<\/span><\/li>\n  <li><span class='match'>country<\/span><\/li>\n  <li><span class='match'>current<\/span><\/li>\n  <li><span class='match'>decision<\/span><\/li>\n  <li><span class='match'>definite<\/span><\/li>\n  <li><span class='match'>department<\/span><\/li>\n  <li><span class='match'>describe<\/span><\/li>\n  <li><span class='match'>develop<\/span><\/li>\n  <li><span class='match'>difference<\/span><\/li>\n  <li><span class='match'>difficult<\/span><\/li>\n  <li><span class='match'>discuss<\/span><\/li>\n  <li><span class='match'>district<\/span><\/li>\n  <li><span class='match'>document<\/span><\/li>\n  <li><span class='match'>economy<\/span><\/li>\n  <li><span class='match'>educate<\/span><\/li>\n  <li><span class='match'>electric<\/span><\/li>\n  <li><span class='match'>encourage<\/span><\/li>\n  <li><span class='match'>english<\/span><\/li>\n  <li><span class='match'>environment<\/span><\/li>\n  <li><span class='match'>especial<\/span><\/li>\n  <li><span class='match'>evening<\/span><\/li>\n  <li><span class='match'>evidence<\/span><\/li>\n  <li><span class='match'>example<\/span><\/li>\n  <li><span class='match'>exercise<\/span><\/li>\n  <li><span class='match'>expense<\/span><\/li>\n  <li><span class='match'>experience<\/span><\/li>\n  <li><span class='match'>explain<\/span><\/li>\n  <li><span class='match'>express<\/span><\/li>\n  <li><span class='match'>finance<\/span><\/li>\n  <li><span class='match'>fortune<\/span><\/li>\n  <li><span class='match'>forward<\/span><\/li>\n  <li><span class='match'>function<\/span><\/li>\n  <li><span class='match'>further<\/span><\/li>\n  <li><span class='match'>general<\/span><\/li>\n  <li><span class='match'>germany<\/span><\/li>\n  <li><span class='match'>goodbye<\/span><\/li>\n  <li><span class='match'>history<\/span><\/li>\n  <li><span class='match'>holiday<\/span><\/li>\n  <li><span class='match'>hospital<\/span><\/li>\n  <li><span class='match'>however<\/span><\/li>\n  <li><span class='match'>hundred<\/span><\/li>\n  <li><span class='match'>husband<\/span><\/li>\n  <li><span class='match'>identify<\/span><\/li>\n  <li><span class='match'>imagine<\/span><\/li>\n  <li><span class='match'>important<\/span><\/li>\n  <li><span class='match'>improve<\/span><\/li>\n  <li><span class='match'>include<\/span><\/li>\n  <li><span class='match'>increase<\/span><\/li>\n  <li><span class='match'>individual<\/span><\/li>\n  <li><span class='match'>industry<\/span><\/li>\n  <li><span class='match'>instead<\/span><\/li>\n  <li><span class='match'>interest<\/span><\/li>\n  <li><span class='match'>introduce<\/span><\/li>\n  <li><span class='match'>involve<\/span><\/li>\n  <li><span class='match'>kitchen<\/span><\/li>\n  <li><span class='match'>language<\/span><\/li>\n  <li><span class='match'>machine<\/span><\/li>\n  <li><span class='match'>meaning<\/span><\/li>\n  <li><span class='match'>measure<\/span><\/li>\n  <li><span class='match'>mention<\/span><\/li>\n  <li><span class='match'>million<\/span><\/li>\n  <li><span class='match'>minister<\/span><\/li>\n  <li><span class='match'>morning<\/span><\/li>\n  <li><span class='match'>necessary<\/span><\/li>\n  <li><span class='match'>obvious<\/span><\/li>\n  <li><span class='match'>occasion<\/span><\/li>\n  <li><span class='match'>operate<\/span><\/li>\n  <li><span class='match'>opportunity<\/span><\/li>\n  <li><span class='match'>organize<\/span><\/li>\n  <li><span class='match'>original<\/span><\/li>\n  <li><span class='match'>otherwise<\/span><\/li>\n  <li><span class='match'>paragraph<\/span><\/li>\n  <li><span class='match'>particular<\/span><\/li>\n  <li><span class='match'>pension<\/span><\/li>\n  <li><span class='match'>percent<\/span><\/li>\n  <li><span class='match'>perfect<\/span><\/li>\n  <li><span class='match'>perhaps<\/span><\/li>\n  <li><span class='match'>photograph<\/span><\/li>\n  <li><span class='match'>picture<\/span><\/li>\n  <li><span class='match'>politic<\/span><\/li>\n  <li><span class='match'>position<\/span><\/li>\n  <li><span class='match'>positive<\/span><\/li>\n  <li><span class='match'>possible<\/span><\/li>\n  <li><span class='match'>practise<\/span><\/li>\n  <li><span class='match'>prepare<\/span><\/li>\n  <li><span class='match'>present<\/span><\/li>\n  <li><span class='match'>pressure<\/span><\/li>\n  <li><span class='match'>presume<\/span><\/li>\n  <li><span class='match'>previous<\/span><\/li>\n  <li><span class='match'>private<\/span><\/li>\n  <li><span class='match'>probable<\/span><\/li>\n  <li><span class='match'>problem<\/span><\/li>\n  <li><span class='match'>proceed<\/span><\/li>\n  <li><span class='match'>process<\/span><\/li>\n  <li><span class='match'>produce<\/span><\/li>\n  <li><span class='match'>product<\/span><\/li>\n  <li><span class='match'>programme<\/span><\/li>\n  <li><span class='match'>project<\/span><\/li>\n  <li><span class='match'>propose<\/span><\/li>\n  <li><span class='match'>protect<\/span><\/li>\n  <li><span class='match'>provide<\/span><\/li>\n  <li><span class='match'>purpose<\/span><\/li>\n  <li><span class='match'>quality<\/span><\/li>\n  <li><span class='match'>quarter<\/span><\/li>\n  <li><span class='match'>question<\/span><\/li>\n  <li><span class='match'>realise<\/span><\/li>\n  <li><span class='match'>receive<\/span><\/li>\n  <li><span class='match'>recognize<\/span><\/li>\n  <li><span class='match'>recommend<\/span><\/li>\n  <li><span class='match'>relation<\/span><\/li>\n  <li><span class='match'>remember<\/span><\/li>\n  <li><span class='match'>represent<\/span><\/li>\n  <li><span class='match'>require<\/span><\/li>\n  <li><span class='match'>research<\/span><\/li>\n  <li><span class='match'>resource<\/span><\/li>\n  <li><span class='match'>respect<\/span><\/li>\n  <li><span class='match'>responsible<\/span><\/li>\n  <li><span class='match'>saturday<\/span><\/li>\n  <li><span class='match'>science<\/span><\/li>\n  <li><span class='match'>scotland<\/span><\/li>\n  <li><span class='match'>secretary<\/span><\/li>\n  <li><span class='match'>section<\/span><\/li>\n  <li><span class='match'>separate<\/span><\/li>\n  <li><span class='match'>serious<\/span><\/li>\n  <li><span class='match'>service<\/span><\/li>\n  <li><span class='match'>similar<\/span><\/li>\n  <li><span class='match'>situate<\/span><\/li>\n  <li><span class='match'>society<\/span><\/li>\n  <li><span class='match'>special<\/span><\/li>\n  <li><span class='match'>specific<\/span><\/li>\n  <li><span class='match'>standard<\/span><\/li>\n  <li><span class='match'>station<\/span><\/li>\n  <li><span class='match'>straight<\/span><\/li>\n  <li><span class='match'>strategy<\/span><\/li>\n  <li><span class='match'>structure<\/span><\/li>\n  <li><span class='match'>student<\/span><\/li>\n  <li><span class='match'>subject<\/span><\/li>\n  <li><span class='match'>succeed<\/span><\/li>\n  <li><span class='match'>suggest<\/span><\/li>\n  <li><span class='match'>support<\/span><\/li>\n  <li><span class='match'>suppose<\/span><\/li>\n  <li><span class='match'>surprise<\/span><\/li>\n  <li><span class='match'>telephone<\/span><\/li>\n  <li><span class='match'>television<\/span><\/li>\n  <li><span class='match'>terrible<\/span><\/li>\n  <li><span class='match'>therefore<\/span><\/li>\n  <li><span class='match'>thirteen<\/span><\/li>\n  <li><span class='match'>thousand<\/span><\/li>\n  <li><span class='match'>through<\/span><\/li>\n  <li><span class='match'>thursday<\/span><\/li>\n  <li><span class='match'>together<\/span><\/li>\n  <li><span class='match'>tomorrow<\/span><\/li>\n  <li><span class='match'>tonight<\/span><\/li>\n  <li><span class='match'>traffic<\/span><\/li>\n  <li><span class='match'>transport<\/span><\/li>\n  <li><span class='match'>trouble<\/span><\/li>\n  <li><span class='match'>tuesday<\/span><\/li>\n  <li><span class='match'>understand<\/span><\/li>\n  <li><span class='match'>university<\/span><\/li>\n  <li><span class='match'>various<\/span><\/li>\n  <li><span class='match'>village<\/span><\/li>\n  <li><span class='match'>wednesday<\/span><\/li>\n  <li><span class='match'>welcome<\/span><\/li>\n  <li><span class='match'>whether<\/span><\/li>\n  <li><span class='match'>without<\/span><\/li>\n  <li><span class='match'>yesterday<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
```

[](https://bookdown.org/wangminjie/R4DS/images/regex_repeat.jpg)

### 分组与回溯


```r
ft <- fruit %>% 
  head(10)
ft
```

```
##  [1] "apple"        "apricot"      "avocado"      "banana"      
##  [5] "bell pepper"  "bilberry"     "blackberry"   "blackcurrant"
##  [9] "blood orange" "blueberry"
```

如何匹配`ft`中那些字母是连续重复两次的呢？使用`.`进行尝试:


```r
str_view(ft, ".{2}", match = TRUE)
```

```{=html}
<div id="htmlwidget-46a139c1ddecb5bf41c6" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-46a139c1ddecb5bf41c6">{"x":{"html":"<ul>\n  <li><span class='match'>ap<\/span>ple<\/li>\n  <li><span class='match'>ap<\/span>ricot<\/li>\n  <li><span class='match'>av<\/span>ocado<\/li>\n  <li><span class='match'>ba<\/span>nana<\/li>\n  <li><span class='match'>be<\/span>ll pepper<\/li>\n  <li><span class='match'>bi<\/span>lberry<\/li>\n  <li><span class='match'>bl<\/span>ackberry<\/li>\n  <li><span class='match'>bl<\/span>ackcurrant<\/li>\n  <li><span class='match'>bl<\/span>ood orange<\/li>\n  <li><span class='match'>bl<\/span>ueberry<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
```

可以发现并没有得到想要的结果，这是就需要用**分组与回溯引用**的新技术，具体来看，要匹配`ft`中连续出现的字母：


```r
str_view(ft, "(.)\\1", match = TRUE)
```

```{=html}
<div id="htmlwidget-0fdfc45738d71efcc056" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-0fdfc45738d71efcc056">{"x":{"html":"<ul>\n  <li>a<span class='match'>pp<\/span>le<\/li>\n  <li>be<span class='match'>ll<\/span> pepper<\/li>\n  <li>bilbe<span class='match'>rr<\/span>y<\/li>\n  <li>blackbe<span class='match'>rr<\/span>y<\/li>\n  <li>blackcu<span class='match'>rr<\/span>ant<\/li>\n  <li>bl<span class='match'>oo<\/span>d orange<\/li>\n  <li>bluebe<span class='match'>rr<\/span>y<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
```

- 使用`"(.)\\1"`对两个连续重复的字母进行匹配。`\\1`表示重复`(.)`中匹配到的字母。

- 同理，使用`"(..)\\1"`可以匹配`abab`形式的字符串。

- 使用`"(.)(.)\\2\\1"`可以匹配`abba`形式的字符串，简单理解，`\\2`表示重复第二个`(.)`，`\\1`表示重复第一个`(.)`。

## 解决实际问题

1. 正则表达式不回重叠匹配:


```r
str_view_all("abababa", "aba")
```

```{=html}
<div id="htmlwidget-703f5608f84c1098bd29" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-703f5608f84c1098bd29">{"x":{"html":"<ul>\n  <li><span class='match'>aba<\/span>b<span class='match'>aba<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
```


2. `str_extract()`提取匹配内容，只提取第一个匹配：谁先匹配就提取谁。


```r
sentences <- stringr::sentences

# 建立颜色正则表达式
colors <- c(
  "red", "orange", "yellow",
  "green", "blue", "purple"
)
color_match <- str_c(colors, collapse = "|")
color_match
```

```
## [1] "red|orange|yellow|green|blue|purple"
```

```r
# 提取sentences中包含一种颜色的句子
has_color_one <- str_subset(sentences, color_match)
# 只包含一个颜色的句子所包含的分别是什么颜色
matches <- str_extract(has_color_one, color_match) 
head(matches)
```

```
## [1] "blue" "blue" "red"  "red"  "red"  "blue"
```

```r
# 提取包含多个颜色的句子
more <- sentences[str_count(sentences, color_match) > 1]
more
```

```
## [1] "It is hard to erase blue or red ink."          
## [2] "The green light in the brown box flickered."   
## [3] "The sky in the west is tinged with orange red."
```

```r
# 提取包含多个颜色的句子中所包含的颜色
tibble(more) %>% 
  mutate(color = str_extract(more, color_match))
```

```
## # A tibble: 3 x 2
##   more                                           color 
##   <chr>                                          <chr> 
## 1 It is hard to erase blue or red ink.           blue  
## 2 The green light in the brown box flickered.    green 
## 3 The sky in the west is tinged with orange red. orange
```

## 案例分析

### 案例1

将数据框`dt`装第二列的数值提取出来，并构成新的列。


```r
dt <- tibble(
  x = 1:4,
  y = c("wk 3", "week-1", "7", "w#9")
)
dt
```

```
## # A tibble: 4 x 2
##       x y     
##   <int> <chr> 
## 1     1 wk 3  
## 2     2 week-1
## 3     3 7     
## 4     4 w#9
```

```r
dt %>% 
  mutate(
    z = str_extract(y, "[0-9]")
  )
```

```
## # A tibble: 4 x 3
##       x y      z    
##   <int> <chr>  <chr>
## 1     1 wk 3   3    
## 2     2 week-1 1    
## 3     3 7      7    
## 4     4 w#9    9
```

### 案例2

提取df中第二列中所有的大写字母。


```r
df <- data.frame(
  x = seq_along(1:7),
  y = c("2016123456", "20150513", "AB2016123456", "J2017000987", "B2017000987C", "aksdf", "2014")
)
df
```

```
##   x            y
## 1 1   2016123456
## 2 2     20150513
## 3 3 AB2016123456
## 4 4  J2017000987
## 5 5 B2017000987C
## 6 6        aksdf
## 7 7         2014
```

```r
df %>% 
  mutate(
    z = str_extract_all(y, "[A-Z]")
  ) %>% 
  unnest(z)
```

```
## # A tibble: 5 x 3
##       x y            z    
##   <int> <chr>        <chr>
## 1     3 AB2016123456 A    
## 2     3 AB2016123456 B    
## 3     4 J2017000987  J    
## 4     5 B2017000987C B    
## 5     5 B2017000987C C
```

### 案例三

将下列数据集中的中英文分开。


```r
tb <- tibble(x = c("I我", "love爱", "you你"))
tb
```

```
## # A tibble: 3 x 1
##   x     
##   <chr> 
## 1 I我   
## 2 love爱
## 3 you你
```

```r
tb %>% 
  extract(
    x, c("en", "cn"), "([a-zA-Z+])([^a-zA-Z]+)",
          remove = FALSE)
```

```
## # A tibble: 3 x 3
##   x      en    cn   
##   <chr>  <chr> <chr>
## 1 I我    I     我   
## 2 love爱 e     爱   
## 3 you你  u     你
```

### 案例4

提取数据框中x列的起始数字。


```r
df <- tibble(x = c("1-12week", "1-10week", "5-12week"))
df
```

```
## # A tibble: 3 x 1
##   x       
##   <chr>   
## 1 1-12week
## 2 1-10week
## 3 5-12week
```

```r
df %>% 
  extract(
    x, c("start", "end", "cn"),
    "(\\d+)-(\\d+)(\\D+)",
    remove = FALSE
  )
```

```
## # A tibble: 3 x 4
##   x        start end   cn   
##   <chr>    <chr> <chr> <chr>
## 1 1-12week 1     12    week 
## 2 1-10week 1     10    week 
## 3 5-12week 5     12    week
```

### 案例5 

提取大写字母后的数字。


```r
df <- tibble(
  x = c("12W34", "AB2C46", "B217C", "akTs6df", "21WD4")
)
df
```

```
## # A tibble: 5 x 1
##   x      
##   <chr>  
## 1 12W34  
## 2 AB2C46 
## 3 B217C  
## 4 akTs6df
## 5 21WD4
```

```r
df %>% 
  mutate(item = str_extract_all(x, "(?<=[A-Z])[0-9]")) %>% 
  unnest(item)
```

```
## # A tibble: 5 x 2
##   x      item 
##   <chr>  <chr>
## 1 12W34  3    
## 2 AB2C46 2    
## 3 AB2C46 4    
## 4 B217C  2    
## 5 21WD4  4
```

```r
# 提取数字前的大写字母

df %>% 
  mutate(item = str_extract_all(x, "[A-Z](?=[0-9])")) %>% 
  unnest(item)
```

```
## # A tibble: 5 x 2
##   x      item 
##   <chr>  <chr>
## 1 12W34  W    
## 2 AB2C46 B    
## 3 AB2C46 C    
## 4 B217C  B    
## 5 21WD4  D
```

### 案例6 

提取数字并求和


```r
df <- tibble(
  x = c("1234", "B246", "217C", "2357f", "21WD4")
)
df
```

```
## # A tibble: 5 x 1
##   x    
##   <chr>
## 1 1234 
## 2 B246 
## 3 217C 
## 4 2357f
## 5 21WD4
```

```r
df %>% 
  mutate(num = str_match_all(x, "\\d")) %>% 
  unnest(num) %>% 
  mutate_at(vars(num), as.numeric) %>%
  group_by(x) %>%
  summarise(sum = sum(num))
```

```
## # A tibble: 5 x 2
##   x       sum
##   <chr> <dbl>
## 1 1234     10
## 2 217C     10
## 3 21WD4     7
## 4 2357f    17
## 5 B246     12
```

### 案例7 

提取`Sichuan Univ`后面的学院。


```
## # A tibble: 8 x 2
##      No address                      
##   <int> <chr>                        
## 1     1 Sichuan Univ, Coll Chem      
## 2     2 Sichuan Univ, Coll Elect Engn
## 3     3 Sichuan Univ, Dept Phys      
## 4     4 Sichuan Univ, Coll Life Sci  
## 5     6 Sichuan Univ, Food Engn      
## 6     7 Sichuan Univ, Coll Phys      
## 7     8 Sichuan Univ, Sch Business   
## 8     9 Wuhan Univ, Mat Sci
```

```
## # A tibble: 8 x 3
##      No address                       coll              
##   <int> <chr>                         <chr>             
## 1     1 Sichuan Univ, Coll Chem       " Coll Chem"      
## 2     2 Sichuan Univ, Coll Elect Engn " Coll Elect Engn"
## 3     3 Sichuan Univ, Dept Phys       " Dept Phys"      
## 4     4 Sichuan Univ, Coll Life Sci   " Coll Life Sci"  
## 5     6 Sichuan Univ, Food Engn       " Food Engn"      
## 6     7 Sichuan Univ, Coll Phys       " Coll Phys"      
## 7     8 Sichuan Univ, Sch Business    " Sch Business"   
## 8     9 Wuhan Univ, Mat Sci            <NA>
```

