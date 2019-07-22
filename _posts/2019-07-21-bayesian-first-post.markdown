---
layout: post
title:  Bayesian first post
date:   2019-07-21 19:40:18 -0500
categories: bayes math python
---

I've started reading a book called [Data Science from
Scratch](https://www.amazon.com/Data-Science-Scratch-Principles-Python/dp/149190142X)
by Joel Grus. The book does go a bit into probability and statistics
theory. Early on we are introduced to the Bayes theorem (which the
author calls _One of the data scientist's best friends_). I decided to
look some problems involving bayes theorem online and solve them for
practice (and to have an execuse to code a bit of python too).

## Problem 1

> There are two jars of marbles. In the first jar, 75 percent of the
> marbles are red and 25 percent are non-red; in the second jar, 40
> percent are red and 60 percent are non-red. One dice is going to be
> rolled. If the dice lands on one, then a marble is taken from the
> first jar; if the dice lands on any other number, then a marble is
> taken from the second jar.

> 1a. A marble has been taken from one of the two jars, and it is red. How likely is it
> came from the first jar?

Let's tackle the problem with mathematics first.

Let the event _R_ be the event that the marble chosen at random is
Red. Similarly, the event _~R_ will be the event that the marble
chosen is not red.

Let the event _F_ be the event that the marble chosen at random comes
from the first jar, while _~F_ is its complement, the event indicating
that the marble does not come from the first jar (i.e. it comes from
the seconds jar).

The problem gives us the following information:

- P(R\|F) = 0.75
- P(~R\|F) = 0.25
- P(R\|F) = 0.40
- P(R\|~F) = 0.60
- P(F) = 1/6
- P(~F) = 5/6

This information is sufficent to solve question 1a. as it follows that

P(F\|R) = P(R\|F)P(F) / ( P(R\|F)P(F) +  P(R\|~F)P(~F) )

P(F\|R) = (0.75)(1/6) / ( (0.75)(1/6) + (0.40)(5/6) )

P(F\|R) = 0.272727272727

Since the problems don't come with solutions I'll do a short python
script to look at an approximation.

{% highlight python %}
import random

RED = 'R'
NON_RED = 'O'

first_jar = RED*75+NON_RED*25
second_jar = RED*40+NON_RED*60

n_trials = 100000

first_jar_count = 0
reds_count = 0

for _ in range(n_trials):
    die = random.choice(range(1, 7))
    chosen_marble = random.choice(first_jar if die == 1 else second_jar)

    if chosen_marble == RED:
        reds_count += 1
        if die == 1:
            first_jar_count += 1

print('{} from first jar, {} reds, P(F|R) = {}'.format(
    first_jar_count, reds_count, float(first_jar_count) / reds_count))
{% endhighlight %}

Which when run yields the following output

```
12572 from first jar, 45994 reds, P(F|R) = 0.27334000087
```

Seems about right.

> 1b. How likely is it that this marble came from the second jar?

Following a similar logic as in the fist problem we have that

P(~F\|R) = P(R\|~F)P(~F) / ( P(R\|~F)P(~F) +  P(R\|F)P(F) )

P(~F\|R) = (0.40)(5/6) / ( (0.40)(5/6) + (0.75)(1/6) )

P(~F\|R) = 0.727272727273

Which of course is just the complement of `P(F|R)`.

We can modify the previous script to double check the answer to this
problem:


{% highlight python %}
import random

RED = 'R'
NON_RED = 'O'

first_jar = RED*75+NON_RED*25
second_jar = RED*40+NON_RED*60

n_trials = 100000

second_jar_count = 0
reds_count = 0

for _ in range(n_trials):
    die = random.choice(range(1, 7))
    chosen_marble = random.choice(first_jar if die == 1 else second_jar)

    if chosen_marble == RED:
        reds_count += 1
        if die != 1:
            second_jar_count += 1

print('{} from second jar, {} reds, P(~F|R) = {}'.format(
    second_jar_count, reds_count, float(second_jar_count) / reds_count))
{% endhighlight %}

which yields

```
33603 from second jar, 46100 reds, P(~F|R) = 0.728915401302
```

About right once again.
