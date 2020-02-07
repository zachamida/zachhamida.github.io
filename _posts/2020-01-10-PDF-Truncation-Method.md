---
title: "State-Space Models Constraints Using PDF Truncation (Article in Development)"
date: 2020-01-30 00:34:00 +0800
categories: [Blogging, Tutorial]
tags: [State-Space Models, PDF Truncation]
toc: false
---
## Introduction
State-space models is well suited for time-series analyses. One of the popular state-space models algorithms is Kalman filter (KF). In some applications, the estimates of a Kalman filter are required to be restricted between a range of values. For example, evaluating structural elements with a score from [0-100] over a period of time T.

![Example Data](https://zachamida.github.io/assets/img/posts/post1/ExampleData.png)

Utlizing KF for analyzing a domain restricted time-series data may carry some limitations. This is because the KF  estimates have a Normal distribution. Having a Normal distribution implies that part of the probablity distribution can be outside the feasible domain (i.e. [0 100]) or in some cases, when the variablity is high, the KF estimate itself can be outside the feasible range. This can cause problems in interpreting the outcome of the filter.

One way to solve for this problem to apply the PDF truncation method which was introduced by Simon et. al. [1].

## PDF Truncation
In order to demonstrate how the PDF truncation method works, it is easier to start with the simpliest case, which is a constrained standard Normal PDF $X\sim\mathcal{N}(0,1)$ with constraints $c_1=-1$ and $c_2=0.5$. Figure (1) show the Normal PDF with the constraints.

![Constrained Standard Normal PDF](https://zachamida.github.io/assets/img/posts/post1/ConstNorm.png)

In the PDF Truncation, the area between the bounds $c_1$
and $c_2$ is approximated into a normal PDF. For that end, the area between $c_1$ and $c_2$ is calculated as in,

$A=\int_{c_{1}}^{c_{2}} \frac{1}{\sqrt{2 \pi}} \exp \left(-x^{2} / 2\right) \mathrm{d}x =\frac{1}{2}\left[\operatorname{erf}\left(c_{2} / \sqrt{2}\right)-\operatorname{erf}\left(c_{1} / \sqrt{2}\right)\right]$

For any PDF, it is required that the area under the PDF curve is equal to one. Therefore, the area $A$ is normlized with the term $\alpha$ which is given by,

$\alpha=\frac{1}{A}=\frac{2}{\left[\operatorname{erf}\left(c_{2} / \sqrt{2}\right)-\operatorname{erf}\left(c_{1} / \sqrt{2}\right)\right]}.$

Consequently, the truncated PDF equation is then given by,

$\alpha=\frac{1}{A}=\frac{2}{\left[\operatorname{erf}\left(c_{2} / \sqrt{2}\right)-\operatorname{erf}\left(c_{1} / \sqrt{2}\right)\right]}.$

After obtaininig the new (truncated) PDF equation, the expected value of the new PDF is computed as in,

$\mu =\frac{\alpha}{\sqrt{2 \pi}} \int_{c_{1}}^{c_{2}} x\exp \left(-x^{2} / 2\right) \mathrm{d}x =\frac{\alpha}{\sqrt{2 \pi}}\left[\exp \left(-c_{1}^{2} / 2\right)-\exp \left(-c_{2}^{2} / 2\right)\right].$

And the new variance is calculated as in,

$\sigma^{2}= \frac{\alpha}{\sqrt{2 \pi}} \int_{c_{1}}^{c_{2}}\left(x-\mu\right)^{2} \exp \left(-x^{2} / 2\right) \mathrm{d} x = \frac{\alpha}{\sqrt{2 \pi}}\left[\exp \left(-c_{1}^{2} / 2\right)\left(c_{1}-2 \mu\right)\right.\left.-\exp \left(-c_{2}^{2} / 2\right)\left(c_{2}-2 \mu\right)\right]+\mu^{2}+1.$

Accordingly, the truncated PDF will look as in the figure below,

![Constrained Standard Normal PDF](https://zachamida.github.io/assets/img/posts/post1/TruncNorm.png)

## Generalizing The PDF Truncation Method:
Applying the PDF truncation for higher dimentions (i.e. multivariate Normal distribution) requires additional mathematical operations. The PDF truncation is applied to one of the multivariate distribution dimentions at a time. The method is tractable in a sense that the multivariate distribution is transformed into a one dimentional standard Normal case, similar to what was presented earlier. An example application is by considering a mutivariate Normal distribution with two components ($x:X\sim([\mu_1\; \mu_2]^\intercal,\Sigma)$. The constraints can be expressed as in,

$c_{1} \leq D x_t \leq c_{2}$

The constrained state is defined by $\tilde{x}$. The transformation to one dimentional problem is done through the follwoing equation,

$z_{t}=S W^{-1/2} T^{\intercal}(x_{t}-\tilde{x}_{t})$

where $x_{t}$ is the original state and $\tilde{x}_{t}$ is the constrained state. The matrix $\mathbf{S}$ is obtained from the Gram–Schmidt orthogonalisation (Moon and Stirling 2000) as in,

$S_{t} W_{t}^{1/2} T_{t}^{\intercal} D_{t}=[(D_{t}^{\intercal}\Sigma_{t}D_{t})^{1/2}\; 0\;\cdots\; 0]^{\intercal}$

where $\mathbf{W}$ and $\mathbf{T}$ are obtained from the Jordan canonical as in,

$\mathbf{T}_{t}\mathbf{W}_{t}\mathbf{T}_{t}^{\intercal}=\tilde{\Sigma}_{t}$

The Jordan canonical provides a matrix $\mathbf{W}$ with similar eigen values. For a covariance matrix, the eigenvectors represent the directions of the largest variance of the data, while the eigenvalues represent the magnitude of this variance in those directions.

Following the calculation of the matrices $\mathbf{W}$, $\mathbf{T}$
 and $\mathbf{S}$, the transformed upper bound for a standard Normal distribution is derived as in,

$D^{\intercal} x_t \leq c_{2}\\D^{\intercal} T W^{1/2} S^{\intercal} z_{t}+D^{\intercal} \tilde{x}_{t}  \leq c_{2}$,

$\frac{\left(D^{\intercal} T W^{1 / 2} S^{\intercal}\right) z_{t}}{\left(D^{\intercal}\tilde{\Sigma}_{t} D\right)^{1 / 2}} \leq \frac{c_{2}-D^{\intercal} \tilde{x}_{t}}{\left(D^{\intercal} \tilde{\Sigma}_{t} D\right)^{1 / 2}}$,

$[1\; 0 \cdots 0] z_{t}  \leq \tilde{c}_{2}(k)$

Similarly, the lower bound is given by,

$\left[\begin{array}{cccc}{1} & {0} & {\cdots} & {0}\end{array}\right] z_{t} \geq \frac{c_{1}-D^{\intercal} \tilde{x}_{t}}{\left(D^{\intercal} \tilde{\Sigma}_{t} D\right)^{1 / 2}}$

$[1\;0\cdots0] z_{t}\ge\tilde{c}_{1}(k)$

The transformed upper and lower bounds are applied on a standard Normal PDF ($X\sim\mathcal{N}(0,1)$), in a similar manner to the first example.

The resulting mean $\mu$ and variance $\sigma^2$ are then back-transformed through the following equations,

$\begin{array}{l} {\tilde{x}_{t}=T W^{1 / 2} S^{\intercal} \tilde{\mu}+{x}_{t}} \\ {\tilde{\Sigma}_{t}=T W^{1 / 2} S^{\intercal} \tilde{\sigma}_{t}^2 S W^{1 / 2} T^{\intercal}} \end{array}$

## Limitations:
1- The expected value of the constrained state is shifted towards the feasible region. This may cause a biase in the constrained estimate.

## Matlab Code:
[1] https://academic.csuohio.edu/simond/kalmanconstrained/

[2] https://github.com/zachamida/ConstrainedKalmanFilter

### Resources:

[1] Dan Simon and Donald L Simon. Constrained kalman filtering via density function truncation for turbofan engine health estimation. International Journal of Systems Science, 41(2):159–171.

[2] Khan Academy: https://www.khanacademy.org/math/linear-algebra/alternate-bases/orthonormal-basis/v/linear-algebra-the-gram-schmidt-process


