---
title: "pandas Wishes"
description: |
  Features that I miss in pandas coming from the tidyverse.
categories:
  - pandas
  - tidyverse
  - Spark
  - Python
  - R
author:
  - name: Bruno Testaguzza Carlin
    url: https://twosidesdata.netlify.app/
preview: https://rstudio.github.io/distill/images/javascript-d3-preview.png
css: ../../css/hover.css
date: 2022-02-28
output:
  distill::distill_article:
    keep_md: true
    toc: true
    toc_float: true
---




# Setup


## Link to the GitHub source code

<div class="layout-chunk" data-layout="l-body">

```{=html}
<a href="https://github.com/brunocarlin/carlin/blob/main/_posts/2022-02-28-pandaswishes/pandaswishes.Rmd">
<button class="btn btn-default"><i class="fa fa-github"></i> See the source rmd file</button>
</a>
```

</div>


## Libraries in R

<div class="layout-chunk" data-layout="l-body">
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='kw'><a href='https://rdrr.io/r/base/library.html'>library</a></span><span class='op'>(</span><span class='va'><a href='https://tidyverse.tidyverse.org'>tidyverse</a></span><span class='op'>)</span>
<span class='kw'><a href='https://rdrr.io/r/base/library.html'>library</a></span><span class='op'>(</span><span class='va'><a href='https://r-datatable.com'>data.table</a></span><span class='op'>)</span>
</code></pre></div>

</div>



## Libraries in Python

<div class="layout-chunk" data-layout="l-body">

```python
import pandas as pd
import numpy as np
```

</div>


## The Data: Penguins!

[Horst AM, Hill AP, Gorman KB (2020). palmerpenguins: Palmer Archipelago (Antarctica) penguin data. R package
  version 0.1.0. https://allisonhorst.github.io/palmerpenguins/](https://github.com/allisonhorst/palmerpenguins/)

I choose the Palmer Penguins Dataset. Some call it the new iris after the iris dataset was rightly criticized for Eugenics issues. You can read more about it here [Iris dataset retirement](https://armchairecology.blog/iris-dataset/).

### What does the dataset look like? 

<div class="layout-chunk" data-layout="l-body">
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>penguins</span> <span class='op'>&lt;-</span> <span class='fu'>palmerpenguins</span><span class='fu'>::</span><span class='va'><a href='https://allisonhorst.github.io/palmerpenguins/reference/penguins.html'>penguins</a></span>

<span class='fu'><a href='https://rdrr.io/r/utils/head.html'>head</a></span><span class='op'>(</span><span class='va'>penguins</span><span class='op'>)</span>
</code></pre></div>

```
# A tibble: 6 x 8
  species island    bill_length_mm bill_depth_mm flipper_length_mm
  <fct>   <fct>              <dbl>         <dbl>             <int>
1 Adelie  Torgersen           39.1          18.7               181
2 Adelie  Torgersen           39.5          17.4               186
3 Adelie  Torgersen           40.3          18                 195
4 Adelie  Torgersen           NA            NA                  NA
5 Adelie  Torgersen           36.7          19.3               193
6 Adelie  Torgersen           39.3          20.6               190
# ... with 3 more variables: body_mass_g <int>, sex <fct>, year <int>
```

<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='fu'><a href='https://pillar.r-lib.org/reference/glimpse.html'>glimpse</a></span><span class='op'>(</span><span class='va'>penguins</span>,width <span class='op'>=</span> <span class='fl'>50</span><span class='op'>)</span>
</code></pre></div>

```
Rows: 344
Columns: 8
$ species           <fct> Adelie, Adelie, Adelie~
$ island            <fct> Torgersen, Torgersen, ~
$ bill_length_mm    <dbl> 39.1, 39.5, 40.3, NA, ~
$ bill_depth_mm     <dbl> 18.7, 17.4, 18.0, NA, ~
$ flipper_length_mm <int> 181, 186, 195, NA, 193~
$ body_mass_g       <int> 3750, 3800, 3250, NA, ~
$ sex               <fct> male, female, female, ~
$ year              <int> 2007, 2007, 2007, 2007~
```

</div>


The dataset consists of 344 samples of three species of Penguins.

### How do they look like?

The three species have different characteristics.

![Artwork by @allison_horst](https://github.com/allisonhorst/palmerpenguins/raw/master/man/figures/lter_penguins.png)

And what the hell is Bill Lenght and Depth?

![Artwork by @allison_horst](https://github.com/allisonhorst/palmerpenguins/raw/master/man/figures/culmen_depth.png)

### Interaction among variables and species

[Credit to Allison once again](https://allisonhorst.github.io/palmerpenguins/articles/examples.html)

#### Penguin mass vs. flipper length

<div class="layout-chunk" data-layout="l-body">
![](pandaswishes_files/figure-html5/mass_vs_flipper_lenght-1.png)<!-- -->

</div>


As expected from the picture, the Gento species is a bit bigger, and we can see a heavy correlation between Flipper length and weight.

#### Bill length vs. depth

<div class="layout-chunk" data-layout="l-body">
![](pandaswishes_files/figure-html5/bill_lenght_vs_depth-1.png)<!-- -->

</div>


As we saw in the picture, the Adelie has smaller-length Bills.  
The Gentoo has much more elongated ones than the Adelie, but they look 'flatter,' or as the dataset calls it, they have a smaller Bill depth.
The Chinstrap are positioned on the bigger side of both metrics.

#### Impact of sex on size


<div class="layout-chunk" data-layout="l-body">
![](pandaswishes_files/figure-html5/sex_vs_size-1.png)<!-- -->

</div>


# My experience

At this point I have worked more than 4 years with both Python and R, Python has been my language choice when interacting with colleges simply because of culture or better support in platforms such as AWS, but it is important to understand that R was my first data language (I dabbled in some Macros on excel before) and I have used it since graduation more than 7 years ago.


# The tasks of pandas framework?

Firstly, I call it a framework because pandas comes bundled with NumPy and other friends like matplot or seaborn. It is fair to compare them as a bundle since the competition is also split among many packages. I will borrow a picture from the tidyverse to explain where pandas stands on the analysis workflow.

![Credit tohttps://oliviergimenez.github.io/](https://oliviergimenez.github.io/intro_tidyverse/assets/img/data-science-workflow.png)

pandas Is responsible for the Import -> Tidy -> Transform part

But it can sometimes reach a little bit into Visualize by delegating when necessary. If your communication task is a graph, you can use matplot or seaborn, and there are also some simple APIs inside pandas that can plot basic graphs.   


If you are importing from parquet, sometimes pandas calls on the arrow package.  


There are many other examples, but I think of pandas as this central player on the data workflow of the Python ecosystem, it is unmatched in its amount of methods, and therefore it should be expected that some things could use some improvement, and that is the point of this post, I want pandas to change for the better, I want to discuss why pandas is in this state of right now and what I would like to see instead.

# The current landscape

In R, I have used three different frameworks when dealing with data that can be considered a problem. Still, there is a crucial difference when comparing Python and R, you will usually find competition inside R, and it happens on every sphere, modeling has the tidymodels vs. mlr fight, visualization has plotly vs. ggplot2, statistical tests sometimes have tens of different implementations from all over the world, etc. As an economist, I see this competition with good eyes. The competition has fostered fast and tested change on multiple instances as competing 
## The timeline

First there was Base R created on August 1993; 28 years ago according to Wikipedia, but with many more years of baggage from the S era. 
Then pandas starts development around 2005 and gets open sourced on 2008, it is a game changer and vastly more declarative and faster than Base R.
data.table comes along on the premise of speed matters a lot, remember that it is 2006 and the single big machine paradigm was the end all for big data analysis, maybe it wasn't even called big data back then. 
the tidyverse releases dplyr on the back of the huge success of ggplot2, it wasn't even called the tidyverse back then.
And finally Koalas get created on 2019 to help pandas scale with PySpark, it eventually get incorporated back into PySpark as the pyspark.pandas api.


## Base R

![](https://cdn.vox-cdn.com/thumbor/MpA2HVftSFntl9HhmhlQA3MEjIU=/0x0:1409x785/1200x800/filters:focal(622x252:846x476)/cdn.vox-cdn.com/uploads/chorus_image/image/55701647/Screen_Shot_2017_07_13_at_1.09.20_PM.0.png)

The oldest, most of the time slowest way, base R has scared so many away from r. You will see some nasty behaviors here, like the famous partial string matching or the very crypt function names and arguments. Base R feels dated because it is dated, now more than 20 years old, this framework inspired pandas, but you need some patience if you are using it. There is an excellent book about all of Base R's little details that I highly recommend called [The R Inferno](https://www.burns-stat.com/pages/Tutor/R_inferno.pdf)

Base r implement some design ideas that users of the pandas ecosystem will recognize, like the dreadful row names pandas turned into indexes and using square brackets for 2d manipulation of data.

Some details of R for the Python folks, R doesn't expect you to know what a pointer is because R doesn't expect you to be a regular programmer. Base R envisions a statistician with some thousand lines of beautiful math that got turned into a package. This means that if you assign a copy of a data.frame to a new name, R initially creates a pointer, and eventually, if you have changed it in a destructible way, R automatically copies it into another new object. There is no need to keep manually using the copy method as in pandas.

### Data Prep

<div class="layout-chunk" data-layout="l-body-outset">
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>penguins_base</span> <span class='op'>&lt;-</span> <span class='va'>penguins</span> |&gt; <span class='fu'><a href='https://rdrr.io/r/base/as.data.frame.html'>as.data.frame</a></span><span class='op'>(</span><span class='op'>)</span>
<span class='fu'><a href='https://rdrr.io/r/base/row.names.html'>row.names</a></span><span class='op'>(</span><span class='va'>penguins_base</span><span class='op'>)</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://stringr.tidyverse.org/reference/str_c.html'>str_c</a></span><span class='op'>(</span><span class='st'>'penguin_'</span>,<span class='fu'><a href='https://dplyr.tidyverse.org/reference/ranking.html'>row_number</a></span><span class='op'>(</span><span class='va'>penguins_base</span><span class='op'>$</span><span class='va'>species</span><span class='op'>)</span><span class='op'>)</span>

<span class='fu'><a href='https://rdrr.io/r/utils/head.html'>head</a></span><span class='op'>(</span><span class='va'>penguins_base</span><span class='op'>)</span> |&gt; <span class='fu'>knitr</span><span class='fu'>::</span><span class='fu'><a href='https://rdrr.io/pkg/knitr/man/kable.html'>kable</a></span><span class='op'>(</span><span class='op'>)</span>
</code></pre></div>


|          |species |island    | bill_length_mm| bill_depth_mm| flipper_length_mm| body_mass_g|sex    | year|
|:---------|:-------|:---------|--------------:|-------------:|-----------------:|-----------:|:------|----:|
|penguin_1 |Adelie  |Torgersen |           39.1|          18.7|               181|        3750|male   | 2007|
|penguin_2 |Adelie  |Torgersen |           39.5|          17.4|               186|        3800|female | 2007|
|penguin_3 |Adelie  |Torgersen |           40.3|          18.0|               195|        3250|female | 2007|
|penguin_4 |Adelie  |Torgersen |             NA|            NA|                NA|          NA|NA     | 2007|
|penguin_5 |Adelie  |Torgersen |           36.7|          19.3|               193|        3450|female | 2007|
|penguin_6 |Adelie  |Torgersen |           39.3|          20.6|               190|        3650|male   | 2007|

</div>


### Slicing

Now let's use some base R

You can filter in a 2d manner based on df[x,y] just like pandas .loc

<div class="layout-chunk" data-layout="l-body">
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>penguins_base</span><span class='op'>[</span><span class='st'>'penguin_1'</span>,<span class='op'>]</span>
</code></pre></div>

```
          species    island bill_length_mm bill_depth_mm
penguin_1  Adelie Torgersen           39.1          18.7
          flipper_length_mm body_mass_g  sex year
penguin_1               181        3750 male 2007
```

</div>


<div class="layout-chunk" data-layout="l-body">
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>penguins_base</span><span class='op'>[</span><span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='st'>'penguin_1'</span>,<span class='st'>'penguin_10'</span><span class='op'>)</span>,<span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='st'>'species'</span>,<span class='st'>'island'</span><span class='op'>)</span><span class='op'>]</span>
</code></pre></div>

```
           species    island
penguin_1   Adelie Torgersen
penguin_10  Adelie Torgersen
```

</div>


It even feels like pandas all the way into it randomly deciding to change my data types.

<div class="layout-chunk" data-layout="l-body">
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>penguins_base</span><span class='op'>[</span><span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='st'>'penguin_1'</span>,<span class='st'>'penguin_10'</span><span class='op'>)</span>,<span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='st'>'species'</span><span class='op'>)</span><span class='op'>]</span>
</code></pre></div>

```
[1] Adelie Adelie
Levels: Adelie Chinstrap Gentoo
```

</div>


Yep, we just fell out of a data.frame straight into a vector of the factor class, fantastic.

### Complex manipulation

Let's try to get the mean of the kgs of the females by species.


Doing some complex calculations on base R feels like a chore, but some functions that work as an apply on steroids may usually.

You will also rely heavily on saving intermediary df's unless you overwrite the original, or you cheat a little bit and use pipes from the tidyverse (I don't even think of it as cheating anymore as pipes come natively with R since the  4.0 release), as I will explain the tidyverse is a superset of base, meaning that it can be used inside of the Base R workflow. It can borrow functions from base as well. Some may call this a modern Base R code as it would not run on earlier than 4.0 versions of R. This also enables shorter anonymous functions using '\' instead of 'function'.



<div class="layout-chunk" data-layout="l-body">
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>results_base</span> <span class='op'>&lt;-</span> <span class='va'>penguins_base</span><span class='op'>[</span><span class='va'>penguins_base</span><span class='op'>[</span><span class='st'>'sex'</span><span class='op'>]</span> <span class='op'>==</span> <span class='st'>'female'</span>,<span class='op'>]</span> |&gt;
  <span class='fu'><a href='https://rdrr.io/r/base/with.html'>with</a></span><span class='op'>(</span><span class='fu'><a href='https://rdrr.io/r/stats/aggregate.html'>aggregate</a></span><span class='op'>(</span>x <span class='op'>=</span><span class='va'>body_mass_g</span>,by <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/list.html'>list</a></span><span class='op'>(</span><span class='va'>species</span><span class='op'>)</span>,FUN <span class='op'>=</span> \<span class='op'>(</span><span class='va'>x</span><span class='op'>)</span> <span class='fu'><a href='https://rdrr.io/r/base/mean.html'>mean</a></span><span class='op'>(</span><span class='va'>x</span><span class='op'>)</span><span class='op'>/</span><span class='fl'>1000</span><span class='op'>)</span><span class='op'>)</span>

<span class='va'>results_base</span>
</code></pre></div>

```
    Group.1        x
1    Adelie 3.368836
2 Chinstrap 3.527206
3    Gentoo 4.679741
```

</div>


Nice looking table, It is hell to use multiple functions, but if you know what you are doing is simple, Base R gets the job done with no imports... if you still care about that.

## data.table

The motto here is gotta go fast![](https://memegenerator.net/img/instances/65946828.jpg)


Going as far as naturally parallelize execution on local cores when possible, some love the syntax, I honestly think it is the worst out of all the options, but when speed on a single machine is relevant (something that I encounter less and less as we will discuss later) data.table really shines, outperforming just about anything I have ever used on Python and R.

### Data Prep

<div class="layout-chunk" data-layout="l-body">
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>penguins_data_table</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://Rdatatable.gitlab.io/data.table/reference/as.data.table.html'>as.data.table</a></span><span class='op'>(</span><span class='va'>penguins</span><span class='op'>)</span>
</code></pre></div>

</div>



### Complex manipulation

<div class="layout-chunk" data-layout="l-body">
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>penguins_data_table</span><span class='op'>[</span>,<span class='fu'><a href='https://rdrr.io/r/base/list.html'>list</a></span><span class='op'>(</span><span class='va'>species</span>,<span class='va'>body_mass_g</span><span class='op'>)</span><span class='op'>]</span>
</code></pre></div>

```
       species body_mass_g
  1:    Adelie        3750
  2:    Adelie        3800
  3:    Adelie        3250
  4:    Adelie          NA
  5:    Adelie        3450
 ---                      
340: Chinstrap        4000
341: Chinstrap        3400
342: Chinstrap        3775
343: Chinstrap        4100
344: Chinstrap        3775
```

<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>penguins_data_table</span><span class='op'>[</span><span class='va'>species</span> <span class='op'><a href='https://rdrr.io/r/base/match.html'>%in%</a></span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='st'>'Adelie'</span>,<span class='st'>'Gentoo'</span><span class='op'>)</span> <span class='op'>&amp;</span> <span class='va'>sex</span> <span class='op'>==</span> <span class='st'>'female'</span>,<span class='fu'><a href='https://rdrr.io/r/base/list.html'>list</a></span><span class='op'>(</span><span class='va'>species</span>,<span class='va'>body_mass_g</span><span class='op'>)</span><span class='op'>]</span><span class='op'>[</span>,<span class='fu'><a href='https://rdrr.io/r/base/lapply.html'>lapply</a></span><span class='op'>(</span><span class='va'>.SD</span>,<span class='va'>mean</span>,na.rm <span class='op'>=</span> <span class='cn'>TRUE</span><span class='op'>)</span>,<span class='va'>species</span><span class='op'>]</span>
</code></pre></div>

```
   species body_mass_g
1:  Adelie    3368.836
2:  Gentoo    4679.741
```

</div>


It produces these magical one-liners with speed to spare. The problem is that I can barely glimpse what I did here, as almost all of the execution depends on you remembering this model behind the scenes.

DT[i, j, by]  

  i = order by | select  
  j = update  
  by = group by  

And trust me when I say it gets complicated data.table is Turing complete as all options here are, and it is out there performing all of the functions of either dplyr or pandas, with just three arguments! That produces some of the most confusing pieces of code you will ever read, at least the data.table team killed the idea of row names as well.


## Dplyr / the tidyverse

![Storybench picture of the tidyverse](https://www.storybench.org/wp-content/uploads/2017/05/tidyverse-730x294.png)

My clear favorite, in a perfect world, everyone should know the tidyverse for the power it brings on expressing ideas about data with straightforward declarative syntax. This is very much the empowered version of SQL. A nice thing that I already showed on the base R part is that the tidyverse is only a part of the R ecosystem, meaning you can get your old statistic operations and just plug it into place. I will further detail how easy it is to develop an extension for the tidyverse but first, let's see some syntax.

### Simple Manipulation


<div class="layout-chunk" data-layout="l-body">
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>penguins</span> |&gt;
  <span class='fu'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span><span class='op'>(</span><span class='va'>species</span> <span class='op'><a href='https://rdrr.io/r/base/match.html'>%in%</a></span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='st'>'Adelie'</span>, <span class='st'>'Gentoo'</span><span class='op'>)</span>,
         <span class='va'>sex</span> <span class='op'>==</span> <span class='st'>'female'</span><span class='op'>)</span> |&gt; 
  <span class='fu'><a href='https://dplyr.tidyverse.org/reference/group_by.html'>group_by</a></span><span class='op'>(</span><span class='va'>species</span><span class='op'>)</span> |&gt; 
  <span class='fu'><a href='https://dplyr.tidyverse.org/reference/summarise.html'>summarise</a></span><span class='op'>(</span>body_mass_g_to_kg <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/mean.html'>mean</a></span><span class='op'>(</span><span class='va'>body_mass_g</span><span class='op'>)</span><span class='op'>/</span><span class='fl'>1000</span><span class='op'>)</span>
</code></pre></div>

```
# A tibble: 2 x 2
  species body_mass_g_to_kg
  <fct>               <dbl>
1 Adelie               3.37
2 Gentoo               4.68
```

</div>


This is an example of what dplyr can do while remaining very similar to English, you can opt into named arguments that are very well thought out, some of which have gone into twitter polls, the team at RStudio clearly thinks about usage and is willing to redesign old parts of the systems to reach new usability levels.

### Complex Manipulation

<div class="layout-chunk" data-layout="l-body">
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>result_tidyverse</span> <span class='op'>&lt;-</span> <span class='va'>penguins</span> |&gt;
  <span class='fu'><a href='https://dplyr.tidyverse.org/reference/select.html'>select</a></span><span class='op'>(</span><span class='op'>-</span><span class='va'>year</span><span class='op'>)</span> |&gt;
  <span class='fu'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span><span class='op'>(</span><span class='va'>species</span> <span class='op'><a href='https://rdrr.io/r/base/match.html'>%in%</a></span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='st'>'Adelie'</span>, <span class='st'>'Gentoo'</span><span class='op'>)</span>,
         <span class='va'>sex</span> <span class='op'>==</span> <span class='st'>'female'</span><span class='op'>)</span> |&gt;
  <span class='fu'><a href='https://dplyr.tidyverse.org/reference/group_by.html'>group_by</a></span><span class='op'>(</span><span class='va'>species</span><span class='op'>)</span> |&gt;
  <span class='fu'><a href='https://dplyr.tidyverse.org/reference/select.html'>select</a></span><span class='op'>(</span><span class='fu'>where</span><span class='op'>(</span><span class='va'>is.numeric</span><span class='op'>)</span><span class='op'>)</span> |&gt;
  <span class='fu'><a href='https://dplyr.tidyverse.org/reference/summarise.html'>summarise</a></span><span class='op'>(</span><span class='fu'><a href='https://dplyr.tidyverse.org/reference/across.html'>across</a></span><span class='op'>(</span>
    .cols <span class='op'>=</span> <span class='fu'>where</span><span class='op'>(</span>\<span class='op'>(</span><span class='va'>x</span><span class='op'>)</span> <span class='fu'><a href='https://rdrr.io/r/base/mean.html'>mean</a></span><span class='op'>(</span><span class='va'>x</span><span class='op'>)</span> <span class='op'>&gt;</span> <span class='fl'>188</span><span class='op'>)</span>,
    .fns <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/list.html'>list</a></span><span class='op'>(</span>median <span class='op'>=</span> <span class='va'>median</span>, mean <span class='op'>=</span> <span class='va'>mean</span><span class='op'>)</span>,
    .names <span class='op'>=</span> <span class='st'>"{.fn}-{.col}"</span>
  <span class='op'>)</span><span class='op'>)</span> |&gt;
  <span class='fu'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span><span class='op'>(</span><span class='fu'><a href='https://dplyr.tidyverse.org/reference/across.html'>across</a></span><span class='op'>(</span>
    .cols <span class='op'>=</span> <span class='fu'><a href='https://tidyselect.r-lib.org/reference/starts_with.html'>ends_with</a></span><span class='op'>(</span><span class='st'>'_g'</span><span class='op'>)</span>,
    .fns <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/list.html'>list</a></span><span class='op'>(</span>to_kg <span class='op'>=</span> \<span class='op'>(</span><span class='va'>x</span><span class='op'>)</span> <span class='va'>x</span> <span class='op'>/</span> <span class='fl'>1000</span><span class='op'>)</span>,
    .names <span class='op'>=</span> <span class='st'>"{.col}-{.fn}"</span>
  <span class='op'>)</span><span class='op'>)</span>

<span class='va'>result_tidyverse</span> |&gt; <span class='fu'>rmarkdown</span><span class='fu'>::</span><span class='fu'><a href='https://pkgs.rstudio.com/rmarkdown/reference/paged_table.html'>paged_table</a></span><span class='op'>(</span><span class='op'>)</span>
</code></pre></div>
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["species"],"name":[1],"type":["fct"],"align":["left"]},{"label":["median-flipper_length_mm"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["mean-flipper_length_mm"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["median-body_mass_g"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["mean-body_mass_g"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["median-body_mass_g-to_kg"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["mean-body_mass_g-to_kg"],"name":[7],"type":["dbl"],"align":["right"]}],"data":[{"1":"Adelie","2":"188","3":"187.7945","4":"3400","5":"3368.836","6":"3.4","7":"3.368836"},{"1":"Gentoo","2":"212","3":"212.7069","4":"4700","5":"4679.741","6":"4.7","7":"4.679741"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

</div>



This is incredibly similar to my usage of data manipulation in the real world. Some functions are simple, like some business metric is better on a kg basis instead of g, while others empower you to write incredibly succinct syntax that feels like a superpower for your laziness. You start to write smaller and smaller code to deal with more and more complex problems. I realize that most of what is in here exists solely on the tidyverse (for now) and that newcomers may not understand somewhat complex functions like across the first time they try to use it. Still, it is such a game-changer that functions like across exist, the alternatives being you sometimes writing tens of column names, or that you pre-compute a list as I will show it in pandas.


Also, you can see how the tidyverse easily interacts with custom functions. The anonymous function gets placed right into the middle of the pipeline without a custom method or any other fancy workaround, and it just builds upon what R offers.


One drawback is that your code feels 'old' pretty fast on the tidyverse ecosystem. In this example alone, the |> operator called a pipe, the new anonymous function syntax, the across, and the where functions from the package all have less than two years.


The tidyverse can also turn this code written in r, and with only a connection to a data source, compile it into code for another language, it is mainly SQL code, but this is super helpful, as I will show later on what I wished pandas implemented.



## pandas


![pandas](https://miro.medium.com/max/481/1*n_ms1q5YoHAQXXUIfeADKQ.png)


I usually think of pandas as a project to copy into Python what worked on other languages, mainly what people call base R and some SQL into Python, and it is hugely successful, and usage is most of the time a joyful experience, it is not the prettiest, but it can get the job done.

pandas is flexible enough to the point where you can write the same code and make it feel like Base R or Tidyverse depending on what methods you choose, for example, if you go heavy into indexing the base R style.

We can read its mission on the [page](https://pandas.pydata.org/about/)

Mission    
pandas aims to be the fundamental high-level building block for doing practical, real world data analysis in Python. Additionally, it has the broader goal of becoming the most powerful and flexible open source data analysis / manipulation tool available in any language.


That is quite a gredy statement, and I love it, pandas should strive for perfection, for power and flexibility, but let's try to see some current limitations and quirks which I dislike

### Set up data

We can easily import the penguins dataset by reading the repository csv.


<div class="layout-chunk" data-layout="l-body">

```python
penguins = pd.read_csv('https://raw.githubusercontent.com/allisonhorst/palmerpenguins/master/inst/extdata/penguins.csv')
```

</div>



### Slicing

Using loc, you get very close to base R philosophy.


<div class="layout-chunk" data-layout="l-body">

```python
df_result = penguins.loc[penguins.species.isin(['Adelie','Chinstrap']),['species','sex','body_mass_g']]

df_result
```

```
       species     sex  body_mass_g
0       Adelie    male       3750.0
1       Adelie  female       3800.0
2       Adelie  female       3250.0
3       Adelie     NaN          NaN
4       Adelie  female       3450.0
..         ...     ...          ...
339  Chinstrap    male       4000.0
340  Chinstrap  female       3400.0
341  Chinstrap    male       3775.0
342  Chinstrap    male       4100.0
343  Chinstrap  female       3775.0

[220 rows x 3 columns]
```

</div>


But pandas gets ahead of itself and starts changing the data types depending on the parameters, so what started out as a DataFrame may sometimes get back as a Series... You can avoid this behavior by passing lists on this example, this is similar to the data.table way, and it baffles me why this is even a possibility, it overcharges the slicing operations into another capacity of object type manipulators, and that is common in Pandas and data.frame, slicing is this super powerful method that may return wildly different results depending on very little change.


<div class="layout-chunk" data-layout="l-body">

```python
df_result = penguins.loc[penguins.species.isin(['Adelie','Chinstrap']),'species']

df_result
```

```
0         Adelie
1         Adelie
2         Adelie
3         Adelie
4         Adelie
         ...    
339    Chinstrap
340    Chinstrap
341    Chinstrap
342    Chinstrap
343    Chinstrap
Name: species, Length: 220, dtype: object
```

</div>


### Simple Manipulation


Numpy has a super similar syntax to the tidyverse if you opt into it

<div class="layout-chunk" data-layout="l-body">

```python

penguins\
  .query("species in ('Adelie', 'Gentoo')")\
  .groupby('species')\
  .agg({'body_mass_g':lambda x: np.mean(x)/1000})


# penguins |>
#   filter(species %in% c('Adelie', 'Gentoo'),
#          sex == 'female') |> 
#   group_by(species) |> 
#   summarise(body_mass_g_to_kg = mean(body_mass_g)/1000)
# df_full_indexes.columns = ["_".join(col_name).rstrip('_') for col_name in df_full_indexes.columns.to_flat_index()]
# 
# df_full_indexes['B_sum']
# 
# df_full_indexes['B_mean']
```

```
         body_mass_g
species             
Adelie      3.700662
Gentoo      5.076016
```

</div>


### Complex Manipulations

pandas has this tendency to create more and more indexes, drop_index  will quickly become your go to solution, and when we add hierachical indexes to the mix, you are going to be copying and pasting some solutions from Stack Overflow in order to flatten the data you created or you will need some sophisticated indexing operation to get some specific results back.

Another details is that I need to manually drop the columns before the groupby operation, and this sucks because there is no data type that won't exclude species (our grouping variable) while excluding island and sex 

<div class="layout-chunk" data-layout="l-body">

```python
penguins\
  .drop(columns = 'year')\
  .query("species in ('Adelie', 'Gentoo')")\
  .groupby('species')\
  .select_dtypes('numeric')
```

```
Error in py_call_impl(callable, dots$args, dots$keywords): AttributeError: 'DataFrameGroupBy' object has no attribute 'select_dtypes'

Detailed traceback:
  File "<string>", line 1, in <module>
  File "C:\Users\bruno\AppData\Local\R-MINI~1\lib\site-packages\pandas\core\groupby\groupby.py", line 904, in __getattr__
    raise AttributeError(
```

</div>

<div class="layout-chunk" data-layout="l-body">

```python
result_hierarchical = penguins\
  .drop(columns = ['year','island','sex'])\
  .query("species in ('Adelie', 'Gentoo')")\
  .groupby('species')\
  .agg([np.mean,np.median])

result_hierarchical
```

```
        bill_length_mm        bill_depth_mm  ... flipper_length_mm  body_mass_g        
                  mean median          mean  ...            median         mean  median
species                                      ...                                       
Adelie       38.791391   38.8     18.346358  ...             190.0  3700.662252  3700.0
Gentoo       47.504878   47.3     14.982114  ...             216.0  5076.016260  5000.0

[2 rows x 8 columns]
```

</div>


Now in order to access the body_mass_g columns and do a transformation to kg I need to deal with the index system without using the loc method.

<div class="layout-chunk" data-layout="l-body">

```python
result_hierarchical["body_mass_g"] /1000
```

```
             mean  median
species                  
Adelie   3.700662     3.7
Gentoo   5.076016     5.0
```

</div>

So you might just think, OK simple I just need to assign it back

<div class="layout-chunk" data-layout="l-body">

```python
result_hierarchical["body_mass_g"] = result_hierarchical["body_mass_g"] /1000
```

</div>


and it works if you don't mind losing the original data, if you want to create some new name

<div class="layout-chunk" data-layout="l-body">

```python
result_hierarchical["body_mass_g_back_to_g"] = result_hierarchical["body_mass_g"] * 1000
```

```
Error in py_call_impl(callable, dots$args, dots$keywords): ValueError: Expected a 1D array, got an array with shape (2, 2)

Detailed traceback:
  File "<string>", line 1, in <module>
  File "C:\Users\bruno\AppData\Local\R-MINI~1\lib\site-packages\pandas\core\frame.py", line 3645, in __setitem__
    self._set_item_frame_value(key, value)
  File "C:\Users\bruno\AppData\Local\R-MINI~1\lib\site-packages\pandas\core\frame.py", line 3788, in _set_item_frame_value
    self._set_item_mgr(key, arraylike)
  File "C:\Users\bruno\AppData\Local\R-MINI~1\lib\site-packages\pandas\core\frame.py", line 3802, in _set_item_mgr
    self._mgr.insert(len(self._info_axis), key, value)
  File "C:\Users\bruno\AppData\Local\R-MINI~1\lib\site-packages\pandas\core\internals\managers.py", line 1235, in insert
    raise ValueError(
```

</div>


Infuriating to say the least, you can go on a SO hunt to see the right approach to keep the indexes but at this point I am done with pandas indexing and just cheat my way into the result with some flattened data frame

<div class="layout-chunk" data-layout="l-body">

```python
# Flattern MultiIndex columns
result_hierarchical.columns = ["_".join(col_name).rstrip('_') for col_name in result_hierarchical.columns.to_flat_index()]

result_hierarchical['body_mass_g_median_back_to_g'] = result_hierarchical['body_mass_g_median'] * 1000


result_hierarchical['body_mass_g_median']
```

```
species
Adelie    3.7
Gentoo    5.0
Name: body_mass_g_median, dtype: float64
```

```python
result_hierarchical['body_mass_g_median_back_to_g']
```

```
species
Adelie    3700.0
Gentoo    5000.0
Name: body_mass_g_median_back_to_g, dtype: float64
```

</div>


## PySpark/Koalas

This framework is delightful to work with especially because you can go back and fourth between 4 apis SQL,Spark,Koalas and Pandas, and chances are one of them has a good approach to your problem, this post would deviate too much if I talked in depth about this framework, but it certainly has it's place on the big data manipulation side, with some apis that are sometimes superior to what pandas can offer, I will touch the issue of laziness on the topic of what I wanted that Pandas implemented, also PySpark really struggles with indexing as it should, and Koalas implements some crazy index rules.

It's vital that you understand that any speed analysis among packages on the individual personal computer level gets turned irrelevant as long as you get access to Spark Clusters, this is how you can query billions of records with ease, not by having slightly faster sorts on a single machine level.

# Grivances with pandas

I classify two kinds of problems on the pandas framework, the first and honestly the simplest to explain are things that it implements and I think it shouldn't a lot were copied from base R and that is why it was important that you understood the timeline of the packages at the beginning of this post, and the second are the new features that dplyr put into the table in recent years, features that when pandas was being created didn't exist and that I hope the pandas or other packages teams will eventually be able to integrate into the Python ecosystem.


# What pandas has and it shouldn't

There are many things that are strange to when I have to work in Python the first in that the syntax looks like someone from base r tried to port everthing to Python and unfortunally suceeded... this creates some strange patterns like the whole iloc vs loc vs [[]] debate, they all suck and I firmily believe that 2d manipulation of data was a mistake, a mistake that Python chose to copy.


## What pandas hasn't and it should
## Last updated on {.appendix}

<div class="layout-chunk" data-layout="l-body">

```
[1] "2022-03-02 11:59:45 -03"
```

</div>


## References {.appendix}

1. Horst AM, Hill AP, Gorman KB (2020). palmerpenguins: Palmer Archipelago (Antarctica) penguin data. R package
  version 0.1.0. https://allisonhorst.github.io/palmerpenguins/](https://github.com/allisonhorst/palmerpenguins/
```{.r .distill-force-highlighting-css}
```
