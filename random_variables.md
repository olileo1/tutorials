# Random Variables
## Theoretical Concept
A random variable is a theoretical construct used in probability and statistics to model real-world phenomena. Random Variables are the key principle of statistics (and subsequently of most, if not all, Machine Learning Models). The key idea is, that every observation/data point we see is just the a random realization of a Random Variable. Let's consider this basic example:

To most of us there is no doubt that in coin tossing the probability of the coin showing **head** is 50%. But have you ever thought about why this is? Maybe there is some scientific physical derivation of the law "In a coin toss the probability for **head** is 50% and the probability for **tail** is also 50%", but for the common human being it is just easier to just toss the coin $n$ times and then calculate the percentage of **head**. After that you will be very certain, that the probability for **head** is 50%, even if you don't know the physics behind it. So now you have a Model in your head when it comes to coin tossing and you can use this model. For example you would never take the bet "if head I pay you 10 if tail you pay me 30", because it would be unfair to have different payoffs for the same likelihood of winning.

In probability theory the coin toss is modelled as a random variable, usually denoted as $X$. So $X$ is the theoretical modell of the coin toss. Once you toss the coin and see a result you have an observation which is normally denoted as $x$. There is a very formal mathematical definition of a random variable, but as a simplification, we can say that a random variable represents an entire set of possible outcomes and their associated probabilities:

* **a set of possible outcomes**: For the coin toss it is {head, tail}. So we can say, if $x$ is a realization of $X$ ($x \sim X$), then $x$ can take the values $x=\text{head}$ or $x=\text{tail}$
* **a probability distribution**: So we can say, the probability that any realization of $X$ shows head is 50% ($P[X=\text{head}] = 0.5$)

---
**NOTE**: I will only focus on continuous random variables. There is also the concept of concrete random variables

---

## Random Variables and Machine Learning/Statistical Modelling
Let's consider the Coin Tossing again. Imagine I offer you the following:

You can watch me toss a coin for a 1000 times. Then you are allowed to analyse the data of my 1000 coin tosses. After that you are allowed to place a bet on my next coin toss.

So here is what will probably happen:

1. I toss the coin a 1000 times and at each toss you write down the result. So your data looks like this: head, tail, taill, head, head, head, tail, tail, ...
2. After my 1000th toss you have the count of how many times I tossed head and how many times I tossed tail. If it is a normal coin and everything is as usual, there will be roughly 500 times head and 500 times tail. If the coin is special for some reason you might end up with 800 **head** and 200 **tail**. You don't have enough time and information and also not the education in theoretical physics to being able to explain why the results are the way they are. But you trust me, that I don't cheat, so you are comfortable with your model, that tells you: the probability that the next toss will be **head** is the calculated percentage of **head** from the previous 1000 tosses. So your model takes the form of the following Random Variable $X$, with $P[X=\text{head}] = \frac{\text{number of head in observations}}{1000}$ and $P[X=\text{tail}] = \frac{\text{number of tail in observations}}{1000}$
3. Now, we are the point where I offer you the bet: You can bet \\$10 on any outcome (head or tail). If you guess correctly you get \\$20. I toss the coin exactly the same time as the previus 1000 times. I don't cheat and you don't have any reason to believe that this coin toss will be any different then the ones before. If 800 of the last 1000 tosses were head, your model will tell you: "If you bet on head now, you will have an 80% chance of winning", and you will probably take the bet. If heads only occured 500 times you will probably think twice if you take a bet at all

So let's look at the three steps from a Machine Learning perspective:

1. We got some data
2. We trained our model (obviously a very easy and very simple model)
3. We used the model to make predictions about future events, for which we don't know the outcome yet

The **Moral of the Story** is, that almost (if not all) Machine Learning tasks can be considered as: Deriving a Theoretical Model, that can be formulate using the concept of Random Variables, using existing data. Using the probability distribution of this Model to predict future outcomes.

Of course when it comes to Machine Learning the models are much more complex, than the example with the coin toss, because you need to consider stuff like **dependent probabilities** and **multivariate random variables**.

## Continouos Random Variables
For now I want to focus on onedimensional continouos random variables. Continuous random variables describe outcomes in probabilistic situations where the possible values some quantity can take form a continuum, which is often (but not always) the entire set of real numbers $\mathbb{R}$.

### Example

Consider a random experiment where we measure the height of individuals in a population. Let $X$ be the random variable representing the height of an individual. The possible values of $X$ form a continuum, as height can take on any value within a certain range. For example 170.5cm or 182.345cm.

### Probability Density Function (PDF)
Random Variables are mostly described by their probability density functions (PDF).

The probability density function (PDF) $f(x)$ describes the likelihood of the random variable $X$ taking on a any value in a particular interval $[c_1, c_2)$. If we know the PDF $f(x)$ of a random variable $X$ we can calculate the probability that a realisation $x$ of $X$ lies in the interval $[c_1, c_2)$ by calculating the Integral:

$$
\int_{c_{1}}^{c_{2}} f(t) \, dt = P(c_1 \leq X < c_2)
$$

The PDF is bascially the mathematical generalisation of a histogram.

### Cumulative Distribution Function (CDF)
A concept that is closely related to the PDF is the concept of the CDF. The cumulative distribution function (CDF), denoted as $F(x)$, represents the probability that the random variable $X$ takes on a value less than or equal to $c$. The CDF is obtained by integrating the PDF:

$$F(c) = \int_{-\infty}^{c} f(t) \, dt = P(X < c)$$

#### Interactive Plot
In the following interactive Plot you see how a histogram (something from the real world) relates to the theoretical concept of a probability distribution function. The histogram plots the histogram from a random sample from normal distributed values together with the PDF of a normal distribution.
It is also possible to use histograms to heuristically explain why the probability that an observations $x$ of $X$ lies in the interval $[c_1, c_2)$ can be calculated using the integral. In the histogram below, the height of the bar equals the percentage of observations that lie between the left and right bound of the bar. This is roughly the area under the PDF curve in that interval.

Below the CDF with the histogram you can see the CDF together with the empirical CDF (ECDF) calculated on the same random sample as the histogram.

```python
import plotly.graph_objs as go
from plotly.subplots import make_subplots
import numpy as np
from scipy.stats import norm
from ipywidgets import interactive, HBox, VBox, widgets

def create_histogram_density_plot(n):
    observations = np.random.normal(loc=0, scale=1, size=n)
    # Calculate points for the PDF
    x = np.linspace(min(observations), max(observations), 1000)
    pdf = norm.pdf(x)
    cdf = norm.cdf(x)

    fig = make_subplots(rows=2, cols=1, shared_xaxes=True)
    # Create histogram trace with density instead of count
    _ = fig.add_trace(
        go.Histogram(x=observations, nbinsx=min(int(n / 25), 500), opacity=0.75, name='Observations', histnorm='probability density'),
        row=1, col=1
    )
    _ = fig.add_trace(
        go.Scatter(x=x, y=pdf, mode='lines', name='PDF'),
        row=1, col=1
    )
    _ = fig.add_trace(
        go.Scatter(x=x, y=cdf, mode='lines', name='CDF'),
        row=2, col=1
    )
    _ = fig.add_trace(
        go.Scatter(x=np.sort(observations), y=np.arange(n) / n, mode='lines', name='ECDF'),
        row=2, col=1
    )
    return fig

# Function to update the plot
def update_plot(n):
    fig = create_histogram_density_plot(n)
    fig.show()

# Create an interactive widget for the plot
n_widget = widgets.IntSlider(value=1000, min=250, max=5000, step=100, continuous_update=False)
interactive_plot = interactive(update_plot, n=n_widget)

# Display the widgets
display(interactive_plot)
```

### Expected Value of a Random Variable
The Expected Value of a Random Variable is defined as $$E(X) = \int_{-\infty}^{\infty} x f(x) \, dx$$
So the Expected Value is basically a mathematical generalisation to the **sample mean**.

### Variance of a Random Variable
The Variance of a Random Variable is defined as $$ \text{Var}(X) = \int_{-\infty}^{\infty} (x - E(X))^2 f(x) \, dx $$
Again this is basically the mathematical generalisation of the **sample variance**. Given the variance $\text{Var}(X)$, the standard deviation if defined as the square root of the variance $\sqrt{\text{Var}(X)} = \text{SD}(X)$

# Normal Distribution

The **Normal Distribution**, also known as the **Gaussian Distribution**, is a continuous probability distribution that is symmetrical around its mean. It is one of the most important distributions in statistics and is often used to represent real-valued random variables whose distributions are not known. If we assume that a random variable $X$ is normal distributed we denote this as $X\sim N(\mu, \sigma)$


## Probability Density Function (PDF)

The probability density function (PDF) of a normal distribution is given by:

$$
f(x | \mu, \sigma^2) = \frac{1}{\sqrt{2 \pi \sigma^2}} e^{-\frac{(x - \mu)^2}{2 \sigma^2}}
$$

where:
- $\mu$ is the mean
- $\sigma$ is the standard deviation

## Standard Normal Distribution

A special case of the normal distribution is the **standard normal distribution**, which has a mean of 0 and a standard deviation of 1. The PDF of the standard normal distribution is:

$$
f(z) = \frac{1}{\sqrt{2 \pi}} e^{-\frac{z^2}{2}}
$$

The PDF can be used to tell you the probability, that an observations of a standard normal distribution lies in a certain interval. For example the Probability that a random generated value $x$ of a standard normal distribution is bigger and $-1$ and smaller than $1$ an be calculated like this:

$$
P(-1 \leq X < 1) = \int_{-1}^{1} \frac{1}{\sqrt{2 \pi}} e^{-\frac{t^2}{2}} \, dt
$$

## Relationship between PDF and CDF

The **cumulative distribution function (CDF)** of a normal distribution is the integral of the probability density function (PDF). It represents the probability that a random variable \( X \) takes on a value less than or equal to \( x \). The CDF is given by:

$$
F(x) = P(X \leq x) = \int_{-\infty}^{x} f(t) \, dt
$$

For the standard normal distribution, the CDF is often denoted by $\Phi(z)$ and is given by:

$$
\Phi(z) = \int_{-\infty}^{z} \frac{1}{\sqrt{2 \pi}} e^{-\frac{z^2}{2}} \, dt
$$

```python
import plotly.graph_objs as go
import numpy as np
import scipy.stats as stats

# Define the range of x values
x = np.linspace(-10, 10, 1000)

# Define different values for mu and sigma
parameters = [(0, 1), (0, 2), (2, 1), (-2, 1)]

# Create a figure
fig = go.Figure()

# Add traces for each combination of mu and sigma
for mu, sigma in parameters:
    y = stats.norm.pdf(x, mu, sigma)
    fig.add_trace(go.Scatter(x=x, y=y, mode='lines', name=f'mu={mu}, sigma={sigma}'))

# Update layout for a better view
fig.update_layout(
    title='Normal Distribution Density Functions',
    xaxis_title='x',
    yaxis_title='Probability Density',
    legend_title='Parameters'
)

# Show the plot
fig.show()
```

```python
import plotly.graph_objs as go
from plotly.subplots import make_subplots
import numpy as np
import scipy.stats as stats
import plotly.graph_objs as go
from ipywidgets import interactive, HBox, VBox, widgets

def plot_pdf_and_cdf(mu, sigma, x):
    # Define the range of x values
    x_values = np.linspace(-10, 10, 1000)

    # Calculate the y values for the normal distribution
    y_values = stats.norm.pdf(x_values, mu, sigma)

    # Calculate the y values for the cumulative distribution function
    cdf_values = stats.norm.cdf(x_values, mu, sigma)

    # Create subplots
    fig = make_subplots(rows=2, cols=1, subplot_titles=('Density Function', 'Cumulative Distribution Function'))

    # Add the density function trace to the first subplot
    fig.add_trace(go.Scatter(x=x_values, y=y_values, mode='lines', name='Density Function'), row=1, col=1)

    # Fill the area left of x in the first subplot
    fill_area = np.linspace(-10, x, 1000)
    fill_y = stats.norm.pdf(fill_area, mu, sigma)
    fig.add_trace(go.Scatter(x=fill_area, y=fill_y, fill='tozeroy', mode='none', fillcolor='rgba(0,100,80,0.2)', name='Area under curve'), row=1, col=1)

    # Add the cumulative distribution function trace to the second subplot
    fig.add_trace(go.Scatter(x=x_values, y=cdf_values, mode='lines', name='CDF'), row=2, col=1)

    # Add a vertical line at x in the first subplot
    fig.add_shape(type='line', x0=x, y0=0, x1=x, y1=stats.norm.pdf(x, mu, sigma), line=dict(color='Red',), xref='x1', yref='y1', row=1, col=1)

    # Add a horizontal line at the CDF value for x in the second subplot with dotted line style
    fig.add_shape(type='line', x0=-10, y0=stats.norm.cdf(x, mu, sigma), x1=x, y1=stats.norm.cdf(x, mu, sigma), line=dict(color='Red', dash='dot'), xref='x2', yref='y2', row=2, col=1)

    # Add a vertical line at x in the second subplot
    fig.add_shape(type='line', x0=x, y0=0, x1=x, y1=stats.norm.cdf(x, mu, sigma), line=dict(color='Red', dash='dot'), xref='x2', yref='y2', row=2, col=1)

    # Update layout for a better view
    fig.update_layout(
        title=r'$\text{Normal Distribution Density and Cumulative Distribution Functions }(\mu=\text{%.2f}, \sigma=\text{%.2f}) \\ \Phi(%f)=%f$' % (mu, sigma, x, stats.norm.cdf(x, mu, sigma)),
        showlegend=False
    )
    return fig

# Create interactive widgets for x, y, z
mu_widget = widgets.FloatSlider(value=0, min=-10, max=10, step=0.1, description='mu', continuous_update=False)
sigma_widget = widgets.FloatSlider(value=1, min=0.1, max=10, step=0.1, description='sigma', continuous_update=False)
z_widget = widgets.FloatSlider(value=0, min=-10, max=10, step=0.1, description='x', continuous_update=False)

# Function to update the plot
def update_plot(mu, sigma, x):
    fig = plot_pdf_and_cdf(mu=mu, sigma=sigma, x=x)
    fig.show()

# Create an interactive widget for the plot
interactive_plot = interactive(update_plot, mu=mu_widget, sigma=sigma_widget, x=z_widget)

# Display the widgets
display(interactive_plot)
```
