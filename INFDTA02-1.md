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
| User  |Item 1   |Item 2   |Item 3   |Item 4   |Item 5   |
|:------|:--:|:--:|:--:|:--:|:--:|
|Clara  |4.75|4.5 |5   |4.25|4   |
|Robert |4   |3   |5   |2   |1   |
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
| User  |Item 1   |Item 2   |Item 3   |Item 4   |Item 5   |
|:------|:--:|:--:|:--:|:--:|:--:|
|Clara  |4.75|4.5 |5   |4.25|4   |
|Robert |4   |3   |5   |2   |1   |

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

>   Pearson Coefficient
-   Grade inflation
    -   Different users => different scale

>   Cosine Similarity
-   Data is sparse

>   Manhattan, Euclidean
-   Data is dense
    -   Almost all attriutes have non-zero values
    -   Magnitude of the values is important

##      RECAP Exercise

Calculate the similarity between Bill and Jim using all four measures learned during this lecture.
| User | A | B | C | D | E |
|:-----|:-:|:-:|:-:|:-:|:-:|
| Bill | 5 | - | 1 | 3 | 5 |
| Jim  | 2 | 5 | 4 | 3 | - |

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
    - >No! Any quirk would be used in creating recommendations
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
-   Item `i` = Spiderman 2

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
|:-|:-:|:-:|:-:|:-:|:-:|
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
d(Tom,Jim) = 0.5
s(Tom,Jim) = 1 / 1 + 0.5
s(Tom,Jim) = 0.67

s(Bill,Jim) = 1 / 1 + sqrt(18)
s(Bill,Jim) = 0.19

s(Amy,Jim) = 1 / 1 + sqrt(3)
s(Amy,Jim) = 0.37

d(Sara,Jim) = (3 - 2)^2 + (4 - 5)^2
d(Sara,Jim) = 2
s(Sara,Jim) = 1 / 1 + 2
s(Sara,Jim) = 0.33
```
The nearest neighbours to Jim are Tom and Amy, the others did not meet the threshold.

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
dev(TS,PSY) = 2
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
