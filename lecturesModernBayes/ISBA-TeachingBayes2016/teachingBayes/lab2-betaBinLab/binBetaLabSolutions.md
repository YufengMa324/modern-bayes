Teaching Bayes: A Binomial-Beta Lab with Solutions
================================================
author: Rebecca C. Steorts
date: June 11, 2016
css: multisymbols.sty
width: 1440
height: 900


Beta-Bernoulli model
================================================
Recall the Beta-Bernoulli model:
$$
\begin{aligned}
X\mid \theta &\sim \text{Bernoulli}(\theta)\\
\theta &\sim \text{Beta}(a,b)
\end{aligned}
$$
where $a,b$ are fixed parameters. 

Goals:
-  Let's determine whether the probability that a worker will fake an illness is truly 1 percent. 
- Your task is to assist me!
- Let's outline our tasks and then solve them.


Task 1 
================================================
- Simulate some data using the \textsf{rbinom} function of size $n = 100$ and probability equal to 1\%. 
- In order to replicate results, let's set our seed using \textsf{set.seed(123)}.

Task 2
================================================
- Write a function that takes as its inputs the data you simulated above and a sequence of $\theta$ values of length 1000 and produces Likelihood values based on the Binomial Likelihood. 
- Plot your sequence and its corresponding Likelihood function.

Task 3 
================================================
- Write a function that takes as its inputs  prior parameters \textsf{a} and \textsf{b} for the Beta-Bernoulli model and the observed data, and produces the posterior parameters you need for the model. 
- Generate the posterior parameters for a non-informative prior i.e. \textsf{(a,b) = (1,1)} and for an informative case \textsf{(a,b) = (3,1)}.

Task 4
================================================
- Create two plots, one for the informative and one for the non-informative case to show the posterior distribution and superimpose the prior distributions on each along with the likelihood. 
- What do you see? (Remember to turn the y-axis ticks off since superimposing may make the scale non-sense). 

Task 5
================================================
- Based on the informative case, generate a 95\% credible interval with 1000 posterior draws and a 95\% confidence interval for your parameter of interest, and use \textsf{xtable} to output these. What is the problem?
- Based on the data you simulated, do you conclude that the true value higher or lower than 1\%?

Recap
================================================
Let's review the tasks and break them down into the most 
important information in each task. 


Task 1: Simulate data
================================================
- Simulate input data using the Beta-Bernoulli model
- $n=100$, 
- $p=0.01$, 
- Set a seed.

Task 2: Plot Binomial LH
================================================
- Write a function called \textrf{myBernLH}. 
- Inputs = simualted data and sequence of $\theta$ values with length 1000.
- Output: Likelihood values based upon the Binomial likelihood. 
- Plot the Binomial likelihood as a function of $\theta.$

Task 3: Generate posterior
================================================
- Write a function called \textrf{myPosteriorParam}. 
- Let the input parameters be \textrf{a,b} and the simulated input data. 
- The output should be the posterior parameter for the Beta-Bernoulli model. 
- Generate the posterior parameters for $(a,b) = (1,1)$ and $(3,1).$

Task 4: Posterior comparisons
================================================
- Create two plots based upon task 3 showing the posterior distributions. 
- Show these distributions on the same plot with the likelihood. 
- What do you observe? 

Task 5: Credible intervals
================================================
- Based on the informative case for $a,b$ generate a 95 percent credible interval with 1000 posterior draws and a 95 percent confidence interval for your parameter of interest. 
- What is the problem?
- Based on your simulated data, do you conclude that the true value is higher or lower thatn 1 percent?

Solutions
================================================
We now provide solutions to the five tasks. 

Task 1: Simulate data
================================================
The data can be simulated as follows:

```r
set.seed(123)
obs.data <- rbinom(n = 100, size = 1, prob = 0.01)
```

Task 2: Plot Binomial LH
================================================
- We write a function called myBernLH to produce likelihood Bernoulli 
likelihood values. 


```r
### Bernoulli LH Function ###
# Input: the observed data, theta grid #
# Output: Produces likelihood values #
myBernLH <- function(obs.data, theta){
	N <- length(obs.data)
	x <- sum(obs.data)
	LH <- ((theta)^x)*((1 - theta)^(N-x))
	return(LH)
}
```

Task 2: Plot Binomial LH (continued)
================================================
  We now plot the likelihood as a function of $\theta.$

```r
### Plot LH for a grid of theta values ###
# Create the grid #
theta.sim <- seq(from = 0, to = 1, length.out = 1000)
# Store the LH Values #
sim.LH <- myBernLH(obs.data = obs.data, theta = theta.sim)
# Create the Plot #
```

Task 2: Plot Binomial LH (continued)
================================================
<img src="binBetaLabSolutions-figure/unnamed-chunk-4-1.png" title="plot of chunk unnamed-chunk-4" alt="plot of chunk unnamed-chunk-4" style="display: block; margin: auto;" />

Task 3: Generate posterior
================================================


```r
### Function to determine posterior parameters based on ###
### observed data and prior assumptions ###
# Inputs - Prior Paramters, observed data #
myPosteriorParam <- function(pri.a, pri.b, obs.data){
	N <- length(obs.data)
	x <- sum(obs.data)
	post.a <- pri.a + x
	post.b <- pri.b + N - x
	post.param <- list('post.a' = post.a, 
	 'post.b' = post.b)
	return(post.param)
}
```

Task 3: Generate posterior (continued)
================================================

```r
# Find posterior parameters for two different priors #
# a = 1, b = 1 #
non.inform <- myPosteriorParam(pri.a = 1, pri.b = 1, obs.data = obs.data)
inform <- myPosteriorParam(pri.a = 3, pri.b = 1, obs.data = obs.data)
# a = 3, b = 1 #
```

Task 3: Generate posterior (continued)
================================================

```r
# print the output values
print(c(non.inform,inform))
```

```
$post.a
[1] 2

$post.b
[1] 100

$post.a
[1] 4

$post.b
[1] 100
```

Task 4: Posterior comparisons
================================================
We first create non-informative and informative priors and posteriors

```r
### Create a plot of LH, Pri, Posterior using the simulated seq ###
non.inform.den <- dbeta(x = theta.sim, shape1 = non.inform$post.a, 
 shape2 = non.inform$post.b)
inform.den <- dbeta(x = theta.sim, shape1 = inform$post.a, 
 shape2 = inform$post.b)
pri.inform <- dbeta(x = theta.sim, shape1 = 3, 
 shape2 = 1)
pri.non.inform <- dbeta(x = theta.sim, shape1 = 1, 
 shape2 = 1)
```


Task 4: Posterior comparisons 
================================================
 <img src="binBetaLabSolutions-figure/unnamed-chunk-9-1.png" title="plot of chunk unnamed-chunk-9" alt="plot of chunk unnamed-chunk-9" style="display: block; margin: auto;" />

What do you observe? 

Task 5: Credible Intervals
================================================

```r
require(xtable)
## Create confidence/ credible intervals - informative ##
# Credible Interval #
my.sim <- rbeta(n = 1000, shape1 = inform$post.a, 
 shape2 = inform$post.b)
my.credI <- quantile(my.sim, prob = c(0.025, 0.975))
```

Task 5: Confidence Intervals 
================================================

```r
# Confidence Interval #
p.hat <- sum(obs.data) / length(obs.data)
my.confI <- p.hat + qnorm(p = c(0.025, 0.975)) * 
 sqrt(p.hat*(1-p.hat)/length(obs.data))
# Store the results # 
results <- rbind(my.credI, my.confI)
rownames(results) <- c('Credible Interval', 'Confidence Interval')
# present these results in a nice table #
```
Task 5: Credible Interval Compared with Confidence Interval
================================================

```r
print(xtable(results, digits = 4), comment = FALSE)
```

\begin{table}[ht]
\centering
\begin{tabular}{rrr}
  \hline
 & 2.5\% & 97.5\% \\ 
  \hline
Credible Interval & 0.0107 & 0.0799 \\ 
  Confidence Interval & -0.0095 & 0.0295 \\ 
   \hline
\end{tabular}
\end{table}

Food for Thought
================================================
- Did you find the lab too long?
- What do you anticipate students might have trouble with?
- How could you use this material to have students complete a homework
- How could you use the lab to incorporate real data? 
- I'm finding that two-three tasks is reasonable and the last two
are better as a follow up for homework. 
- This also keeps the students engaged in the course and makes it flow well. 
 





