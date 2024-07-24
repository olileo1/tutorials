# Anomaly vs Outlier
A very common task in Machine Learning is **Anomaly Detection**. Some people also use the term **Outlier Detection** synonymously for good reasons. I personally think that there is slight difference and it can be very helpful to also think about the two terms **Anomaly** and **Outlier** differently. So let's start with some definitions.

## Anomaly
For Anomaly I prefer the following definition:

**something that deviates from what is standard, normal, or expected**

So an anomaly is simply any event, that you would not expect. For example a wild polar bear in the saharan desert or cream in your spaghetti carbonara. But I want to point out, that *what you expect* heavily depends on your *knowledge* and the *information* you have. If you are sitting in a cheap italian restaurant somewhere far away from Italy, getting served a sauce with milk and cream that was labelled as "Carbonara" might not surprise you as much as getting the same plate sitting at the Piazza Navona in the heart of Rome.

## Outlier
I am neither an etymologist nor a native english speaker, but I would be very surprised if the term **outlier** does not derive from **something that is lying outside**. So, to being able to call something an outlier you need to have some understanding of *what means inside*, *what means outside*, *what is near* and *what is far*. In everday language the term **outlier** is closely linked to locations.

## Anomalies and Outliers in Machine Learning
As already said in Machine Learning the terms Anomaly and Outlier are often used synonymously, and I guess everyone with a technical background will also use the two terms synonymously. But if we only focus on the meaning of the words in everyday language we see two slightly different meanings:

* **Anomaly**: something that deviates from what is expected. So to classify something as anomaly, we need to have expectations first.
* **Outlier**: something that lies outside/far away. So to classify something as outlier, we need to have a concept of space together with a definition of a distance between two objects

In the following I want dwelve deeper into Anomalies and Outliers and analyse the two concepts in the context of Machine Learning. I will show the following things:

* In Machine Learning every Outlier is also an Anomaly
* Not every Anomaly is an Outlier
* All Anomaly Detection methods rely on somehow transforming/processing the observations in a way that Anomalies turn out to be Outliers

So in the end it is fair enough to equate **Anomalies** and **Outliers**, but distinguishing the two concepts can be helpful in developing Machine Learning Methodology for Anomaly/Outlier Detection Use Cases

# Outlier Detection for Gaussian Distributed data
To begin our journey into Anomaly Detection let's start with the most basic example I can imagine: **univariate Outlier Detection in Gaussian Distributed Data**. Imagine the following scenario:

You have a table with information about adult dogs, like this:

| name  | breed            | sex | height | weight |
|-------|------------------|-----|--------|--------|
| Rocco | German Shepherd  | M   | 63     | 35     |
| Lucy  | American Bulldog | F   | 53     | 37     |
| Happy | Dachshund        | M   | 20     | 10     |
| Cindy | Dalmatian        | F   | 56     | 18     |
| Benny | Dalmatian        | M   | 61     | 18     |

The data was collected manually and although you are quite confident, that most of the entries are correct, you have already seen observations like this: Rambo, the 95cm high German Shepherd with a weight of 52kg. Or Bella, the 22cm short American Bulldog with a weight of 45kg.

Now you want to assess how many of those suspicious entries are there in your database. Your first idea is to focus on the height and you want to detect entries of the database, where the height seems very suspicious given the breed and sex of the dog.

## What is far away in One Dimensional Distributions?
Since you are the proud owner of a Dalmatian named Rocky, who is a specimen of his breed with a height of almost 64cm, you start to plot a histogram of the height of all mal Dalmatians. The histogram shows a typically bell shaped form, almost as if the underlying data has been generated to follow a normal distribution.
In the histogram you see that your neighbours Dalmatian named Benny is almost perfectly average with his height of 60, and Mr.Pickles, the smallest Dalmatian in you dataset.

Judging from the histogram, you will probably think something like this:

* Benny is pretty average
* Rocky is quite tall, there is almost no other Dalmatian as tall as Rocky
* Mr.Pickles is very small. Maybe almost too small for a male Dalmatian? Maybe the breed or the height isn't correct?

Intuitively it is quite clear, that the farther away a point is from the **bell** the more conspicuous the point appears. But what is too far to be true?

You know Rocky, he is sitting right next to you and you know he is almost 64cm. If someone would tell you, that their Dalmatian is 64.5cm you would probably think: "Well that is even bigger, but still believable". And where is it written, that a Dalmatian can't be 100cm tall and we just haven't seen something like this yet? And what about Mr.Pickles?

So what would be an appropriate number, where we say: Any Male Damlmatian higher than that number is too tall and it is either a very very very special Dalmatian or the data is just wrong?

### The Scaled Distance (or Z-Score)
If we look at the histogram for the size of Dalmatians only, we could easily come to the conclusion to say, that we simple measure the distance between a Point and the average (the average is the Peak of the Bell Curve). So we could say Benny is exactly the average with a height of 60 and the distance of Rocky to Benny is around 4. In comparison to that, Mr.Pickles has a height of 49. So the distance of Mr.Pickles to the center is 11, which is much higher than the distance of Rocky.

But if we look at the Histogram for German Shepherds we see a much wider Bell Curve. So even if the average German Shepherd is also 60cm high, we observed way more German Shepherds smaller than 58 or taller than 62 compared with Dalmatians. So the variance for the Heights of German Shepherds is higher. So if Rocky was a German Shepherd, he would still be rather tall but it would be easier to find another German Shepherd that is even taller.

To make the **distance to the center** comparable we need to account for the width of the bell. This width is directly dependent on the Variance of the distribution. And so the **Z-Score** is born. The **Z-Score** for an observations $x_i$ is defined as:

$$
z_i = \frac{x_i - \mu}{\sigma},
$$

where $\mu$ is the expected value of the population and $\sigma$ is the Standard deviation. So Rocky's Z-Score compared with all the other dalmatians would be:

$$
\frac{64 - 60}{1} = 4
$$

If Rocky was a German Shepherd his Z score would be:

$$
\frac{64 - 60}{2} = 2
$$

In Comparison Mr.Pickles Z-Score is:

$$
\frac{49 - 60}{1} = -11
$$

So according to the Z-Score Mr.Pickles is almost three times as far away from the center as Rocky.

## The Z-Score for Gaussian Distributed Values
Let's remember that a gaussian Distribution is completely specified by the two parameters:

* $\mu$: the mean
* $\sigma$: the standard deviation

With *completely specified* I mean, that for any realization $x$ of $X \sim (\mu, \sigma)$, we can exactly calculate the Probability that $x$ falls into any interval $[c_1, c_2)$. So let's assume that the distribution for height of dogs is normal distribution and that it is a godgiven law that:

* The height of Male Dalmatians is gaussian distributed (and predefined at birth. So just for simplification, we say that it doesn't matter how you design the life of the dog, the final height once he is an adult is purely defined by randomness)
* with an average height of 60.
* and a standard deviation of 1.

Therefore the Height of Male Dalmatians is gaussian distributed and we write $X \sim N(60, 1)$. Based on these assumptions we can conclude the following:

* The probability that a random male dalmatian, that I see on the street is between 58cm and 62cm is around 95.4\%. We denote this as $P[58 \leq X < 62] = 0.954$ (For further details please read the explanation on the Normal Distribution)
* Or the probability that the height of a random observation is higher than 62cm 2.2\%. We denote this as $P[62 \leq X] = 0.02$

Now we get to a very convenient property of the Gaussian Distribution. If we know that a given Random Variable $X$ is gaussian distributed ($X \sim N(\mu, \sigma)$), we can calculate the Z-Score for any realisation $x$ as:

$$z = \frac{x - \mu}{\sigma}$$

and we can denote this by $Z = \frac{X - \mu}{\sigma}$. Then we now that this new Random Variable Z is standard gaussian distributed ($Z \sim N(0, 1)$). So in words if we take random realisation of any normal distribution and we scale this realisation by substracting the mean and dividing by the standard deviation, then we know for a fact, that the result follows a standard normal distribution.

Given this fact it is very easy for us to compare observations from different normal distributions in terms of their probability. We know already that God decided that the height of Male Dalmatians follows a normal distribution with $\mu=60$ and $\sigma=1$. Let's further assume that God decided that the height of Male Corgis follows a normal distribution with $\mu=28$ and $\sigma=2$. Given these two godgiven laws we can finally settle the argument:

**What is more special? A dalmatian smaller than 57 centimeters or a Corgi higher than 36 centimeters?**

For that we can simply calculate the two Z-Scores as:

$$
z_1 = \frac{57 - 60}{1} = -3 \land z_2 = \frac{36 - 28}{2} = 4
$$

So according to the Z-Scores the 36cm high Corgi lies **further away from the bell** as the 57cm high Dalmatian. So it is definitely a more significant outlier. Now comes the nice fact, that we also know the Z-Scores are standard gaussian distributed. So we again know the probabilities that any given random observation $z$ is lower than -3:

$$
P[Z \leq -3] = \Phi(-3) = 0.0013  \text{  }(\Phi \text{ is the CDF of the Standard Gaussian Distribution})
$$

In words that mean the probability, that we see a Male Dalmatian that is even small than 57cm is very very low. Similarly we can do the same thing on the upper side. We know that every z-Score must be between negative infinity and positive infinity. So there are only two possibilities. Either the z-Score is smaller or equal 4 or it is bigger than 4. From that fact we can say that the probability that the z-Score is bigger than 4 is equal to 1 minus the probability that the z-Score is smaller or equal to 4. So we can say:

$$
P[4 < Z] = 1 - \Phi(4) = 0.00003
$$

In words the Probability that we see a Corgi that is even higher than 36cm is almost impossible.

## For Gaussian Distributions **distance to the center** equals **probability of occurrence**
Now let's go back to our definitions of **Outlier** and **Anomaly**.

* **Outlier**: something that lies far away
* **Anomaly**: something that deviates from what is expected

Above we derived the z-Score as a score, that tells us how far an observation is away from the bulk of the other observations. The more extreme the z-Score is (very small or very high), the further away lies the observation. The z-Score is also comparable. That means we can easily just compare the Z-Score of a Corgi with the Z-Score of a Dalmatian to assess which of the two individuals is *more unusual* compared to their groups. So we therefore have a measure for defining Outliers.

Now let's think about **Anomaly** a little bit: Something that deviates from what is expected? Now I don't want to open up a metaphysical debate about expectations and objective/subjective probabilities, but I am bold enough to say "if you expect something to happen, you think there is a high probability that it happens". On the otherside an anomalous event is "something that happened, but you didn't expect it to happen. So you assigned a very very low probability that it will happen". Using the notation from above we would say the observation that a Corgi is 36cm high is an anomaly. The reasoning is: given everything we know about the height of Corgis (namely that the height of Corgis is gaussian distributed $N(28, 2)$), we know that the likelihood that a random Corgi we pick up from the streets is 36cm or even taller is soo unbelievable low, that we would never expect to see a Corgi that big. So the actual observation deviates from our expectation.

So I hope, that this example showed that under the assumption that some observations are gaussian distributed the task of "Detecting Outliers" is equivalent to the task of "Detecting Anomalies".

In another chapter I will give some examples of Use Cases, where the relationship between "Outlier" and "Anomaly" is not as direct and immediate as in the Case of Gaussian distributed observations.

```python
import numpy as np
import plotly.graph_objs as go
from plotly.subplots import make_subplots

# Calculate the standard deviation for 95% of observations to lie between 58 and 62
# For a normal distribution, 95% of the data lies within 1.96 standard deviations from the mean
mean = 60
lower_bound = 58
upper_bound = 62
std_dev = 1

np.random.seed(42)
# Generate 1000 random normal distributed values
normal_dalmatians = np.random.normal(loc=mean, scale=std_dev, size=1000)
mr_pickles = 49
benny = 60
rocky = np.max(normal_dalmatians) + 0.05
values = np.concatenate([
    normal_dalmatians,
    np.array([mr_pickles]),
    np.array([benny]),
    np.array([rocky])
])
mean_shepherds = 60
lower_bound_shepherds = 56
upper_bound_shepherds = 64
std_dev_shepherds = 2
normal_shepherds = np.random.normal(loc=mean, scale=std_dev_shepherds, size=1000)
basti = 60
values_shepherds = np.concatenate([
    normal_shepherds,
    np.array([basti]),
])

# Create a histogram of the values using plotly
fig = make_subplots(
    rows=2,
    cols=1,
    shared_xaxes=True,
    subplot_titles=(
        f'Histogram of Heights for Male Dalmatians (mean={mean}, std={std_dev:.2f})',
        f'Histogram of Heights for Male German Shepherds (mean={mean_shepherds}, std={std_dev_shepherds:.2f})'
    )
)
_ = fig.add_trace(go.Histogram(x=values, showlegend=False, marker_color="blue", marker_opacity=0.6, xbins_size=0.2), row=1, col=1)

# Add a vertical line at X=61
_ = fig.add_trace(go.Scatter(
    x=[benny, benny],
    y=[0, 100], # y values range from 0 to the max count in the histogram
    mode="lines",
    line=go.scatter.Line(color="red"),
    showlegend=True,
    name="Benny"
), row=1, col=1)

# Add a vertical line at X=61
_ = fig.add_trace(go.Scatter(
    x=[mr_pickles, mr_pickles],
    y=[0, 100], # y values range from 0 to the max count in the histogram
    mode="lines",
    line=go.scatter.Line(color="green"),
    showlegend=True,
    name="Mr.Pickles"
), row=1, col=1)

# Add a vertical line at X=61
_ = fig.add_trace(go.Scatter(
    x=[rocky, rocky],
    y=[0, 100],
    mode="lines",
    line=go.scatter.Line(color="orange"),
    showlegend=True,
    name="Rocky"
), row=1, col=1)

_ = fig.add_trace(go.Histogram(x=values_shepherds, showlegend=False, marker_color="blue", marker_opacity=0.6, xbins_size=0.2), row=2, col=1)

# Add a vertical line at X=61
_ = fig.add_trace(go.Scatter(
    x=[basti, basti],
    y=[0, 100],
    mode="lines",
    line=go.scatter.Line(color="yellow"),
    showlegend=True,
    name="Basti"
), row=2, col=1)

# Add a vertical line at X=61
_ = fig.add_trace(go.Scatter(
    x=[rocky, rocky],
    y=[0, 100],
    mode="lines",
    line=go.scatter.Line(color="orange"),
    name="Rocky (as German Shepherd)",
    showlegend=False
), row=2, col=1)

fig.update_xaxes(title_text="Height", row=1, col=1, range=[np.min(values) - 1, np.max(values) + 10])
fig.update_xaxes(title_text="Height", row=2, col=1, range=[np.min(values) - 1, np.max(values) + 10])
fig.update_yaxes(title_text="Count", row=1, col=1)
fig.update_yaxes(title_text="Count", row=2, col=1)
fig.show()
```
