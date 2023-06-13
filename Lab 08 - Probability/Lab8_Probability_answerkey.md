Lab 8: Probability
================
Constanza F. Schibber
November 2, 2017

# Agenda

1.  A die and a weighted die
2.  A deck of playing cards

# Rolling Dice

## Rolling a Die

We can simulate rolling a die with . Below we (1) create a die, (2) roll
a die, (3) roll a pair of dice.

``` r
#Create a die
die <- 1:6
die
```

    ## [1] 1 2 3 4 5 6

``` r
mean(die)
```

    ## [1] 3.5

``` r
sum(die)
```

    ## [1] 21

``` r
#Simulate rolling a die with R's sample function
sample(x = die, size = 1)
```

    ## [1] 5

``` r
#Let's roll again a die once. Do you get the same result?
sample(x = die, size = 1)
```

    ## [1] 2

``` r
#We can write a function for rolling a die 
roll1 <- function ( ) {
  die <- 1:6
  dice <- sample(die, size = 1)
  return(dice)
  }

roll1() # what did you get when you rolled a die?
```

    ## [1] 3

``` r
roll1() # what did you get this time?
```

    ## [1] 4

We know that the probability of rolling each side of the die is
$\frac{1}{6}=0.1666667$

What happens if you run several experiments by rolling a die a number of
times?

``` r
### Rolling a die 10 times ###

#The probability of getting "2" in rolling a die is 1/6. Can you show this?  
rolls10 <- replicate(10, roll1())

#See what you got in your ten trials
rolls10
```

    ##  [1] 2 4 5 6 2 6 3 4 1 1

``` r
# we can build a table in which we get the frequency of rolling a 1, a 2, and so on
table(rolls10)   ##Make a frequency table
```

    ## rolls10
    ## 1 2 3 4 5 6 
    ## 2 2 1 2 1 2

``` r
# we can build a histogram in which y-axis is the frequency for rolling each side of the die
hist(rolls10)    
```

![](Lab8_Probability_answerkey_files/figure-gfm/Rolling%20a%20die%2010,%20100,%20and%20100,000%20times-1.png)<!-- -->

``` r
# We can also do a barplot, which is better because x-axis is discrete, but we have to put table inside to give barplot the frequencies

barplot(table(rolls10), col='deeppink')
```

![](Lab8_Probability_answerkey_files/figure-gfm/Rolling%20a%20die%2010,%20100,%20and%20100,000%20times-2.png)<!-- -->

``` r
### Rolling a die 100 times ###

# What happens when we increase the number of trials/experiments to a 100?

rolls100 <- replicate(100, roll1())   
barplot(table(rolls100), col='lightblue')
```

![](Lab8_Probability_answerkey_files/figure-gfm/Rolling%20a%20die%2010,%20100,%20and%20100,000%20times-3.png)<!-- -->

``` r
table(rolls100)
```

    ## rolls100
    ##  1  2  3  4  5  6 
    ## 12 14 21 17 18 18

``` r
# We can calculate the ``proportions" we go (frequency of rolling each side divided by the number of rolls)

# Step 1: Save the table as a data frame into an object
tb100 <- as.data.frame(table(rolls100))
tb100
```

    ##   rolls100 Freq
    ## 1        1   12
    ## 2        2   14
    ## 3        3   21
    ## 4        4   17
    ## 5        5   18
    ## 6        6   18

``` r
dim(tb100)
```

    ## [1] 6 2

``` r
# Step 2: Calculate the proportions. We divide by 100 because we rolled the die 100 times. We save the information in a column 'prop' to the data frame.
tb100$prop <- tb100[,2]/100
tb100
```

    ##   rolls100 Freq prop
    ## 1        1   12 0.12
    ## 2        2   14 0.14
    ## 3        3   21 0.21
    ## 4        4   17 0.17
    ## 5        5   18 0.18
    ## 6        6   18 0.18

``` r
### Rolling a die 100,000 times ###

# What happens when we increase the number of trials/experiments to 100,000?
rolls100000 <- replicate(100000, roll1())

# Look at the first 6 rolls (don't even try to look at all of them!)
head(rolls100000)
```

    ## [1] 5 5 5 5 6 5

``` r
# we can build a table in which we get the frequency of rolling a 1, a 2, and so on
table(rolls100000)
```

    ## rolls100000
    ##     1     2     3     4     5     6 
    ## 16631 16723 16714 16582 16696 16654

``` r
# we can build a barplot in which y-axis is the frequency for rolling each side of the die
barplot(table(rolls100000), col='lightgreen')
```

![](Lab8_Probability_answerkey_files/figure-gfm/Rolling%20a%20die%2010,%20100,%20and%20100,000%20times-4.png)<!-- -->

``` r
#As the number of trials increases, the proportion (frequency for rolling each side over the number of rolls) approaches to the probability (Law of Large Numbers)

tb <- as.data.frame(table(rolls100000))
dim(tb)
```

    ## [1] 6 2

``` r
head(tb)
```

    ##   rolls100000  Freq
    ## 1           1 16631
    ## 2           2 16723
    ## 3           3 16714
    ## 4           4 16582
    ## 5           5 16696
    ## 6           6 16654

``` r
tb$prop <- tb[,2]/100000
tb
```

    ##   rolls100000  Freq    prop
    ## 1           1 16631 0.16631
    ## 2           2 16723 0.16723
    ## 3           3 16714 0.16714
    ## 4           4 16582 0.16582
    ## 5           5 16696 0.16696
    ## 6           6 16654 0.16654

## Rolling Two Dice

Let’s consider the case of rolling a pair of dice. Now to the function
`sample` we have to add `replace=TRUE`. This is because, we want to roll
two dice, but this is equivalent to rolling a die, writing down the
number we got, and then rolling it again. Because this is a die, the
second time around we can get sides 1 through 6 again.

``` r
#Simulate rolling a pair of dice
#"with replacement"
dice <- sample(x = die, size = 2, replace=TRUE)
dice
```

    ## [1] 6 6

``` r
#What is the sum of the numbers you get?
sum(dice)
```

    ## [1] 12

``` r
# Writing a generic function for rolling x number of dice
rollx <- function(x) {
  die <- 1:6
  dice <- sample(die, size = x, replace=TRUE)
  return(as.vector(dice)) # Now we have a vector of length x
}

# Let's roll 2 dice
rollx(2)
```

    ## [1] 3 1

Calculate the following probabilities by hand:

- Probability of rolling 1-1
- Probability of rolling either a 1-2 or a 2-1
- Probability of rolling a combination of prime numbers.

Now, calculate the proportions for each question when we roll the dice
100,000 times.

Tip: Because the output of `rollx` is a vector, when using `replicate`
you are going to run into problems. You will have to use `as.matrix` and
`t`.

``` r
# rolling 2 dice 100,000 times
rolls2 <- as.matrix(t(replicate(100000, rollx(2))))   
head(rolls2)
```

    ##      [,1] [,2]
    ## [1,]    2    5
    ## [2,]    2    6
    ## [3,]    2    6
    ## [4,]    4    4
    ## [5,]    4    1
    ## [6,]    4    1

``` r
#Save as data frame for easier manipulation
rolls2<-as.data.frame(rolls2)

################
# rolling 1-1 #
###############

# How many 1-1 rolls do we have in rolls2?
rolls2$Roll11<-NA
for(i in 1:nrow(rolls2)){  
  rolls2[i, 3]<-ifelse(rolls2[i,1]==1 & rolls2[i,2]==1, "1", "0")
}

# Take a look at the first rows

head(rolls2)
```

    ##   V1 V2 Roll11
    ## 1  2  5      0
    ## 2  2  6      0
    ## 3  2  6      0
    ## 4  4  4      0
    ## 5  4  1      0
    ## 6  4  1      0

``` r
# How many 1-1 did we get?
sum(as.numeric(rolls2$Roll11))
```

    ## [1] 2807

``` r
# What is the proportion of 1-1?
sum(as.numeric(rolls2$Roll11))/nrow(rolls2)
```

    ## [1] 0.02807

``` r
########################
# rolling 1-2 or a 2-1 #
#######################

# How many 1-2, 2-1 rolls do we have in rolls2?
rolls2$Roll21<-NA

for(i in 1:nrow(rolls2)){  
  rolls2[i, 4]<-ifelse(rolls2[i,1]==1 & rolls2[i,2]==2 | rolls2[i,1]==2 & rolls2[i,2]==1, "1", "0")
}

# Take a look at the first rows

head(rolls2)
```

    ##   V1 V2 Roll11 Roll21
    ## 1  2  5      0      0
    ## 2  2  6      0      0
    ## 3  2  6      0      0
    ## 4  4  4      0      0
    ## 5  4  1      0      0
    ## 6  4  1      0      0

``` r
# How many 1-2 or 2-1 did we get?
sum(as.numeric(rolls2$Roll21))
```

    ## [1] 5451

``` r
# What is the proportion of 1-1?
sum(as.numeric(rolls2$Roll21))/nrow(rolls2)
```

    ## [1] 0.05451

``` r
###################################
# rolling combinations of 1, 3, 5 #
##################################

rolls2$Rollprime<-NA

for(i in 1:nrow(rolls2)){  
  rolls2[i, 5]<-ifelse((rolls2[i,1]==1 | rolls2[i,1]==3 | rolls2[i,1]==5) & 
                         (rolls2[i,2]==1 | rolls2[i,2]==3 | rolls2[i,2]==5),
                         "1", "0")
}

# Take a look at the first rows
head(rolls2)
```

    ##   V1 V2 Roll11 Roll21 Rollprime
    ## 1  2  5      0      0         0
    ## 2  2  6      0      0         0
    ## 3  2  6      0      0         0
    ## 4  4  4      0      0         0
    ## 5  4  1      0      0         0
    ## 6  4  1      0      0         0

``` r
# How many prime combinations did we get?
sum(as.numeric(rolls2$Rollprime))
```

    ## [1] 24902

``` r
# What is the proportion?
sum(as.numeric(rolls2$Rollprime))/nrow(rolls2)
```

    ## [1] 0.24902

## Rolling dice and looking at the sum

In class we did an example in which we were interested in rolling 2 dice
and finding the probability that the outcome would add up to 5. We found
that the probability was $\frac{4}{36}$ or $\frac{1}{9}$. Let’s do the
same in R!

``` r
#We can write a function for rolling a pair of dice and returning a sum of the numbers 
rollxsum <- function (x) {
  die <- 1:6
  dice <- sample(die, size = x, replace=TRUE)
  return(sum(dice))
}

rollxsum(2) # what did you get when you rolled a pair of dice?
```

    ## [1] 4

``` r
rollxsum(2) # what did you get this time?
```

    ## [1] 10

``` r
#Make a histogram of your results when you repeat rolling a pair of dice for 100,000 times 
rolls4 <- replicate(100000, rollxsum(2))    
barplot(table(rolls4))
```

![](Lab8_Probability_answerkey_files/figure-gfm/rolling%20dice%20and%20summing%20the%20output-1.png)<!-- -->

``` r
table(rolls4)
```

    ## rolls4
    ##     2     3     4     5     6     7     8     9    10    11    12 
    ##  2840  5536  8258 11116 13978 16555 13784 11078  8405  5635  2815

``` r
#We can add proportion for each event
tb.rolls4 <- as.data.frame(table(rolls4))
tb.rolls4$prop <- tb.rolls4[,2]/100000
tb.rolls4
```

    ##    rolls4  Freq    prop
    ## 1       2  2840 0.02840
    ## 2       3  5536 0.05536
    ## 3       4  8258 0.08258
    ## 4       5 11116 0.11116
    ## 5       6 13978 0.13978
    ## 6       7 16555 0.16555
    ## 7       8 13784 0.13784
    ## 8       9 11078 0.11078
    ## 9      10  8405 0.08405
    ## 10     11  5635 0.05635
    ## 11     12  2815 0.02815

What if we had 3 dice? If we roll 3 dice, we have $6^3=216$ possible
outcomes. If we sum the outcome, the smallest possible sum would be 3
(if the 3 dice are 1, 1, 1) and the largest possible sum would be 18 (if
the 3 dice are 6, 6, 6).

Which possible sum is the most likely?

``` r
rolls5 <- replicate(1000000, rollxsum(3))    
barplot(table(rolls5))
```

![](Lab8_Probability_answerkey_files/figure-gfm/rolling%203%20dice%20ANSWER-1.png)<!-- -->

``` r
table(rolls5)
```

    ## rolls5
    ##      3      4      5      6      7      8      9     10     11     12     13 
    ##   4650  13887  28034  46180  69702  96775 115559 125394 125088 115446  97299 
    ##     14     15     16     17     18 
    ##  69517  46097  28008  13760   4604

``` r
#We can add proportion for each event
tb.rolls5 <- as.data.frame(table(rolls5))
tb.rolls5$prop <- tb.rolls5[,2]/1000000
tb.rolls5
```

    ##    rolls5   Freq     prop
    ## 1       3   4650 0.004650
    ## 2       4  13887 0.013887
    ## 3       5  28034 0.028034
    ## 4       6  46180 0.046180
    ## 5       7  69702 0.069702
    ## 6       8  96775 0.096775
    ## 7       9 115559 0.115559
    ## 8      10 125394 0.125394
    ## 9      11 125088 0.125088
    ## 10     12 115446 0.115446
    ## 11     13  97299 0.097299
    ## 12     14  69517 0.069517
    ## 13     15  46097 0.046097
    ## 14     16  28008 0.028008
    ## 15     17  13760 0.013760
    ## 16     18   4604 0.004604

``` r
# we might need to increase the replications because the sum 10 and 11 are very veryclose. But even if you increase to 1,000,000 they get even closer (0.125100 and 0.125459).That is because the probability is the same for both of them. See here: https://www.thoughtco.com/probabilities-for-rolling-three-dice-3126558
```

## A Curious Coin-Flipping Game

The following challenge was set in the August-September 1941 issue of
American Mathematical Monthly and was not solved until 1966. That is 25
years later!

The problem as stated in the journal was:

Three men have respectively l, m, and n number of coins which they match
so that the odd man wins. In case all coins appear alike they repeat the
throw. Find the average number of tosses required until one man is
forced out of the game.

As stated, we know the following:

- Coins are fair
- When playing, each person selects one of her/his coins.
- The three players throw their coins at the same time.
- If all the coins are equal, they pick another coin and they throw
  again.
- If two coins are equal and one is not, those with the matched coins
  give her/his coin to the odd man out. This means that two people loose
  one coin. The third person wins two coins.
- If someone does not have any coins left, that person is ruined and out
  of the game.

Write a simulation that accepts valus for l, m, and n, and then plays a
large number of tosses sequences, keeping track for each sequence of the
number of tosses until one of the men goes broke/ruined. From that, the
average number of tosses is easy to calculate.

``` r
# Your answer here
```

``` r
# flip 3 coins
coin.flip<-function(){
   coin<-c("H", "T")
   flip<-sample(coin, size=3, replace = TRUE)
   return(flip)
}

# Write a function as a function of the number of coins each player has
coin.game<-function(l, m, n){
  # flip the 3 coins
  flip<-coin.flip()
  # if all coins are NOT equal to each other
  if(length(unique(flip)) != 1){ 
     if(flip[1]==flip[2]){
      new.l<-l-1
      new.m<-m-1
      new.n<-n+2}
     else{
    if(flip[1]==flip[3]){
      new.l<-l-1
      new.m<-m+2
      new.n<-n-1}
       else{
      new.l<-l+2
      new.m<-m-1
      new.n<-n-1}
     }
  return(cbind(new.l, new.m, new.n))
  }
  return(cbind(l, m, n))
  
 }

# play one, each player has 2 coins
coin.game(2,2,2)
```

    ##      new.l new.m new.n
    ## [1,]     1     1     4

``` r
# Repeat Game
repeat.game<-function(l, m, n){
  # Set starting values
  i<-0
  new.l<-l
  new.m<-m
  new.n<-n
  # Repeat until Break
  repeat{
    outcome<-coin.game(new.l, new.m, new.n)
#    print(outcome)
    new.l<-outcome[1]
    new.m<-outcome[2]
    new.n<-outcome[3]
    i<-i+1
    if(new.l==0 | new.m==0 | new.n==0){
 #     print(outcome)
      return(i)
      break
    }
  }
}

# Play a lot of times

m<-2
n<-4
l<-8

#list.of.players<-matrix(NA, ncol=4) 
#colnames(list.of.players)<-c("PlayerL", "PlayerM", "PlayerN", "iter")
start_time <- Sys.time()
iter<-as.matrix(replicate(100000, repeat.game(3, 4, 5)), ncol=1)
end_time <- Sys.time()
end_time - start_time
```

    ## Time difference of 11.49769 secs

``` r
hist(iter, breaks=30)
```

![](Lab8_Probability_answerkey_files/figure-gfm/A%20Curious%20Coin-Flipping%20Game%20ANSWER-1.png)<!-- -->

``` r
summary(iter)
```

    ##        V1        
    ##  Min.   : 3.000  
    ##  1st Qu.: 4.000  
    ##  Median : 6.000  
    ##  Mean   : 8.013  
    ##  3rd Qu.:10.000  
    ##  Max.   :81.000

# A Deck of 52 cards

We can also simulate to “shuffle” or “deal” a deck of playing cards. We
are going to load the data that was created by the author (Grolemund) as
a CSV file. Please download the data to your computer and load it.

``` r
#Download and read the data (Use a correct path to the file)
#Name the file "deck"
deck <- read.csv("deck.csv")
#View(deck)
head(deck)
```

    ##    face   suit value
    ## 1  king spades    13
    ## 2 queen spades    12
    ## 3  jack spades    11
    ## 4   ten spades    10
    ## 5  nine spades     9
    ## 6 eight spades     8

``` r
#How many cards are "queen"? 
deck$face   # extract the face column 
```

    ##  [1] "king"  "queen" "jack"  "ten"   "nine"  "eight" "seven" "six"   "five" 
    ## [10] "four"  "three" "two"   "ace"   "king"  "queen" "jack"  "ten"   "nine" 
    ## [19] "eight" "seven" "six"   "five"  "four"  "three" "two"   "ace"   "king" 
    ## [28] "queen" "jack"  "ten"   "nine"  "eight" "seven" "six"   "five"  "four" 
    ## [37] "three" "two"   "ace"   "king"  "queen" "jack"  "ten"   "nine"  "eight"
    ## [46] "seven" "six"   "five"  "four"  "three" "two"   "ace"

``` r
deck$face=="queen"    #Is each value in the face column equal to "queen"?
```

    ##  [1] FALSE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
    ## [13] FALSE FALSE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
    ## [25] FALSE FALSE FALSE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
    ## [37] FALSE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
    ## [49] FALSE FALSE FALSE FALSE

``` r
sum(deck$face=="queen")   # How many cards are equal to "queen"?
```

    ## [1] 4

``` r
#What is the value of a card "queen"? 
deck$value[deck$face=="queen"]
```

    ## [1] 12 12 12 12

``` r
-#What is the value of a "queen" of "hearts" card?
deck$value[deck$face=="queen" & deck$suit=="hearts"]
```

    ## [1] -12

Imagine a deck of playing cards (which is piled in some orders) as a
data frame in . When we `deal`, we pick the top card. Picking the first
card from the deck is the same as picking the card in the first row of
the data frame in .

Once we take a card, we look at it and we put it back in the deck.

Also, we can `shuffle` a (real) deck of cards by randomy rearranging the
order of the cards. To do that with our virtual cards in , we randomly
reorder the rows in the data frame.

``` r
#Deal - We give out the first card on the deck

deal <- function(cards){
  cards[1,]
  }
deal(deck)
```

    ##   face   suit value
    ## 1 king spades    13

``` r
#Different types of shuffle 

#In R we can reorder the rows, by creating another data frame and specifying the order of rows. "deck1" has the same order of the rows in "deck." We are simply extracting every row in the data frame.  
deck1 <- deck[1:52,]

#Let's shuffle it. For example, we could ask for row 2, then row 1, and then the rest of the cards. That is, we create a "deck2" where the order of rows is R2, R1, R3, R4, ... , R52 (of the original deck)
deck2 <- deck[c(2, 1, 3:52),] 

#For a random shuffle, we can create a row index using a random numbers drawn from 1 to 52, then use it do create another deck.
random <- sample(1:52, size = 52)
random
```

    ##  [1] 48 14 41 46 29 42 28 18 47 31 12 27 37 30 25 16 44 40 13 34 15 38  9 35 32
    ## [26] 52 20 11 23  8  7 43 19  1  5 24  2 33 17 51  6  4 49 45  3 39 10 22 26 21
    ## [51] 36 50

``` r
deck3 <- deck[random,]
deck3
```

    ##     face     suit value
    ## 48  five   hearts     5
    ## 14  king    clubs    13
    ## 41 queen   hearts    12
    ## 46 seven   hearts     7
    ## 29  jack diamonds    11
    ## 42  jack   hearts    11
    ## 28 queen diamonds    12
    ## 18  nine    clubs     9
    ## 47   six   hearts     6
    ## 31  nine diamonds     9
    ## 12   two   spades     2
    ## 27  king diamonds    13
    ## 37 three diamonds     3
    ## 30   ten diamonds    10
    ## 25   two    clubs     2
    ## 16  jack    clubs    11
    ## 44  nine   hearts     9
    ## 40  king   hearts    13
    ## 13   ace   spades     1
    ## 34   six diamonds     6
    ## 15 queen    clubs    12
    ## 38   two diamonds     2
    ## 9   five   spades     5
    ## 35  five diamonds     5
    ## 32 eight diamonds     8
    ## 52   ace   hearts     1
    ## 20 seven    clubs     7
    ## 11 three   spades     3
    ## 23  four    clubs     4
    ## 8    six   spades     6
    ## 7  seven   spades     7
    ## 43   ten   hearts    10
    ## 19 eight    clubs     8
    ## 1   king   spades    13
    ## 5   nine   spades     9
    ## 24 three    clubs     3
    ## 2  queen   spades    12
    ## 33 seven diamonds     7
    ## 17   ten    clubs    10
    ## 51   two   hearts     2
    ## 6  eight   spades     8
    ## 4    ten   spades    10
    ## 49  four   hearts     4
    ## 45 eight   hearts     8
    ## 3   jack   spades    11
    ## 39   ace diamonds     1
    ## 10  four   spades     4
    ## 22  five    clubs     5
    ## 26   ace    clubs     1
    ## 21   six    clubs     6
    ## 36  four diamonds     4
    ## 50 three   hearts     3

``` r
# Writing a function to shuffle the cards
shuffle <- function(cards) {
  random <- sample(1:52, size = 52)
  cards[random, ]
}

#Now we can shuffle your cards between each deal
deal(deck)
```

    ##   face   suit value
    ## 1 king spades    13

``` r
deck4 <- shuffle(deck)
deal(deck4)
```

    ##     face  suit value
    ## 20 seven clubs     7

``` r
deck5 <- shuffle(deck)
deal(deck5)
```

    ##     face     suit value
    ## 33 seven diamonds     7

``` r
# We create different objects called deck. If you overwrite the original deck and make a mistake, you would have to reaload the deck
```

We we can calculate some probabilities and then, look at the proportions
in R.

## What is the probability of picking the card, Queen of Hearts?

``` r
#Queen & Heart is a unique card in the deck
summary(deck$face=="queen" & deck$suit=="hearts")
```

    ##    Mode   FALSE    TRUE 
    ## logical      51       1

``` r
#We can write a function for shuffling a deck of playing cards and picking one randomly. We will do this 10^6 times.
#How many times do we get a queen of hearts card?

n<-10^6
QueenOfHearts <- rep(NA, n)

pick1 <- function (cards, n) {
    for (i in 1:n){  
      #shuffle the deck using "shuffle" function we created earlier
      cards<- shuffle(cards)  
      # if the first one in the deck is a queen of hearts, 1; else, 0 
      ifelse(cards[1,1]=="queen" & cards[1,2]=="hearts", QueenOfHearts[i]<- 1, QueenOfHearts[i] <-0)
    }
  }
#Note: It takes some time. Check this:

ex1 <- pick1(deck, n)
ex1
```

    ## NULL

``` r
#What is the proportion? I.e, How many times do we get a queen of hearts, out of 10^6 trials
#19330/ (10^6)
ex1/(10^6)
```

    ## numeric(0)

``` r
#Is it similar to the probability of this event? 
1/52
```

    ## [1] 0.01923077

``` r
all.equal(19330/(10^6), 1/52)
```

    ## [1] "Mean relative difference: 0.005133511"

## What is the probability of picking a Queen?

``` r
summary(deck$suit=="queen")
```

    ##    Mode   FALSE 
    ## logical      52

``` r
#We can write a function for shuffling a deck of playing cards and picking one randomly  
#How many times do we get a queen?

pick2 <- function (cards) {
  t <- rep(NA, 10^6)
  i <- 1
      for (i in 1:10^6){  
         cards<- shuffle(cards)  #shuffle the deck using "suffle" funciton made above
      # if the first one in the deck is a queen of hearts, 1; else, 0 
         if (cards[1,1]=="queen") { t[i]<- 1 } else {t[i] <-0}
    }
  sum(t)
  }

ex2 <- pick2(deck)
ex2
```

    ## [1] 76531

``` r
#What is the proportion? I.e, How many times do we get a queen of hearts, out of 10^6 trials
# 76718 /(10^6)
ex2/ (10^6)
```

    ## [1] 0.076531

``` r
#Is it similar to the probability of this event? 
4/52
```

    ## [1] 0.07692308

``` r
all.equal(76718/(10^6), 4/52)
```

    ## [1] "Mean relative difference: 0.002673127"

## What is the probability of picking a Queen in 5 consecutive cards?

Solve!
