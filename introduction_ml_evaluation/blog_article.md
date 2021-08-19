# Evaluating Machine Learning Solutios

## An Educational Job Interview
**Interviewer**: "Sounds good so far, BUT how do we know that your solution works?".

**Me**: "Well...Ehm?...Yeah!?...I think I don't understand the question...."

**Interviewer**: "You just described to us, how you would build a Recommender System for our customers using Machine Learning (ML). But how would you evaluate if it is any good or not?"

I had no idea how to answer this question. I mean of course I had. But my approaches were all very technical, academic and had no direct significance, when it comes to business benefit. So not at all what a company wants to hear, that intents to earn money with Machine Learning.

This was the moment I fell from the soft clouds of academic statistical research and landed on the hard and dirty grounds of applied Machine Learning. Even if I didn't get the job, this interview taught me a valuable lesson:

**Always think about how to measure the real world benefit of your Machine Learning Solution.**

But what does "real world benefit" mean?

## Learning Spanish
Imagine you are learning a new language, spending thousands of hours learning vocabulary and doing regulary vocabulary tests, that you all pass with flying colors. Now imagine yourself flying to the country, where they speak that language, and you aren't able to converse with anybody there. Would you be surprised?

No! Of course not. We all know that learning a new language consists of much more than just learning vocabulary and the performance on vocabulary tests alone is not at all a guarantee that you have mastered a language. Although you did learn something, that has some value for you (Now you are unbeatable in Spanish Scrabble!), you gained no real world benefit (You are still not able to order food and drinks!).

While this example is obvious, when it comes to Machine Learning Solutions it is not. I have seen many projects were a lot of time and effort was spent into bringing solutions live, where the Machine learnt something with no real world benefit.

In this article I will:

* explain the principles of evaluating Machine Learning Solutions (MLS)
* tell you why a Data Scientist doesn't necessarily think about real world benefits (in other words: why I failed my job interview)
* show you what can go wrong

## What do I mean with Machine Learning Solution
If you don't know yet, what Machine Learning is, what Training and Prediction means, I would recommend you to find out first, before continue reading this article. If you are still reading, I assume, that you had your first encounters with Machine Learning and know how it can be used.

On a very high level a Machine Learning Solution can be split into two steps:

* Training: the Training Step is the part where the Machine actually learns something from the data. After the Training Step the Machine is ready to use its learned abilities. For example:
    - the Machine learns to recognize cats in images
    - the Machine learns which characteristics are determining if a person buys a certain product or not.
* Prediction: the Prediction Step is the part where the Machine applies what it has learned, by returning a (hopefully) meaningful response to some new input. For example:
    - given an image, the Machine tells you if it sees a cat or not
    - given the characteristics of a new customer, the Machine tells you how likely it is, that the person will buy the product

In technical terms, one could say that the Training step is a higher-order function $T$ that gets data $D$ as input and returns a Prediction function $f$.

$T(D) \rightarrow f$

Depending on the task the Machine should learn, the Prediction function takes a specific input $x$ (often called features) and returns an appropriate prediction $\hat{y}$.

$f(x) \rightarrow \hat{y}$

Continuing our examples from above:
* $x$ could be any image and $\hat{y}$ is the probability that the image contains a cat
* $x$ could be age and profession of a person and $\hat{y}$ is the probability that the person buys the product

So far so good, but now the question, I was asked by the recruiter becomes relevant:
"How can we now if the solution is any good for our business?"



## Two different worlds
I think the difficulty of assessing the real world benefit of a Machine Learning Solution is mainly that the machine doesn't directly learn what one wants it to learn. For example:

* The machine can't learn how you can increase your online sales. The machine can only learn how likeli it was that a certain customer bought a product in the past, and recommend new similar customers, the product past customers were mostlikel to buy.
* The machine can't learn when you should restock your warehouses. The machine can only learn what would have been the best statistical model explaining the historic demand timeseries, and use this model to make a demand forecast for the future.

That means, in order to design a Machine Learning Solution that solves real world problems, a Data Scientist always needs to translate the problem into a "machine solvable solution".

**Example:**
Using the example from above, our real world problem is that we need to know how much of every item we need to order to properly restock our warehouse and satisfy future demands without buying too much. An obvious possible solution of a Data Scientist could be to fit a Timeseries Model for the weekly sales figures. Simply speaking, one can imagine that the Machine draws a smooth line through the historic demands. The machine then creates demand forecasts by extending this smooth line into the future. So instead of learning how much we need to buy, the Machine learns sales patterns of the past and how to use those patterns to forecast sales figures into the future. Obviously, the Machine doesn't really learn how to draw a line through the historic data, but instead solves numerical optimization problems. After the model training, the Machine knows how it can forecast the timeseries into the future. Additionally, we also know how good the smooth line, that the machine learned, fits the historic data. This goodness of fit is commonly represented by some accuracy metrics like $R^2$, mean absolute error (MAE), mean absolute percentage error (MAPE), etc...

Those accuracy metrics can already be good indicators on how good the Machine can solve your real world problem. But they do not directly answer the question:
How high will be our stock-keeping costs based on the Machines predictions? How high will be the losses due to understocking because of the Machines predictions?

Sometimes the real world problem translates almost directly into a machine solvable solution and the machines performance measures are very good indicators for its performance in the real world. But in many cases the machines performance when solving the proxy problem, has no (direct) forecasting power for its real world performance. It happens easily getting convinced of a solution by only looking at the proxy problems performance metrics.

## Like picking up a car from the workshop
The whole process of Machine Learning is quite complex and especially for non-specialists it sometimes feels like magic, and it is best to leave to the specialists and trust them. Trust is good, control is better. Machine Learning is complex and designing Machine Learning Solutions is definitely a task for specialists, I have no doubts about that. But that shouldn't stop you from thinking about possibilities to measure the real world benefit of a solution.

Imagine you hear noises coming from the front of your car and take it to the workshop. A few days later you pick it up, and the mechanic tells you, that the cylinder head gasket was loose and neaded to be tightened. Even if you are not a mechanic and don't even know what a cylinder head gasket is, you should be able to tell if your car is working properly again or not.

I am convinced, the same should apply for Machine Learning Solutions. After all, once a Machine Learning Solution is trained it is nothing more than a function, that predicts/returns something for any input observation.

## It is just a function




But thinking about possibilities to measure the real world benefit of a solution should not be a magical black box. It should be transparent and understandable, even for non specialists.

test

