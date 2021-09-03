# Data wrangling 2

One of the key skills in an researcher's toolbox is the ability to work with data. When you run an experiment you get lots of data in various files. For instance, it is not uncommon for an experimental software to create a new file for every participant you run and for each participant's file to contain numerous columns and rows of data, only some of which are important. Being able to wrangle that data, manipulate it into different layouts, extract the parts you need, and summarise it, is one of the most important skills we will help you learn.

Over this course you will develop your skills in working with data. This chapter focuses on organizing data using the `tidyverse` package that you have read about in earlier chapters. Over the course, you will learn the main functions for data wrangling and how to use them, and we will use a number of different datasets to give you a wide range of exposure to what Psychology is about, and to reiterate that the same skills apply across different datasets. **The skills don't change, just the data!**

There are some questions to answer as you go along to test your skills: use the example code as a guide and the solutions are at the bottom. Remember to be pro-active in your learning, work together as a community, and if you get stuck use the **[cheatsheets](https://www.rstudio.com/resources/cheatsheets/)**. The key cheatsheet for this activity is the Data Transformation with dplyr.

## Learning to wrangle: Is there a chastity belt on perception

Nearly all data in research methods is stored in two-dimensional tables, either called data-frames, tables or tibbles. There are other ways of storing data that you will discover in time but mainly we will be using tibbles (if you would like more info, type `vignette("tibble")` in the console). A tibble is really just a table of data with columns and rows of information. But within that table you can get different types of data, i.e. numeric, integer, and character.

|Type of Data     | Description                                                  |
|:------------|:-------------------------------------------------------------| 
|Numeric     | Numbers including decimals  |
|Integer     | Numbers without decimals  |
|Character     | Tends to contain letters or be words                       |
|Factor     | Nominal (categorical). Can be words or numbers (e.g., male/1, female/2)                       |


Today we are going to be using data from this paper: [Is there a Chastity Belt on Perception](http://journals.sagepub.com/doi/abs/10.1177/0956797617730892). You can read the full paper if you like, but we will summarise the paper for you. The paper asks, **does your ability to perform an action influence your perception?** For instance, does your ability to hit a tennis ball influence how fast you perceive the ball to be moving? Or to phrase another way, do expert tennis players perceive the ball moving slower than novice tennis players?  

This experiment does not use tennis players however, they used the Pong task: "a computerised game in which participants aim to block moving balls with various sizes of paddles". A bit like a very classic retro arcade game. Participants tend to estimate the balls as moving faster when they have to block it with a smaller paddle as opposed to when they have a bigger paddle. You can read the paper to get more details if you wish but hopefully that gives enough of an idea to help you understand the wrangling we will do on the data. We have cleaned up the data a little to start with. Let's begin!

## Activity 1: Set-up Data Wrangling 2 {#dw2-a1}

* Download <a href="PongBlueRedBack 1-16 Codebook.csv" download>PongBlueRedBack 1-16 Codebook.csv</a> into your chapter folder.  
* Set the working directory to your chapter folder. Ensure the environment is clear.    
* Open a new R Markdown document and save it in your working directory. Call the file "Data wrangling 2". 
* If you're on the server, avoid a number of issues by restarting the session - click `Session` - `Restart R` 
* Delete the default R Markdown welcome text and insert a new code chunk.
* Copy and paste the below code into this code chunk and then run the code.


```r
library("tidyverse")
pong_data <- read_csv("PongBlueRedBack 1-16 Codebook.csv")
summary(pong_data)
```

```
##   Participant     JudgedSpeed      PaddleLength   BallSpeed    TrialNumber    
##  Min.   : 1.00   Min.   :0.0000   Min.   : 50   Min.   :2.0   Min.   :  1.00  
##  1st Qu.: 4.75   1st Qu.:0.0000   1st Qu.: 50   1st Qu.:3.0   1st Qu.: 72.75  
##  Median : 8.50   Median :1.0000   Median :150   Median :4.5   Median :144.50  
##  Mean   : 8.50   Mean   :0.5471   Mean   :150   Mean   :4.5   Mean   :144.50  
##  3rd Qu.:12.25   3rd Qu.:1.0000   3rd Qu.:250   3rd Qu.:6.0   3rd Qu.:216.25  
##  Max.   :16.00   Max.   :1.0000   Max.   :250   Max.   :7.0   Max.   :288.00  
##  BackgroundColor      HitOrMiss       BlockNumber   
##  Length:4608        Min.   :0.0000   Min.   : 1.00  
##  Class :character   1st Qu.:0.0000   1st Qu.: 3.75  
##  Mode  :character   Median :1.0000   Median : 6.50  
##                     Mean   :0.6866   Mean   : 6.50  
##                     3rd Qu.:1.0000   3rd Qu.: 9.25  
##                     Max.   :1.0000   Max.   :12.00
```
  
`summary()` provides a quick overview of the variables in your dataset and can be a useful quick check that you have indeed imported the correct data. It will also provide some basic descriptive statistics or whether the data is character (text) data which can also be useful to check.

## Activity 2: Look at your data {#dw2-a2}

Let's have a look at the `pong_data` and see how it is organized. Click on `pong_data` in your environment.

In the dataset you will see that each row (observation) represents one trial per participant and that there were 288 trials for each of the 16 participants. The columns (variables) we have in the dataset are as follows:

| Variable       |       Type         |           Description               |
|:--------------:|:-------------------|:------------------------------------|
| Participant        | integer            | participant number                  |
| JudgedSpeed   | integer            | speed judgement (1=fast, 0=slow)    |
| PaddleLength         | integer            | paddle length (pixels)              |
| BallSpeed          | integer            | ball speed (2 pixels/4ms)          |
| TrialNumber         | integer            | trial number                        |
| BackgroundColor      | character          | background display colour           |
| HitOrMiss         | integer            | hit ball=1, missed ball=0       |
| BlockNumber  | integer            | block number (out of 12 blocks)     |

We will use this data to master our skills of the Wickham Six verbs, taking each verb in turn. You should refer to the explanations and example code in Data Wrangling 1 to help you complete these. There are **6 verbs to work through** and  after that we will briefly recap on two other functions before finishing with a quick look at pipes. Try each activity and ask your peers or your tutor if you need help.


## Activity 3: **`select()`** {#dw2-a3}

Either by inclusion (telling R all the variables you want to keep) or exclusion (telling R which variables you want to drop), select only the `Participant`, `PaddleLength`, `TrialNumber`, `BackgroundColor` and `HitOrMiss` columns from `pong_data` and store it in a new object named `select_dat`.  

## Activity 4: Reorder the variables {#dw2-a4}

`select()` can also be used to reorder the columns in a table as the new table will display the variables in the order that you wrote them. Use `select()` to keep only the columns `Participant`, `JudgedSpeed`, `BallSpeed`, `TrialNumber`, and `HitOrMiss` from `pong_data` but have them display in alphabetical order, left to right. Save this table in a new object named `reorder_dat`.

## Activity 5: **`arrange()`** {#dw2-a5}

Arrange the data by two variables: `HitOrMiss` (putting hits - 1 - first), and `JudgedSpeed` (fast judgement - 1 - first) and store this in an object named `arrange_dat`.

## Activity 6: `filter()` {#dw2-a6}

Use `filter()` to extract all Participants in the original `pong_data` that had:

* a fast speed judgement;
* for speeds 2, 4, 5, and 7;
* but missed the ball.   

Store this remaining data in a new object called `pong_fast_miss`


<div class='webex-solution'><button>Helpful Hint</button>


There are three parts to this filter so it is best to think about them individually and then combine them.

1. Filter all fast speed judgements (`JudgedSpeed`)

2. Filter for the speeds 2, 4, 5 and 7 (`BallSpeed`)

3. Filter for all Misses (`HitOrMiss`)

You could do this in three filters where each one uses the output of the preceding one, or remember that filter functions can take more than one argument. Also, because the `JudgedSpeed` and `HitOrMiss` are Integer you will need `==` instead of just `=`.

</div>


<br>

<div class="warning">
<p>The filter function is very useful but if used wrongly can give you very misleading findings. This is why it is very important to always check your data after you perform an action. Let's say you are working in comparative psychology and have run a study looking at how cats, dogs and horses perceive emotion. Let's say the data is all stored in the tibble <code>animal_data</code> and there is a column called <code>animals</code> that tells you what type of animal your participant was. Imagine you wanted all the data from just cats:</p>
<p><code>filter(animal_data, animals == "cat")</code></p>
<p>Exactly! But what if you wanted cats and dogs?</p>
<p><code>filter(animal_data, animals == "cat", animals == "dog")</code></p>
<p>Right? Wrong! This actually says "give me everything that is a cat and a dog". But nothing is a cat and a dog, that would be weird - like a dat or a cog. In fact you want everything that is either a cat <strong>or</strong> a dog:</p>
<p><code>filter(animal_data, animals %in% c("cat", "dog"))</code></p>
<p>You used this code when producing your own graph of babynames, it's a very helpful function so don't forget it exists!</p>
</div>


## Activity 7: `mutate()` {#dw2-a7}

In Data Wrangling 1, you learned how the `mutate()` function lets us create a new variable in our dataset. However, it also has another useful function in that it can be combined with `recode()` to create new columns with recoded values. For example, let's add a new column to `pong_data` in which the judged speed  is converted into a text label where `1` will become `Fast`, and `0` will become "Slow". Note that if you gave the recoded variable the same name as the original it would overwrite it.


```r
pong_data <- mutate(pong_data, 
                    JudgedSpeedLabel = recode(JudgedSpeed, 
                                                    "1" = "Fast", 
                                                    "0" = "Slow"))
```

The code here is is a bit complicated but:

* `JudgedSpeedLabel` is the name of your new column, 
* `JudgedSpeed` is the name of the old column and the one to take information from
* Fast and slow are the new codings of 1 and 0 respectively

The `mutate()` function is also handy for making some calculations on or across columns in your data. For example, say you realise you made a mistake in your experiment where your participant numbers should be 1 higher for every participant, i.e. Participant 1 should actually be numbered as Participant 2, etc. You would do something like:


```r
pong_data <- mutate(pong_data, Participant = Participant + 1)
```

Note here that you are giving the new column the same name as the old column `Participant`. What happens here is that you are **overwriting the old data with the new data**! So watch out, mutate can create a new column or overwrite an existing column, depending on what you tell it to do!  

Imagine you realise there is a mistake in your dataset and that all your trial numbers are wrong. The first trial (trial number 1) was a practice so should be excluded and your experiment actually started on trial 2. 

* Filter out all trials with the number 1 (`TrialNumber` column) from `pong_data`, 
* Then use the `mutate()` function to recount all the remaining trial numbers, starting them at one again instead of two. Overwrite `TrialNumber` in `pong_data` with this new data.

You can either do this in two separate steps and create a new object, or you can uses pipes `%>%` and do it it one line of code. 


<div class='webex-solution'><button>Helpful Hint</button>


Step 1. filter(`TrialNumber` does not equal 1) - remember to store this output in a variable?

Step 2. mutate(`TrialNumber` = TrialNumber minus 1)

</div>
  

## Activity 8: Summarising data {#dw2-a8}

Now we're going to calculate descriptive statistics for the data using `summarise()`. In this exercise we will calculate:

* The total (sum) number of hits for each combination of background colour and paddle length.
* The mean number of hits for each combination of background colour and paddle length

Because we want to produce descriptive statistics by groups (background colour and paddle length), there are two steps:

* Group the data by `BackgroundColor` and `PaddleLength` using `group_by()`.
* Then, use `summarise()` to calculate the total and mean number of hits (`HitOrMiss`) using the grouped data

We will do this using pipes to reduce the amount of code we write. Remember to try and read the code out loud and to pronounce `%>%` as 'and then'. Copy and paste the below code into a new code chunk and run the code.


```r
pong_data_hits<- pong_data %>% # take pong_data
  group_by(BackgroundColor, PaddleLength) %>% # then group it
  summarise(total_hits = sum(HitOrMiss, na.rm = TRUE),
            meanhits = mean(HitOrMiss, na.rm = TRUE)) # then summarise it
```

<div class="info">
<p>If you get what looks like an error that says <code>summarise() ungrouping output (override with .groups argument)</code>don't worry, this isn't an error it's just R telling you what it's done. This message was included in a very recent update to the <code>tidyverse</code> which is why it doesn't appear on some of the walkthrough vidoes.</p>
</div>

`summarise()` has a range of internal functions that make life really easy, e.g. `mean`, `sum`, `max`, `min`, etc. See the [dplyr cheatsheet](https://www.rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf) for more examples.

<div class="info">
<p><code>na.rm = TRUE</code> is an argument that we can add when calculating descriptive statistics to tell R what to do if there are missing values. In this dataset, there are no missing values but if there were and we asked R to calculate the mean, it would return <code>NA</code> as the result because it doesn't know how to average nothing. Remember this argument exists, you will use it often and it save you a lot of time!</p>
</div>

* View `pong_data_hits` and enter the number of hits made with the small paddle (50) and the red colour background in this box: <input class='webex-solveme nospaces' size='3' data-answer='["517"]'/>

**Note:**

* The name of the column within `pong_data_hits` is `total_hits`; this is what you called it in the above code. You could have called it anything you wanted but always try to use something sensible.

* Make sure to call your variables something you (and anyone looking at your code) will understand and recognize later (i.e. not variable1, variable2, variable3. etc.), and avoid spaces (use_underscores_never_spaces). 


<div class="try">
<p>After grouping data together using the <code>group_by()</code> function and then performing a task on it, e.g. <code>filter()</code>, it can be very good practice to ungroup the data before performing another function. Forgetting to ungroup the dataset won't always affect further processing, but can really mess up other things. Again just a good reminder to always check the data you are getting out of a function a) makes sense and b) is what you expect.</p>
</div>
