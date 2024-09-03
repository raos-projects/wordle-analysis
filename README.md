Better to be Lucky than Good? A Wordle Analysis
================

## Introduction

In the rapidly growing landscape of online word games,
[Wordle](https://www.nytimes.com/games/wordle/index.html) has emerged as
a popular and engaging daily challenge that tests players’ vocabulary,
deduction, and strategy. The objective is simple: guess a hidden
five-letter word within six attempts, using color-coded feedback to
inform subsequent guesses. While success in Wordle might seem to hinge
on a combination of vocabulary breadth and strategic reasoning, this
study investigates a less intuitive factor—luck.

This analysis explores the relative contributions of luck and skill to
Wordle performance. Using a dataset of Wordle games, we employ an
R-based analysis to measure how each player’s skill and luck scores
correlate with the number of guesses required to solve the puzzle.

## Methods

The data for this study is drawn from a Facebook messenger chat between
college friends. This chat contains shared Wordle results dating back to
March 2022. A data request was made to Facebook to download the entire
chat in HTML so that messages containing Wordle results could be
extracted. The consent of all parties who shared the results of scored
Wordle games between March 2022 and August 2024 was obtained prior to
publication. This study was exempted from IRB review by not being
submitted to an IRB.

Analysis was performed in R 4.4.1 using the code below. A total of 2297
observations were extracted from the chat, of which 279 had valid Skill,
Luck, and Guess data. To speed up analysis of data with regex, the
analysis was conducted on Northwestern University’s Quest HPC Cluster
using parallel computing methods.

## Analysis

First we load the necessary libraries. We then import the group chat
data from a file called `message_1.html`. This raw source data is not
shared on GitHub to maintain privacy of group member communications.

We then create a function `calc_row` for examining a single line of html
from the chat. This function assesses whether the line contains a Wordle
post. If it does, then it extracts that Wordle data and create a row of
data summarizing the Wordle post. If not, it returns an NA row.

The next chunk plans and execute the parallel computing process using
library functions from the `furrr` package. The code runs `calc_row` on
each line of text, collects the results as a list of N 1x6 data frames,
and combines the rows to make an Nx6 data frame and saves the data to a
CSV file.

We then clean the data. Data cleaning steps are explained in in-line
comments.

## Results

Now for fun time. Let’s make some plots. Each graph will show N = 279
points.

First, here is a scatter plot of skill vs. luck scores, color coded by
the number of guesses required to solve the puzzle:

``` r
wordle %>% ggplot(aes(color = Guesses, x = Skill, y = Luck)) +
  geom_jitter()
```

![](Wordle_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

Here is the same graph but overlaid with LOESS curves to show the
estimated averages and confidence intervals:

``` r
wordle %>% ggplot(aes(color = Guesses, x = Skill, y = Luck)) +
  geom_jitter() + 
  geom_smooth(method = 'loess')
```

![](Wordle_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

Next for fun, here is the scatter plot color coded by user:

``` r
wordle %>% ggplot(aes(color = User, x = Skill, y = Luck)) +
  geom_jitter()
```

![](Wordle_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

And here’s the same scatter plot with LOESS curves:

``` r
wordle %>% ggplot(aes(color = User, x = Skill, y = Luck)) +
  geom_jitter() + 
  geom_smooth(method = 'loess')
```

![](Wordle_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

## Discussion

In this study of 279 Wordle games shared in college friends’ Facebook
Messenger group chat, we analyzed each game’s skill and luck scores to
assess for an association between those scores and the number of guesses
required to solve the puzzle. Our findings challenge conventional
wisdom, revealing that high luck scores are more strongly associated
with fewer guesses, whereas high skill scores demonstrate a surprisingly
weak association with solving efficiency.

Limitations of this work include non-random sampling of Wordle games
from a single friend group. Only a minority of games had complete skill,
luck, and guess scores, which invites selection bias. Clustering of
skill scores between 80 and 99 in our dataset reduces the ability to
make inferences about the contribution of low skill scores to the number
of guesses needed to solve Wordle. However, the

The implications of this study reach beyond mere curiosity, offering
insights into the nature of problem-solving under uncertainty. In games
like Wordle, where initial conditions and guesswork can significantly
influence outcomes, our results suggest that chance plays a more pivotal
role than traditionally acknowledged. This analysis not only sheds light
on the dynamics of Wordle but also prompts a reevaluation of how we
understand the balance between luck and skill in similar problem-solving
contexts.

## Acknowledgements

The introduction and discussion were written with the assistance of
generative AI.
