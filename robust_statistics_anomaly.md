Jupyter Notebook
Untitled
Last Checkpoint: vor ein paar Sekunden
(autosaved)
Current Kernel Logo
voice_packet_core_alarming_ufkr7k6 
File
Edit
View
Insert
Cell
Kernel
Widgets
Help

Markdown
}
# Anomalie-Detection and Robust Statistics: An Example based on Gaussian Distribution
​
---
**NOTE for Beta Readers**:
This article requires an understanding of Statistical Models and the knowledge of some basic Statistical/Probability Theoretical Notation. I think it is best to either read the wikipedia articel on Statistical Models first or have it opened side by side to this article
​
---
​
[Robust statistics are statistics that maintain their properties even if the underlying distributional assumptions are incorrect.](https://en.wikipedia.org/wiki/Robust_statistics)
​
Not many Data Science practitioners, that I have met heard about the field of **Robust Statistics**. I myself worked as a Data Scientist for 2 years, creating forecast models/etc., before I first heard about that field. At first the field seemed very abstract to me and more like scientific tinkering than anything applicable in Real World Problems. But once I got into it, I saw the importance of Robust Statistics for the task of Anomaly Detection and now I am convinced, that the key to solid Anomaly Detection methodology lies in the understanding of Robust Statistics. In this article I want to highlight the importance of that field for Anomaly Detection.
​
## Revisiting the principles of Anomaly Detection
Let's remember shortly the principles of Anomaly Detection (I will write a Section on that shortly but for now you can check the wikipedia article on [Statistical Model](https://en.wikipedia.org/wiki/Statistical_model)):
​
1. First we start with formulating our Assumptions in the form of a Statistical Model: $(\mathcal{S}, \mathcal{P})$, with $\mathcal{P}=\left\{F_{\theta}: \theta \in \Theta\right\}$. For example:
    - We assume the height of female Dalmatians (dogs) is normal distributed with unknown parameters $\mu$ and $\sigma$. So $\mathcal{S} = \mathbb{R}$ and $\mathcal{P}=\left\{\mathcal{N}(\mu, \sigma): \mu \in \mathbb{R}, \sigma > 0\right\}$
    - We assume their is a linear relationship between the height and the weight of female Dalmatians (dogs) like this: $\text{weight} = \alpha + \beta\cdot\text{height} + \varepsilon$, where we assume that $\varepsilon \sim \mathcal{N}(0, \sigma)$. Given this Statistical Model (which is the Standard Form for Linear Regression) it is way more complicated to write down $\mathcal{S}$ and $\mathcal{P}$ in set form. (**For now you have to believe me, that it is possible, and wait for my article where I explain Statistical Models in General**)
2. Then we fit the Parameters of our Statistical Model using an appropriate Methodology. So we get our fitted model $\hat{\mathcal{P}} = F_{\hat{\theta}}$. For example:
    - Given our data $X$ (that could look like this: 56, 65, 71, 43, 23, ...), we estimate $\mu$ using the standard formula for the sample mean. So $\hat{\mu} = \frac{1}{n}\sum\limits_{i=1}^{n} x_{i}$. To estimate $\sigma$ we use the standard formula for the sample standard deviation $\hat{\sigma}^{2}=\frac{1}{n-1}\sum\limits_{i=1}^{n-1} (x_{i} - \hat{\mu})^{2}$. Our fitted model, then looks like this $F_{\hat{\theta}} \sim \mathcal{N}(\hat{\mu}, \hat{\sigma})$
    - Given our data on dog weights and heights $X$ (that could look like this: (56, 23), (65, 35), (71, 41), ...), we estimate $\alpha$, $\beta$ and $\sigma$ using the standard method for Linear Regression. So our fitted model, then looks like this: $\text{weight} = \hat{\alpha} + \hat{\beta}\cdot\text{height} + \varepsilon$, with $\varepsilon \sim \mathcal{N}(0, \hat{\sigma})$
3. Now that we have our fitted Statistical Model $\hat{\mathcal{P}}$  we can use that Model to assess for any observation how likely it is, that the observation could be a random observation from that Model. For example:
    - Given our fitted model for the heights of female Dalmatians $F_{\hat{\theta}}$, we can assess the likelihood that any number $x$ is a plausible random observation from that distribution or not like this $P[|x| > F_{\hat{\theta}}] = 1 - \Phi(|\frac{x - \hat{\mu}}{\hat{\sigma}}|)$ (Here $\Phi$ is the CDF of the standard gaussian distribution, see article on the normal distribution). If that value is very small, the observation is far away from the majority of observations and therefore seems to be anomalous.
    - Given our fitted model $\text{weight} = \hat{\alpha} + \hat{\beta}\cdot\text{height} + \varepsilon$, we can asses for any observation $x = (\text{weight}, \text{height})$, the plausbility of that observation using $1- \Phi(|\frac{\text{weight} - \hat{\alpha} - \hat{\beta}\cdot\text{height}}{\hat{\sigma}}|)$ If that value is very small, that mean the observation is very far away from the regression line and therefore seems to be anomalous.
import numpy as np
import plotly.graph_objs as go
from plotly.subplots import make_subplots
import scipy.stats as stats
​
​
# Calculate the standard deviation for 95% of observations to lie between 58 and 62
# For a normal distribution, 95% of the data lies within 1.96 standard deviations from the mean
mean = 56
std_dev = 1
​
np.random.seed(43)
# Generate 1000 random normal distributed values
normal_dalmatians = np.random.normal(loc=mean, scale=std_dev, size=1000)
penny = 61
values = np.concatenate([
    normal_dalmatians,
    np.array([penny]),
])
​
mu_estimate = np.mean(values)
sigma_estimate = np.std(values)
​
fig = make_subplots(
    rows=2,
    cols=1,
    shared_xaxes=True,
    subplot_titles=(
        f'Histogram of Heights for Female Dalmatians',
        f'Probability function for different Values'
    )
)
_ = fig.add_trace(go.Histogram(x=values, showlegend=False, marker_color="blue", marker_opacity=0.6, xbins_size=0.2), row=1, col=1)
_ = fig.add_vline(x=mu_estimate, line_width=5, line_dash="dash", line_color="green")
_ = fig.add_vline(x=mu_estimate + 2.58 * sigma_estimate, line_width=3, line_color="red")
_ = fig.add_vline(x=mu_estimate - 2.58 * sigma_estimate, line_width=3, line_color="red")
​
x_values = np.linspace(51, 61, 100)
y_values = np.abs(x_values - mu_estimate) / sigma_estimate
quantiles = stats.norm.cdf(y_values)
_ = fig.add_trace(go.Scatter(x=x_values, y=(1 - quantiles) * 2, showlegend=False), row=2, col=1)
_ = fig.update_yaxes(type="log", row=2, col=1)
fig.update_layout(height=800)
import numpy as np
import plotly.graph_objs as go
from plotly.subplots import make_subplots
import scipy.stats as stats
import plotly.figure_factory as ff
​
def linear_regression(x, y):
    # Number of observations
    n = len(x)
    
    # Calculate the means of x and y
    mean_x = np.mean(x)
    mean_y = np.mean(y)
    
    # Calculate the terms needed for the numerator and denominator of beta
    xy = x * y
    xx = x * x
    
    # Calculate beta (slope)
    beta = (n * np.sum(xy) - np.sum(x) * np.sum(y)) / (n * np.sum(xx) - np.sum(x)**2)
    
    # Calculate alpha (intercept)
    alpha = mean_y - beta * mean_x
    
    # Calculate the residuals
    residuals = y - (alpha + beta * x)
    
    # Calculate sigma (standard deviation of the residuals)
    sigma = np.sqrt(np.sum(residuals**2) / (n - 2))
    
    return alpha, beta, sigma
​
​
# Calculate the standard deviation for 95% of observations to lie between 58 and 62
# For a normal distribution, 95% of the data lies within 1.96 standard deviations from the mean
mean = 56
std_dev = 1
​
np.random.seed(43)
# Generate 1000 random normal distributed values
n = 1000
normal_dalmatians = np.random.normal(loc=mean, scale=std_dev, size=n)
penny_height = 61
penny_weight = 34
betty_height = 58
betty_weight = 31
heights = np.concatenate([
    normal_dalmatians,
    np.array([penny_height]),
    np.array([betty_height]),
])
weights = np.concatenate([
    10 + 0.4 * heights[:n] + np.random.normal(loc=0, scale=0.2, size=n),
    np.array([penny_weight]),
    np.array([betty_weight]),
])
​
alpha, beta, sigma = linear_regression(heights, weights)
x_values = np.linspace(np.min(heights), np.max(heights), 1000)
y_values = alpha + beta * x_values
residuals = (weights - alpha - heights * beta) / sigma
​
fig = make_subplots(rows=1, cols=2, subplot_titles=("Scatter Plot for Height-Weight Relationship", "Histogram of Scaled Residuals"))
_ = fig.add_trace(go.Scatter(x=heights[:-1], y=weights[:-1], mode="markers", showlegend=False), row=1, col=1)
_ = fig.add_trace(go.Scatter(x=[heights[-1]], y=[weights[-1]], mode="markers", showlegend=False, marker=dict(color='red')), row=1, col=1)
_ = fig.add_trace(go.Scatter(x=x_values, y=y_values, mode="lines", showlegend=False), row=1, col=1)
​
​
# Create the distplot
hist_data = [residuals]
group_labels = ['residuals']
distplot = ff.create_distplot(hist_data, group_labels, show_hist=True, show_rug=True)
​
# Add the distplot to the second subplot
for trace in distplot['data']:
    _ = fig.add_trace(trace, row=1, col=2)
​
fig.add_vline(x=np.min(residuals), line=dict(color="Red", width=2), row=1, col=2)
fig.update_layout(height=800, showlegend=False)


```python
import numpy as np
import plotly.graph_objs as go
from plotly.subplots import make_subplots
import scipy.stats as stats


# Calculate the standard deviation for 95% of observations to lie between 58 and 62
# For a normal distribution, 95% of the data lies within 1.96 standard deviations from the mean
mean = 56
std_dev = 1

np.random.seed(43)
# Generate 1000 random normal distributed values
normal_dalmatians = np.random.normal(loc=mean, scale=std_dev, size=1000)
penny = 61
values = np.concatenate([
    normal_dalmatians,
    np.array([penny]),
])

mu_estimate = np.mean(values)
sigma_estimate = np.std(values)

fig = make_subplots(
    rows=2,
    cols=1,
    shared_xaxes=True,
    subplot_titles=(
        f'Histogram of Heights for Female Dalmatians',
        f'Probability function for different Values'
    )
)
_ = fig.add_trace(go.Histogram(x=values, showlegend=False, marker_color="blue", marker_opacity=0.6, xbins_size=0.2), row=1, col=1)
_ = fig.add_vline(x=mu_estimate, line_width=5, line_dash="dash", line_color="green")
_ = fig.add_vline(x=mu_estimate + 2.58 * sigma_estimate, line_width=3, line_color="red")
_ = fig.add_vline(x=mu_estimate - 2.58 * sigma_estimate, line_width=3, line_color="red")

x_values = np.linspace(51, 61, 100)
y_values = np.abs(x_values - mu_estimate) / sigma_estimate
quantiles = stats.norm.cdf(y_values)
_ = fig.add_trace(go.Scatter(x=x_values, y=(1 - quantiles) * 2, showlegend=False), row=2, col=1)
_ = fig.update_yaxes(type="log", row=2, col=1)
fig.update_layout(height=800)
```

```python
import numpy as np
import plotly.graph_objs as go
from plotly.subplots import make_subplots
import scipy.stats as stats
import plotly.figure_factory as ff

def linear_regression(x, y):
    # Number of observations
    n = len(x)
    
    # Calculate the means of x and y
    mean_x = np.mean(x)
    mean_y = np.mean(y)
    
    # Calculate the terms needed for the numerator and denominator of beta
    xy = x * y
    xx = x * x
    
    # Calculate beta (slope)
    beta = (n * np.sum(xy) - np.sum(x) * np.sum(y)) / (n * np.sum(xx) - np.sum(x)**2)
    
    # Calculate alpha (intercept)
    alpha = mean_y - beta * mean_x
    
    # Calculate the residuals
    residuals = y - (alpha + beta * x)
    
    # Calculate sigma (standard deviation of the residuals)
    sigma = np.sqrt(np.sum(residuals**2) / (n - 2))
    
    return alpha, beta, sigma


# Calculate the standard deviation for 95% of observations to lie between 58 and 62
# For a normal distribution, 95% of the data lies within 1.96 standard deviations from the mean
mean = 56
std_dev = 1

np.random.seed(43)
# Generate 1000 random normal distributed values
n = 1000
normal_dalmatians = np.random.normal(loc=mean, scale=std_dev, size=n)
penny_height = 61
penny_weight = 34
betty_height = 58
betty_weight = 31
heights = np.concatenate([
    normal_dalmatians,
    np.array([penny_height]),
    np.array([betty_height]),
])
weights = np.concatenate([
    10 + 0.4 * heights[:n] + np.random.normal(loc=0, scale=0.2, size=n),
    np.array([penny_weight]),
    np.array([betty_weight]),
])

alpha, beta, sigma = linear_regression(heights, weights)
x_values = np.linspace(np.min(heights), np.max(heights), 1000)
y_values = alpha + beta * x_values
residuals = (weights - alpha - heights * beta) / sigma

fig = make_subplots(rows=1, cols=2, subplot_titles=("Scatter Plot for Height-Weight Relationship", "Histogram of Scaled Residuals"))
_ = fig.add_trace(go.Scatter(x=heights[:-1], y=weights[:-1], mode="markers", showlegend=False), row=1, col=1)
_ = fig.add_trace(go.Scatter(x=[heights[-1]], y=[weights[-1]], mode="markers", showlegend=False, marker=dict(color='red')), row=1, col=1)
_ = fig.add_trace(go.Scatter(x=x_values, y=y_values, mode="lines", showlegend=False), row=1, col=1)


# Create the distplot
hist_data = [residuals]
group_labels = ['residuals']
distplot = ff.create_distplot(hist_data, group_labels, show_hist=True, show_rug=True)

# Add the distplot to the second subplot
for trace in distplot['data']:
    _ = fig.add_trace(trace, row=1, col=2)

fig.add_vline(x=np.min(residuals), line=dict(color="Red", width=2), row=1, col=2)
fig.update_layout(height=800, showlegend=False)
```
