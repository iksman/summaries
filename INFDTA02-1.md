#       INFDTA02-1 Data Science 1

#       Lecture 1

-   Collaborative filtering
    1.  Identify the ratings of a target user
    2.  Identify the users most similar to him (according to a similarity function) => neighbour formation
    3.  Identify the products bought by these similar users
    4.  Generate a prediction (of the rating given by the target user) for each of these products
    5.  Recommend a set of top N products

-   Possible formulas for similarity
    -   Euclidian and Manhattan distances
    -   Pearson correlation coefficient
    -   Cosine similarity

##      Euclidian distance 
>   **Remember by hard for the exam!**

Length of a line connecting two points

![Imgur](https://i.imgur.com/W8typHn.png)

```
Coordinates	(p1, p2, pn...), (q1, q2, qn...)
Distance	d(p,q) = sqrt((p1 - q1)^2 + (p2 - q2)^2 + (pn - qn)^2 ...)
```

###     Example 1
```
p = (2,4,1)
q = (0,3,2)
d(p,q) = sqrt((2 - 0)^2 + (4 - 3)^2 + (1 - 2)^2)
d(p,q) = sqrt(6)
d(p,q) = 2.45
```

###     Example 2
| User | Snow Crash | Dragon Tattoo |
|:-----|:----------:|:-------------:|
| Amy  | 5          | 5             |
| Bill | 2          | 5             |
| Jim  | 1          | 4             |
| X    | 4          | 2             |

Euclidian distance between X and each user and which user is the most similar to X?
```
d(Amy,X) = sqrt((5-4)^2 + (5-2)^2) = sqrt(10)
d(Bill,X) = sqrt((2-4)^2 + (5-2)^2) = sqrt(13)
d(Jim,X) = sqrt((1-4)^2 + (4-2)^2) = sqrt(13)
```
> Amy is the most similar to X

##      Manhattan distance
Distance that would be travelled if a grid like path is followed

![Imgur](https://i.imgur.com/fVgZOBb.png)
```
Coordinates (p1, p2, pn ...), (q1, q2, qn ...)
Distance    d(p,q) = |p1 - q1| + |p2 + q2| + |pn + qn| ...
```
>|x - y| means that the outcome will always be positive

>e.g. `|0 - 4| === 4` and not `-4`

###     Example 1
```
p = (2,4,1)
q = (0,3,2)
d(p,q) = |2-0|+|4-3|+|1-2|
d(p,q) = 2+1+1
d(p,q) = 4
```

###     Example 2
| User | Snow Crash | Dragon Tattoo |
|:-----|:----------:|:-------------:|
| Amy  | 5          | 5             |
| Bill | 2          | 5             |
| Jim  | 1          | 4             |
| X    | 4          | 2             |

Manhattan distance between X and each user and which user is the most similar to X?
```
d(Amy,X) = |5-4|+|5-2| = 4
d(Bill,X) = |2-4|+|5-2| = 5
d(Jim,X) = |1-4|+|4-2| = 5
```
> Amy is the most similar to X

##      Problems
###     Problem 1
| User | Item 1     | Item 2        | Item 3 |
|:-----|:----------:|:-------------:|:------:|
| Amy  | 3.0        | -             | -      |
| Bill | 4.5        | 2.0           | 3.0    |
| X    | 5.0        | 2.5           | 2.0    |

Calculating the distances between users works best without any missing values!
```
d(Amy,X) = 2 (based on 1 comparison)
d(Bill,X) = 2 (based on 3 comparisons)
```

Bill seems more similar than Amy to X but do to the formulas ignoring rows with blanks, this means that both Amy and Bill are evenly similar to X.

###     Problem 2
>   These distances do not have a **maximum** possible value\
>   Measures of **distance**, not of similarity!

```
sim = 1 / (1 + d)
```
-   sim
    -   Measure of similarity
    -   Values between `0` and `1`
        -   Perfect similarity when `d = 0` => `sim = 1`
        -   Denominator gets added with 1 so the sim is always a value between `0` and `1`

##      Pearson Coefficient
>   Formula does not need to be memorized, just the execution

-   Measure of the linear correlation (dependence) between two variables
-   Value between `- 1` and `+ 1`
    -   `- 1` (total negative correlation)
    -   `0` (no correlation)
    -   `+ 1`   (total positive correlation)

![Imgur](https://i.imgur.com/uGowukV.png)

Calculating pearson coefficient between Clara and Robert

| User  | Item 1  | Item 2  | Item 3  | Item 4  | Item 5  |
|:------|:-------:|:-------:|:-------:|:-------:|:-------:|
|Clara|4.75|4.5|5|4.25|4|
|Robert|4|3|5|2|1|

```
r = r1 - r2 / (sqrt(r3x - r4x) * sqrt(r3y - r4y))

r1 = sum(forEach item => (user1Rating * user2Rating))
r2 = sum(ratings of user1) * sum(ratings of user2) / itemAmount
r3N = sum(forEach rating of userN ^ 2)
r4N = (sum(forEach rating of userN))^2 / itemAmount

r1 = (4.75 * 4) + (4.5 * 3) + (5 * 5) + (4.25 * 2) + (4 * 1)
r1 = 19 + 13.5 + 25 + 8.5 + 4
r1 = 70

r2 = (4.75 + 4.5 + 5 + 4.25 + 4) * (4 + 3 + 5 + 2 + 1) / 5
r2 = 22.5 * 15 / 5
r2 = 67.5

r3x = 4.75^2 + 4.5^2 + 5^2 + 4.25^2 + 4^2
r3x = 101.875

r4x = (4.75 + 4.5 + 5 + 4.25 + 4)^2 / 5
r4x = (22.5)^2 / 5
r4x = 101.25

r3y = 4^2 + 3^2 + 5^2 + 2^2 + 1^2
r3y = 55

r4y = (4 + 3 + 5 + 2 + 1)^2 / 5
r4y = (15)^2 / 5
r4y = 45

r = 70 - 67.5 / (sqrt(101.875 - 101.25) * sqrt(55 - 45))
r = 2.5 / 2.5
r = 1

```

##      Cosine similarity

-   Cosine of the angle between two vectors
    -   Values between `-1` and `1`
    -   `+1` => perfect similarity
    -   `-1` => perfect negative similarity

![Imgur](https://i.imgur.com/bfic53G.png)

```
x, y = user with ratings

cos(x,y) = sum(forEach item (xN * yN)) / sqrt(sum(x^2)) * sqrt(sum(y^2))
```

###     Example 1

| User  | Item 1 | Item 2 | Item 3 | Item 4 | Item 5 |
|:------|:------:|:------:|:------:|:------:|:------:|
|Clara|4.75|4.5 |5|4.25|4 |
|Robert|4|3|5|2|1 |

Compute the cosine similarity between Clara and Robert
```
cos(x,y) = x . y / || x || * || y ||

x . y = (4.75 * 4 + 4.5 * 3 + 5 * 5 + 4.25 * 2 + 4 * 1)
x . y = 70

|| x || = sqrt(4.75^2 + 4.5^2 + 5^2 + 4.25^2 + 4^2)
|| x || = sqrt(101.875)
|| x || = 10.093

|| y || = sqrt(4^2 + 3^2 + 5^2 + 2^2 + 1^1)
|| y || = sqrt(55)
|| y || = 7.416

cos(x,y) = 70 / 10.093 * 7.416
cos(x,y) = 0.935
```

###     Back to Problem 1 
Using this knowledge, we can prove that Amy is less similar than Bill to X.

| User | Item 1     | Item 2        | Item 3 |
|:-----|:----------:|:-------------:|:------:|
| Amy  | 3.0        | -             | -      |
| Bill | 4.5        | 2.0           | 3.0    |
| X    | 5.0        | 2.5           | 2.0    |

```
d(Amy,X) = 2 (based on 1 comparison)
s(Amy,X) = 1 / 1 + 2 = 0.33

d(Bill,X) = 2 (based on 3 comparisons)
s(Bill,X) = 1 / 1 + 2 = 0.33

cos(Amy, X) = 0.84
cos(Bill, X) = 0.98
```
> Bill is more similar to X than Amy!

##      Which problem applies to what formula?

-   Pearson Coefficient
    -   Grade inflation
    -   Different users => different scale

-   Cosine Similarity
    -   Data is sparse

-   Manhattan, Euclidean
    -   Data is dense
    -   Almost all attriutes have non-zero values
    -   Magnitude of the values is important

##      RECAP Exercise

Calculate the similarity between Bill and Jim using all four measures learned during this lecture.

| User | A  |  B |  C |  D |  E |
|:-----|:--:|:--:|:--:|:--:|:--:|
| Bill | 5  | -  | 1  | 3  | 5  |
| Jim  | 2  | 5  | 4  | 3  | -  |

Euclidian
```
d(Bill,Jim) = sqrt((5 - 2)^2 + (1 - 4)^2 + (3 - 3)^2)
d(Bill,Jim) = sqrt(9 + 9 + 0)
d(Bill,Jim) = sqrt(18)

s(Bill,Jim) = 1 / 1 + sqrt(18)
s(Bill,Jim) = 0.19
```

Manhattan
```
d(Bill,Jim) = |5 - 2| + |1 - 4| + |3 - 3|
d(Bill,Jim) = 3 + 3 + 0

s(Bill,Jim) = 1 / 1 + 6
s(Bill,Jim) = 0.14
```

Pearson
```
r = r1 - r2 / (sqrt(r3x - r4x) * sqrt(r3y - r4y))

r1 = sum(forEach item => (user1Rating * user2Rating)) (sumXY)
r2 = sum(X) * sum(Y) / itemAmount
r3N = sum(forEach rating of userN ^ 2)
r4N = (sum(forEach rating of userN))^2 / itemAmount

r1 = 5*2 + 1*4 + 3*3
r1 = 23

r2 = (5 + 1 + 3) * (2 + 4 + 3) / 3
r2 = 9 * 9 / 3
r2 = 27

r3X = (5^2 + 1^2 + 3^2)
r3X = 35

r3Y = (2^2 + 4^2 + 3^2)
r3Y = 29

r4X = (5 + 1 + 3)^2 / 3
r4X = 27

r4Y = (2 + 4 + 3)^2 / 3
r4Y = 27

r = 23 - 27 / sqrt(35 - 27) * sqrt(29 - 27)
r = -4 / sqrt(8) * sqrt(2)
r = -1
```

Cosine similarity
```
cos(x,y) = x . y / || x || * || y ||

x . y = (5 * 2 + 0 * 5 + 1 * 4 + 3 * 3 + 5 * 0)
x . y = 23

|| x || = sqrt(5^2 + 0^2 + 1^2 + 3^2 + 5^2)
|| x || = sqrt(60)


|| y || = sqrt(2^2 + 5^2 + 4^2 + 3^2 + 0^1)
|| y || = sqrt(54)

cos(x,y) = 23 / sqrt(60) * sqrt(54)
cos(x,y) = 23 / 56.9
cos(x,y) = 0,4
```

#       Lecture 2

##      User-item: nearest neighbours
-   Do we use the *single* most similar person to a user?
    > No! Any quirk would be used in creating recommendations
    - More data = more accuracy

-   Solution?
    -   Consider the `k` most similar persons
    -   Each of these neighbours will influence the recommendation

How do you determine the `k` nearest neighbours (which have a similarity >= than a predefined threshold `Sm`)?

###     Simple approach
-   For each user `U` (except target `Ut`)
    -   `s = similarity(U,Ut)`
    -   `if s > Sm`
        -   If the list of neighbours is not full yet, insert `U`
        -   Else if the list is already full, but `s` is greater than the lowest similarity in the list, replace the neighbour associated to such lowest similarity with `U`

###     Improved approach
-   For each user `U` (except target `Ut`)
    -   `s = similarity(U,Ut)`
    -   `if s > Sm && U has rated additional items with respect to Ut` (additional items = the user has rated at least one product that the target hasn’t rated)
        -   If the list of neighbours is not full yet, insert u
        -   Else if the list is already full, but `s` is greater than the lowest similarity in the list, replace the neighbour associated to such lowest similarity with `U`
    -   If the neighbours list is full, update the value of the threshold `Sm`
        -   The new `Sm` is the lowest neighbour similarity

-   Which value for k?
    -   Depends on the application (`10`, `25`, `50` ...)

-   Which value for `Sm`?
    -   `0.35` is a good start

##      User-item: computing a prediction
###     Example 1

| User | Spiderman 2 |
|:-----|:-----------:|
| Bill | 4.5 |
| Robert | 5 |
| Clara | 3.5 |

-   Target user for this example is Amy
-   3 nearest neighbours are Bill, Clara and Robert

> Each neighbour should count **PROPORTIONALLY** to their similarity to the target

Total pearson coefficient amount = `2.0` (`0.8` + `0.7` + `0.5`)

Influence weight
-   Bill => `0.5 / 2.0 = 0.25` => `25%`
-   Robert => `0.7 / 2.0 = 0.35` => `35%`
-   Clara => `0.8 / 2.0 = 0.4` => `40%`

| User | Spiderman 2 | Pearson Coefficient | Influence weight |
|:-----|:-----------:|:-------------------:|:----------------:|
| Bill | 4.5 | 0.5 | 0.25 |
| Robert | 5 | 0.7 | 0.35 |
| Clara | 3.5 | 0.8 | 0.4 |

Weighted ratings
```
billW = 0.25 * 4.5 = 1.125
robertW = 0.35 * 5 = 1.75
claraW = 0.4 * 3.5 = 1.4

sum(billW, robertW, claraW) = 1.125 + 1.75 + 1.4 = 4.275
``` 

###     Example 2
Using only the `2 nearest neighbours` (Robert, Clara => highest Pearson coefficient) to Amy

Total pearson coefficients = `1.5`
|User|New influence weights|
|:-|:-:|
|Robert|`0.7 / 1.5 = 46.7%`|
|Clara|`0.8 / 1.5 = 53.3%`|

Weighted ratings
```
robertW = 0.46 * 5 = 2.33
claraW = 0.53 * 3.5 = 1.855

sum(robertW, claraW) = 2.33 + 1.855 = 4.2

The predicted rating is 4.2
``` 

###     Example 3
Computing using new values, all neighbours

| User | Spiderman 2 | Pearson Coefficient |
|:-----|:-----------:|:-------------------:|
| Bill | 5 | 0.3 |
| Robert | 2 | 0.8 |
| Clara | 1.5 | 0.9 |

####    Influence weights
Total pearson coefficient = `0.3 + 0.8 + 0.9 = 2.0`
```
Bill = 0.3 / 2.0 = 15%
Robert = 0.8 / 2.0 = 40%
Clara = 0.9 / 2.0 = 45%
```

####    Weighted ratings
```
Bill = 5 * 0.15 = 0.75
Robert = 2 * 0.4 = 0.8
Clara = 1.5 * 0.45 = 0.675

sum of all weighted ratings = 0.75 + 0.8 + 0.675 = 2.2

The predicted rating is 2.2
```

##      Weighted average
Another way of computing the predicted rating of item `i` is doing the **weighted average** of all neighbours ratings

![Imgur](https://i.imgur.com/GFdFuq0.png)
-   `n` 	number of neighbours considered
-   `Ru`    rating of the item `i` given by user `u`
-   `Su`    similarity of such user with the target (weight)

###     Example
| User | Spiderman 2 | Pearson Coefficient |
|:-----|:-----------:|:-------------------:|
| Bill | 4.5 | 0.5 |
| Robert | 5 | 0.7 |
| Clara | 3.5 | 0.8 |

```
rPred = (0.5 * 4.5 + 0.7 * 5 + 0.8 * 3.5) / 0.5 + 0.7 + 0.8
rPred = 8.55 / 2.0
rPred = 4.275
```

##      Recap
-   Recommender systems
    -   Collaborative filtering => focus on users and ratings
    -   Content-based filtering => focus on item feature

-   Collaborative filtering
    -   User-item
        -   Compute similarity between users
        -   Find nearest neighbour of target user
        -   User their ratings of new items to predict ratings for the target user
    -   Item-item
        -   Lecture 3

###     Exercise

Compute the predicted rating for Jim for item C, considering his 3 nearest neighbours using Euclidean similarity with a minimum threshold of 0,35.

| User | Item A | Item B | Item C | Item D |
|:-----|:------:|:------:|:------:|:------:|
|Tom|1.5|4.5|3|-|
|Bill|5|-|1|1|
|Amy|1|4|5|5|
|Jim|2|5|**?**|4|
|Sara|3|4|4|-|

|User| Euclidian distance to Jim |
|:--|:--|
|Tom| ? |
|Bill|sqrt(18) |
|Amy|sqrt(3) |
|Sara| ? |

Euclidian similarities
```
d(Tom,Jim) = (1.5 - 2)^2 + (4.5 - 5)^2
d(Tom,Jim) = sqrt(0.5)
s(Tom,Jim) = 1 / 1 + sqrt(0.5)
s(Tom,Jim) = 0.59

s(Bill,Jim) = 1 / 1 + sqrt(18)
s(Bill,Jim) = 0.19

s(Amy,Jim) = 1 / 1 + sqrt(3)
s(Amy,Jim) = 0.37

d(Sara,Jim) = (3 - 2)^2 + (4 - 5)^2
d(Sara,Jim) = sqrt(2)
s(Sara,Jim) = 1 / 1 + sqrt(2)
s(Sara,Jim) = 0.41
```
The nearest neighbours to Jim are Tom, Sara and Amy

#      Lecture 3
-   User-item
    -   Main problems
        -   Scalability
        -   Sparsity

-   Item-item
    -   Idea is to combine item similarity and user ratings to create recommendations
    -   Item based reasoning
        -   If item A is very similar to item B
        -   ... and if a user likes item A very much...
        -   ... then they will probably like item B as well!
    -   Similarity between items can be pre-computed
        -   Only the combination with the users' ratings is in "real time"

##      Item-item Slope One

-   Steps
    1.  Compute `deviations` between pairs of items (ahead of time)
    2.  Combine ratings and deviations to make predictions (runtime)

###     Intuitive Example
How much will Ben like Whitney Houston?
| | PSY | Whitney Houston|
|:--|:-:|:-:|
|Amy|3|4|
|Ben|4|?|

Calculate predicted rating
```
deviation(WH,PSY) = 4 - 3 = +1
//  Note the + in + 1 as this is a deviation of the PSY rating compared to WH rating

Ben's rating of WH = 4 + 1 = 5
```

###     Part 1
1.  Compute deviations between all pairs of items

> Deviation = `average difference` in ratings between the two items (considering only the users who rated both items)

![Imgur](https://i.imgur.com/8Z3DGyZ.png)

-   S<sub>i,j</sub> = set of users who rated _both_ items `i` and `j`
-   u<sub>i</sub> = rating of user `u` for item `i` (same for u<sub>j</sub>)
-   card(SS<sub>i,j</sub>) = number of users who rated both items

Pseudocode for the formula
```
deviation(i, j) =>
   currdev = 0
   foreach user u who rated both items i and j
      currdev += (rating of user u to item i) - (rating of user u to item j)
   currdev/(how many users rated both i and j)
```

####    Example 1
|User|Taylor Swift|PSY|Whitney Houston|
|:--|:-:|:-:|:-:|
|Amy|4|3|4
|Ben|5|2|?
|Clara|?|3.5|4
|Daisy|5|?|3

Deviation between Taylor Swift and PSY?
```
dev(TS,PSY) = (4-3) + (5-2) / 2
dev(TS,PSY) = 1 + 3 / 2
dev(TS,PSY) = +2
```

Deviation between PSY and Whitney Houston?
```
dev(PSY,WH) = (3-4) + (3.5-4) / 2
dev(PSY,WH) = -1 + -0.5 / 2
dev(PSY,WH) = -0.75
```

Deviation between Whitney Houston and Taylor Swift?
```
dev(WH,TS) = (4-4) - (3-5) / 2
dev(WH,TS) = -2 / 2
dev(WH,TS) = -1
```

Deviation matrix
| |Taylor Swift|PSY|Whitney Houston|
|:-|:-:|:-:|:-:|
|Taylor Swift|0|2|1|
|PSY|-2|0|-0.75|
|Whitney Houston|-1|0.75|0|

If a user inserts a new rating in the system, we do not need to update the deviations.

For each pair(x,y) we store
-   Deviation `d`
-   The number of people who rated both items `n`

Updating the deviation requires one quick computation!

**Updating deviations example**
-   `n = 9` users rated both `A` and `B`
-   current deviation between `A` and `B` is dev<sub>A,B</sub> = 2
-   a user rates item `A` with r<sub>A</sub> = 5 and item `B` with r<sub>B</sub> = 1

To update the deviation between A and B
```
dev'(A,B) = (dev(A,B) * n) + (rA - rB) / n + 1
          = (2 * 9) + (5 - 1) / 10
          = 22 / 10
          = 2.2
```

###     Part 2
2.  Making predictions combining deviations and ratings

![Imgur](https://i.imgur.com/wAY0FKh.png)

-   p(u,i) = predicted rating for user `u` and item `i`
-   j are the items already rated by user `u` (uj is the rating)
-   dev(i,j) = deviation between items `i` and `j`
-   card(S(i,j)) = number of users who both rated `i` and `j`

Psuedocode
```js
predictedRating = (u, i) =>
   numerator = 0 //teller
   denominator = 0 //noemer
   foreach (item j that user u already rated)
      extract info about the deviation between i and j (dev and howManyUsers)
      numerator += (rating of u for j + dev between i and j) * (howManyUsers)
      denominator += howManyUsers
   numerator/denominator
```

####    Example 1
Ben's ratings
| | Taylor Swift | PSY | Whitney Houston|
|:--|:-:|:-:|:-:|
|Ben|5|2|?|

| |Taylor Swift|PSY|Whitney Houston|
|:-|:-:|:-:|:-:|
|Taylor Swift|0|2|1|
|PSY|-2|0|-0.75|
|Whitney Houston|-1|0.75|0|

What is the predicted rating of Whitney Houston for Ben?
```
p(Ben, WH) = (5-1) * 2 + (2+0.75) * 2 / 2 + 2
           = 8 + 5.5 / 4
           = 3.375
```

###     Example exercise
|Customer|A|B|C|
|:--|:-:|:-:|:-:|
|John|5|3|2|
|Amy|4|2|3|
|Mark|3|4|?|
|Lucy|?|2|5|

Deviations
```
dev(A,B) = (5-3)+(4-2)+(3-4) / 3 = 1
dev(B,C) = (3-2)+(2-3)+(2-5) / 3 = -1
dev(A,C) = (5-2)+(4-3) / 2 = 2
```

Deviation matrix
| | A | B | C |
|:--|:-:|:-:|:-:|
| A | 0 | 1 <sub>(3)</sub> | 2 <sub>(2)</sub> |
| B | -1 <sub>(3)</sub> | 0 | -1 <sub>(3)</sub> |
| C | -2 <sub>(2)</sub> | 1 <sub>(3)</sub> | 0 |

```
p(Lucy,A) = ?
p(Lucy,A) = (2 + 1) * 3 + (5 + 2) * 2 / 3 + 2
p(Lucy,A) = 23 / 5
p(Lucy,A) = 4.6

p(Mark,C) = ?
p(Mark,C) = (3 - 2) * 2 + (4 + 1) * 3 / 2 + 3
p(Mark,C) = 2 + 15 / 5
p(Mark,C) = 3.4
```

|Customer|A|B|C|
|:--|:-:|:-:|:-:|
|John|5|3|2|
|Amy|4|2|3|
|Mark|3|4|**3.4**|
|Lucy|**4.6**|2|5|

##      Recap exercise

Compute the predicted rating for item `B` for Tom using the Item-item One Slope technique, given the following fataset and the (partially precompiled) deviations matrix

| Customer | Item A | Item B | Item C |
|:---------|:------:|:------:|:------:|
| Bill | 4 | 3 | - |
| Wendy | 1 | 2 | 2 |
| Tom | 3 | - | 3 |
| Sara | - | 2 | 5 |

| Deviations | Item A | Item B | Item C |
|:-----------|:------:|:------:|:------:|
| Item A | 0 | ? | -0.5 <sub>(2)</sub> |
| Item B | ? | ? | ? |
| Item C | 0.5 <sub>(2)</sub> | ? | 0 |

Deviations
```
dev(A,B) = (4-3) + (1-2) / 2 = 0
dev(B,C) = (2-2) + (2-5) / 2 = -1.5
```

| Deviations | Item A | Item B | Item C |
|:-----------|:------:|:------:|:------:|
| Item A | 0 | 0 | -0.5 <sub>(2)</sub> |
| Item B | 0 | 0 | -1.5 <sub>(2)</sub> |
| Item C | 0.5 <sub>(2)</sub> | 1.5 <sub>(2)</sub> | 0 |

```
p(Tom,B) = (3-0) * 2 + (3-1.5) * 2 / 4
p(Tom,B) = 9 / 4
p(Tom,B) = 2.25
```

#       Lecture 4
-   Item-item ACS (Adjusted Cosine Similarity)
    -   Step 1: compute similarities between all pairs of items
    -   Step 2: use the known ratings of the target user and their similarities with the "target item" to predict the rating for the target item

##  Item-Item ACS Step 1
> Similarity between items => **COLUMNS**
    -   Consider only rows with values in both columns

|   | Users | -  | Phoenix | -  | Passion Pit | -  | n  |
|:--|:------|:---|:-------:|:--:|:-----------:|:--:|:--:|
|1  | Tamara Young |  | 5 | | | | |
|2  | Jasmine Abbey | | | | 4 | | 
|u | Cecillia de la Cueva | | **1**| | **2** | | |
|m-1| Jessica Nguyen | | **5**| | **5** | | |
|m | Jordyn Zamora | | **4**| | **5** | | |

![Imgur](https://i.imgur.com/AvvyMSL.png)

- acs(i,j) = similarity between items i and j
- Ru,i = rating of user u for item i
- Ru,i - Ru(with line above) = rating of user u for item i, **adjusted**

-   Numerator
    -   For every user **(who rated both items)** *multiply* the adjusted rating of those two items and sum the results
-   Denominator
    -   Sum the squares of all the adjusted ratings for item i and take the square root of that result for only the users which rated both items
    -   repeat for j
    -   multiply those two

###     Example 1

| Users | Average rating | Kacey Musgraves | Imagine Dragons |
|:------|:--------------:|:----:|:---------------:|
|David|3.25| | 3 |
|Matt | 3.0 | | 3 |
| Ben | 2.75 | **4** | **3** |
| Chris | 3.2 | **4** | **4**|
|Tori|4.25|**5**|**4**|

```
s(Musgraves,Dragons) = ((4-2.75) * (3-2.75) // Ben's ratings) + ((4-3.2) * (4-3.2) // Chris' ratings) + ((5-4.25) * (4-4.25) // Tori's ratings) / sqrt((4-2.75)^2 + (4-3.2)^2 + (5-4.25)^2) * sqrt((3-2.75)^2 + (4-3.2)^2 + (4-4.25)^2)
s(Musgraves,Dragons) = 0.7650 / sqrt(2.765) * sqrt(1.265)
s(Musgraves,Dragons) = 0.7650 / 1.4544
s(Musgraves,Dragons) = 0.5260
```

###     Example 2
Compute ACS between A and B, and between B and C
- Start by computing the average rating for each user

|User|A|B|C|
|:--|:--:|:--:|:--:|
|Amy|3.5|3.0|4.0|
|Bill|2.0|5.0|2.0|
|Clara|3.0|/|5.0|

Averages
```
Amy = 3.5 + 3.0 + 4.0 / 3 = 3.5
Bill = 2.0 + 5.0 + 2.0 / 3 = 3.0
Clara = 3.0 + 5.0 / 2 = 4.0
```

Similarity between A and B
-   Two users involved: Amy and Bill

```
acs(A,B)=(3.5-3.5)(3.0-3.5) + (2.0-3.0)(5.0-3.0) / sqrt((3.5-3.5)^2 + (2.0-3.0)^2) * sqrt((3.0-3.5)^2+(5.0-3.0)^2)
acs(A,B)= 0 - 2 / sqrt(0 + 1) * sqrt(0.25 + 4)
acs(A,B)= -2 / sqrt(4.25)
acs(A,B)= -0.97
```
Similarity between B and C
- Two users involved: Amy and Bill

```
acs(B,C)=(3.0-3.5)(4.0-3.5) + (5.0 - 3.0)(2.0 - 3.0) / sqrt((3.0-3.5)^2 + (5.0-3.0)^2) * sqrt((4.0-3.5)^2 + (2.0-3.0)^2)
acs(B,C)= -0.25 + -2 / sqrt(0.25 + 4) * sqrt(0.25 + 1)
acs(B,C)= -2.25 / sqrt(4.25) * sqrt(1.25)
acs(B,C)= -2.25 / 2.30488
acs(B,C)= -0.97
```
>Watch out for the minus symbols at the end, make sure to put the entire denominator in () brackets to ensure proper calulation

##      Item-item ACS, step 2
![Imgur](https://i.imgur.com/4DswAXV.png)

-   p(u,i) = predicted rating for user u of item i
-   j = all items for which there is a similarity value with i and which user u rated 
    -   S<sub>i,j</sub> = acs(i,j) => similarity between items i and j
    - R<sub>u,j</sub> = rating that user u gave to item j

Pseudocode
```
predictedRating(u, i) =>
numerator = 0 //teller
denominator = 0 //noemer
foreach item j that user u already rated
numerator += (similarity between i and j) * (rating of u for j)
denominator += absolute value of similarity between i and j
numerator/denominator
```

>Important
-   For the formula to work best R<sub>u,j</sub> should be between -1 to 1
-   => **normalization** needed

-   Normalization formula
    -   r' = 2 * ((r - r<sub>min</sub>) / (r<sub>max</sub> - r<sub>min</sub>)) - 1
-   Denormalization formula
    -   r = ((r' + 1) / 2) * (r<sub>max</sub>-r<sub>min</sub>) + r<sub>min</sub>

###     Normalization example
-   r = 2 in a scale from 1 to 5

Compute the normalized rating between -1 and 1
```
r' = 2 * ((2-1)/(5-1)) - 1
r' = 2 * (1/4) - 1
r' = 0.5 - 1
r' = -0.5
```

###     Denormalization example
-   r' = -0.5 in a scale from -1 to 1

Compute the rating in a scale from 1 to 5
```
r = ((-0.5 + 1) / 2) * (5-1) + 1
r = 0.5 / 2 * 4 + 1
r = 0.25 * 4 + 1
r = 2
```

###     Example 1
-   Compute the predicted rating for David for Kacey Musgraves

David's Ratings
| Artist | Rating | Normalized Rating |
|:-------|:-:|:--:|
|Imagine Dragons|3|0|
|Daft Punk|5|1|
|Lorde|4|0.5|
|Fall Out Boy|1|-1|

Similarity matrix
|          | Fall Out Boy | Lorde | Daft Punk | Imagine Dragon |
|:---------|:------------:|:-----:|:---------:|:--------------:|
| Kacey Musgraves | -0.9549 | 0.3210 | 1.000 | 0.5260 |

```
p(David,Musgraves) = (.5260 * 0) + (1.00 * 1) + (.321 * 0.5) + (-.955 * -1) / 0.5260 + 1.000 + 0.321 + 0.955
p(David,Musgraves) = 0 + 1 + 0.1605 + 0.955 / 2.802
p(David,Musgraves) = 2.1105 / 2.802
p(David,Musgraves) = 0.753
```
This still needs to be de-normalized
```
r = ((0.753 + 1) / 2) * (5 - 1) + 1
r = (1.753 / 2) * 4 + 1
r = 3.506 + 1
r = 4.506
```

###     Example 2
-   Exercise: predict the rating of item B for Clara
    -   ACS(A,B) = -0.97 ACS(B,C) = -0.98

|User|A|B|C|
|:--|:--:|:--:|:--:|
|Amy|3.5|3.0|4.0|
|Bill|2.0|5.0|2.0|
|Clara|3.0|/|5.0|

```
p(Clara, B) = 0 * (-0.97) + 1 * (-0.98) / 0.97 + 0.98
p(Clara, B) = -0.98 / 1.95
p(Clara, B) = -0.5
```
This still needs to be denormalized => 2

#   Lesson 6

Lesson 6

1

```
d(Amy,Bill) = sqrt((3.0-2.5)^2 + (4.5-1.0)^2)
            = sqrt((0.5)^2 + (3.5)^2)
            = sqrt((0.5)^2 + (3.5)^2)
            = sqrt(0.25 + 12.25)
            = sqrt(12.5)
s(Amy,Bill) = 1 / 1 + sqrt(12.5)
s(Amy,Bill) = 0.22

d(Amy,John) = sqrt((3.0-3.5)^2 + (4.5-5.0)^2)
            = sqrt((-0.5)^2 + (-0.5)^2)
            = sqrt(0.5)
s(Amy,John) = 1 / 1 + sqrt(0.5)
s(Amy,John) = 0.59

d(Amy,Clara) = sqrt((3.0-3.5)^2 + (4.5-4.5)^2)
             = sqrt((-0.5)^2 + 0)
             = sqrt(0.25)
s(Amy,Clara) = 1 / 1 + sqrt(0.25)
             = 0.67
```

Nearest neighbours are John and Clara
```
r(Amy,A) = (4.5*0.59) + (3.0*0.67) / 0.59 + 0.67
r(Amy,A) = 3.7
```

2

deviation matrix
```
dev(A,B) = (2.0-2.5) + (4.5-3.5) + (3.0-3.5) / 3
         = -0.5 + 1 + -0.5 / 3
         = 0 / 3
         = 0 (3)

dev(B,C) = (3.0-4.5) + (2.5-1.0) + (3.5-5.0) + (3.5-4.0) / 4
         = -1.5 + 1.5 + -1.5 + -.5 / 4
         = -0.5 (4)

dev(A,C) = (2.0-1.0) + (4.5-5.0) + (3.0-4.0) / 3
         = 1 + -0.5 + -1 / 3
         = -0.5 / 3
         = -0.17 (3)
```

x | A    | B   | C   |
:--|:----:|:----:|:----:|
A | 0    | 0   |-0.17|
B | 0    | 0   |-0.5 |
C | 0.17 | 0.5 | 0   |

```
r(Amy,A) = (3.0 + 0) + (4.5-0.17) / 2
         = 3.0 + 4.3 / 2
         = 3.67
```

3

[2.5, 3, 4.25, 5] of [1,5] into [0,1]
```
2.5 - 1 / 5 - 1 = 0.38
3 - 1 / 5 - 1 = 0.5
4.25 / 5 = 0.81
4 / 4 = 1
```
```
[2.5, 3, 4.25, 5] of [-3,5] into [0,1]
2.5 - -3 / 5 - -3 = 0.69
3 - -3 / 5 - -3 = 0.75
4.25 - -3 / 5 - -3 = 0.91
5 - - 3 / 5 - -3 = 1
```

4

Implication expression of the form X=>Y
Where X,Y are itemsets with an empty intersection

Example is { bread, milk } => { beer }

To evaluate if an association rule is good we evaluate it through the three metrics.

|x|A|B|C|D|E|
|1|1|1|1|0|0|
|2|1|0|1|1|0|
|3|0|1|1|1|0|
|4|1|0|0|1|1|
|5|0|1|1|0|1|

```
supp(A,D) = 2 / 5
conf(A,D) = 2 / 3
lift(A,D) = (2 / 5) / 3/5 * 3/5 = 1.11

supp(C,A) = 2 / 5
conf(C,A) = 2 / 4
lift(C,A) = (2 / 5) / 4/5 * 3/5 = 0.83

supp(BC,D) = 1 / 5
conf(BC,D) = 1 / 3
lift(BC,D) = (1 / 5) / 3/5 * 3/5 = 0.55
```

5
```
s = 1 - (7 / 12)
s = 1 - 0.58
s = 0.42
```
6

Average ratings
Amy     6
Bill    3
Clara   5
Dirk    6
Emma    7

Normalized ratings of Amy
```
I3      2 * (4-1) / (10-1) - 1 = -0.33
I4      2 * (8-1) / (10-1) - 1 = 0.55

//  Calculated using Clara and Dirk
acs(I1,I3) = (3-5 * 7-5) + (4-6 * 5-6) / sqrt((3-5)^2 + (4-6)^2) * sqrt((7-5)^2 + (5-6)^2)
           = -0.32

//  Calculated using Bill and Clara
acs(I1,I4) = (2-3 * 6-3) + (3-5 * 5-5) / sqrt((2-3)^2 + (3-5)^2) * sqrt((6-3)^2 + (5-5)^2)
           = -0.45

p(Amy,I1) = (-0.32 * -1/3) + (-0.45 * 5 / 9) / 0.32 + 0.45
          = 0.1067 - 0.25 / 0.77
          = -0.14

r(Amy,I1) = ((-0.14 + 1) / 2) * (10-1) + 1
          = 0.86 / 2 * 9 + 1
          = 4.87
```

#       Lesson 7

1

Manhattan
```
d(1,2) = (3-2) + (2.5-4) + (1-3.5)
       = 1 + 1.5 + 2.5
       = 5

s(1,2) = 1 / 1 + 5
       = 1/6
```

Euclidian
```
d(1,2) = sqrt((3-2)^2 + (2.5-4)^2 + (1-3.5)^2)
       = sqrt(1 + 2.25 + 6.25)
       = sqrt(9.5)

s(1,2) = 1 / 1 + sqrt(9.5)
       = 0.25
```

2

s = 1 - (300 / 100 * 200)
s = 1 - 0.015
s = 0.985

Dataset is very sparse, 98.5% is not filled in

3
```
s(Rick) = 1 / 1 + 1.4 = 0.42
s(Stan) = 1 / 1 + 1.4 = 0.42
s(Lisa) = 1 / 1 + 3.6 = 0.21
```

Nearest neighbours are Rick and Stan
```
r(Anne,Y) = 4 * 0.42 + 5 * 0.42 / 0.42 + 0.42 = 4.5
```

4
```
acs(I2,I3) = (5-5)*(7-5) + (9-6)*(5-6) + (6-7)*(8-7) / sqrt((5-5)^2 + (9-6)^2  + (6-7)^2) * sqrt((7-5)^2 + (5-6)^2 + (8-7)^2)
           = -0.52

acs(I2,I4) = -1

p(Amy,I2) = ((-0.52 * -1/3) + (-1 * 5/9)) / (0.52 + 1) = -0.26
r(Amy,I2) = -0.26 + 1 / 2 * 10-1 + 1 = 4.33
```

