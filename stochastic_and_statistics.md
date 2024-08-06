# Probability Theory and Statistic: The interaction between your assumptions and the Real World
Most people would say the chance of seeing a 6 after roling a dice is 1 in 6, or 16.66\%. Also most of those people would reason their claim with something like:

'The dice has 6 sides and it is equally likely that the dice lands on any of the 6 sides. Therefore the likelihood is 1 in 6.'

But I don't know a single of my friends that could explain to me:

* 'why are the chances equal for all the 6 sides'
* 'why is it impossible, that the likelihood of seeing 1 is twice the likelihood of seeing 4'

For an explanation we probably have to ask some learned physicist, but most of the time we go with our gutfeeling. This gutfeeling tells us that we can expect to see a 6 roughly every 6th role of the dice, but the outcome is random. This gutfeeling represents what is called a **probabilistic model** in the field of **Probability Theory**. To model real world phenomens it is necessary to incorporate the following two assumptions into a **probabilistic model**:

* What possible outcomes do we expect from the process. In the case of rolling a dice we expect to either see 1, 2, 3, 4, 5, 6
* What is the probability of any of the possible outcomes. In the case of rolling a fair dice we assume that it is 16.66/% for each of the possible outcomes

Just for now, please forget all your gut feelings and everything you think you know about rolling a dice. Imagine that you have no idea how likeli it is, that a dice with numbers 1 to 6, lands on 3. What could you do to get a **probabilistic model** of rolling a dice?

Now **Statistics** comes into play. The (frequentist*) statistical approach would be to collect data of rolling a dice. So you would sit down, role the dice a thousand times and for each die you write down the result. Then you would calculate the frequency of the numbers 1 to 6 and you will get a result like:

* based on my 1000 throws the probability to role a 1 is *number of occurence of 1* divided by a thousand
* based on my 1000 throws the probability to role a 2 is *number of occurence of 1* divided by a thousand
* etc.

If you used a fair dice the probabilities will all be very close to 16.66/%. So we have seen two ways to get a **probabilistic model** that describes the features of rolling a dice:

* one probabilistic model is derived logically (or in our example by gutfeeling, which unfortunately is the same as reasoning for many people)
* the other probabilistic model is derived statistically

And this is the core principle of applied statistics (and most of Machine Learning)
