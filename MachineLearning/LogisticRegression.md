# Logistic Regression
from @lichkkkk

[Logistic Regression](https://en.wikipedia.org/wiki/Logistic_regression) is a
linear classification model. In binary classification, we assume the logit
value of probability P(Y=1|X=x) is linear to x:
```
logit(P) = ln(P / 1-P) = -(ax + b) = -[a, b].*[x, 1] = -ax  [Core Assumption]
```
Then we have:
```
P(Y=1|X=x) = 1 / ( 1 + exp(-ax)) = sigmoid(ax)

P(Y=0|X=x) = 1 - P(Y=1|X=x) = 1 - sigmoid(ax)
```
Before go further, let's recap some properties of [sigmoid function](https://en.wikipedia.org/wiki/Sigmoid_function):
```
sigmoid(x) = 1/(1+exp(-x)) = exp(x)/(1+exp(x))
sigmoid(-x) = 1/(1+exp(x)) = 1 - sigmoid(x)
sigmoid(x)' = -sigmoid(x)^2 * -1 * exp(-x) = sigmoid(x) * sigmoid(-x)
```
OK. Now, with the representation of P, we can use [Maximum
Likelihood Estimation](https://en.wikipedia.org/wiki/Maximum_likelihood_estimation) to estimate parameter *a*:
```
Likelihood  = product(P(Y=yi|X=xi)) for all i

P(Y=y|X=x)  = sigmoid(ax)         if y = 1
              (1 - sigmoid(ax))   if y = 0
            = y * sigmoid(ax) + (1 - y) * (1 - sigmoid(ax))
            = y * sigmoid(ax) + (1 - y) * sigmoid(-ax)

Loss  = -log(Likelihood)
      = -sum(log(P(Y=yi|X=xi))) for all i
      = sum(-yi*log(sigmoid(axi)) - (1-yi)*log(sigmoid(-axi)))
```
To maximize likelihood is equal to minimize the Loss. We can use Gradient Descent to minimize the Loss. To do that, let's first get the derivatives of the Loss:
```
log(sigmoid(x))' = 1/sigmoid(x) * sigmoid(x)' = sigmoid(-x)

d(Loss)/da  = sum(-xi * yi * sigmoid(-axi) + xi * (1 - yi) * sigmoid(axi))
            = sum(-xi * yi * (1 - sigmoid(axi)) + xi * (1 - yi) * sigmoid(axi))
            = sum(xi * (sigmoid(axi) - yi))
```
Till here, we can use Gradient Descent to compute the parameter *a*:
```
while criteria > threshold:
  delta_a = -d(Loss)/da * step / #X
  a += delta_a
```
So far we have went through the whole process of Logistic Regression for binary classification. To adapt it to multi-class classification, we can use [softmax function](https://en.wikipedia.org/wiki/Softmax_function) to represent P:
```
Assume there are K classes:
P(Y=k|X=x) = exp(ak*x) / sum(exp(aki*xi)) for all 0 <= i < K
```
With the probability representation, we can use the same approach above for logistic regression.

## Appendix
- A blog about MLE and MAP: https://blog.csdn.net/u011508640/article/details/72815981
- A blog about deriving logistic regression: https://blog.csdn.net/zjuPeco/article/details/77165974
- To understand why we assume P as that logit form, understanding [Generalized Linear Model](https://en.wikipedia.org/wiki/Generalized_linear_model) may be helpful
