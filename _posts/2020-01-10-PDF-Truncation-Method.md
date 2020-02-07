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
In order to demonstrate how the PDF truncation method works, it is easier to start with the simpliest case, which is a constrained standard Normal PDF ![$X\sim\mathcal{N}(0,1)$](https://render.githubusercontent.com/render/math?math=%24X%5Csim%5Cmathcal%7BN%7D(0%2C1)%24) with constraints ![$c_1=-1$](https://render.githubusercontent.com/render/math?math=%24c_1%3D-1%24) and ![$c_2=0.5$](https://render.githubusercontent.com/render/math?math=%24c_2%3D0.5%24). Figure (1) show the Normal PDF with the constraints.

![Constrained Standard Normal PDF](https://zachamida.github.io/assets/img/posts/post1/ConstNorm.png)

In the PDF Truncation, the area between the bounds ![$c_1$](https://render.githubusercontent.com/render/math?math=%24c_1%24)
and ![$c_2$](https://render.githubusercontent.com/render/math?math=%24c_1%24) is approximated into a normal PDF. For that end, the area between ![$c_1$](https://render.githubusercontent.com/render/math?math=%24c_1%24)
and ![$c_2$](https://render.githubusercontent.com/render/math?math=%24c_1%24)
 is calculated as in,

![$A=\int_{c_{1}}^{c_{2}} \frac{1}{\sqrt{2 \pi}} \exp \left(-x^{2} / 2\right) \mathrm{d}x =\frac{1}{2}\left\[\operatorname{erf}\left(c_{2} / \sqrt{2}\right)-\operatorname{erf}\left(c_{1} / \sqrt{2}\right)\right\]$](https://render.githubusercontent.com/render/math?math=%24A%3D%5Cint_%7Bc_%7B1%7D%7D%5E%7Bc_%7B2%7D%7D%20%5Cfrac%7B1%7D%7B%5Csqrt%7B2%20%5Cpi%7D%7D%20%5Cexp%20%5Cleft(-x%5E%7B2%7D%20%2F%202%5Cright)%20%5Cmathrm%7Bd%7Dx%20%3D%5Cfrac%7B1%7D%7B2%7D%5Cleft%5B%5Coperatorname%7Berf%7D%5Cleft(c_%7B2%7D%20%2F%20%5Csqrt%7B2%7D%5Cright)-%5Coperatorname%7Berf%7D%5Cleft(c_%7B1%7D%20%2F%20%5Csqrt%7B2%7D%5Cright)%5Cright%5D%24)


For any PDF, it is required that the area under the PDF curve is equal to one. Therefore, the area ![$A$](https://render.githubusercontent.com/render/math?math=%24A%24)
 is normlized with the term ![$\alpha$](https://render.githubusercontent.com/render/math?math=%24%5Calpha%24) which is given by,

![$\alpha=\frac{1}{A}=\frac{2}{\left\[\operatorname{erf}\left(c_{2} / \sqrt{2}\right)-\operatorname{erf}\left(c_{1} / \sqrt{2}\right)\right\]}.$](https://render.githubusercontent.com/render/math?math=%24%5Calpha%3D%5Cfrac%7B1%7D%7BA%7D%3D%5Cfrac%7B2%7D%7B%5Cleft%5B%5Coperatorname%7Berf%7D%5Cleft(c_%7B2%7D%20%2F%20%5Csqrt%7B2%7D%5Cright)-%5Coperatorname%7Berf%7D%5Cleft(c_%7B1%7D%20%2F%20%5Csqrt%7B2%7D%5Cright)%5Cright%5D%7D.%24)


Consequently, the truncated PDF equation is then given by,

![$\alpha=\frac{1}{A}=\frac{2}{\left\[\operatorname{erf}\left(c_{2} / \sqrt{2}\right)-\operatorname{erf}\left(c_{1} / \sqrt{2}\right)\right\]}.$](https://render.githubusercontent.com/render/math?math=%24%5Calpha%3D%5Cfrac%7B1%7D%7BA%7D%3D%5Cfrac%7B2%7D%7B%5Cleft%5B%5Coperatorname%7Berf%7D%5Cleft(c_%7B2%7D%20%2F%20%5Csqrt%7B2%7D%5Cright)-%5Coperatorname%7Berf%7D%5Cleft(c_%7B1%7D%20%2F%20%5Csqrt%7B2%7D%5Cright)%5Cright%5D%7D.%24)


After obtaininig the new (truncated) PDF equation, the expected value of the new PDF is computed as in,

![$\mu =\frac{\alpha}{\sqrt{2 \pi}} \int_{c_{1}}^{c_{2}} x\exp \left(-x^{2} / 2\right) \mathrm{d}x =\frac{\alpha}{\sqrt{2 \pi}}\left\[\exp \left(-c_{1}^{2} / 2\right)-\exp \left(-c_{2}^{2} / 2\right)\right\].$](https://render.githubusercontent.com/render/math?math=%24%5Cmu%20%3D%5Cfrac%7B%5Calpha%7D%7B%5Csqrt%7B2%20%5Cpi%7D%7D%20%5Cint_%7Bc_%7B1%7D%7D%5E%7Bc_%7B2%7D%7D%20x%5Cexp%20%5Cleft(-x%5E%7B2%7D%20%2F%202%5Cright)%20%5Cmathrm%7Bd%7Dx%20%3D%5Cfrac%7B%5Calpha%7D%7B%5Csqrt%7B2%20%5Cpi%7D%7D%5Cleft%5B%5Cexp%20%5Cleft(-c_%7B1%7D%5E%7B2%7D%20%2F%202%5Cright)-%5Cexp%20%5Cleft(-c_%7B2%7D%5E%7B2%7D%20%2F%202%5Cright)%5Cright%5D.%24)


And the new variance is calculated as in,

![$\sigma^{2}= \frac{\alpha}{\sqrt{2 \pi}} \int_{c_{1}}^{c_{2}}\left(x-\mu\right)^{2} \exp \left(-x^{2} / 2\right) \mathrm{d} x = \frac{\alpha}{\sqrt{2 \pi}}\left\[\exp \left(-c_{1}^{2} / 2\right)\left(c_{1}-2 \mu\right)\right.\left.-\exp \left(-c_{2}^{2} / 2\right)\left(c_{2}-2 \mu\right)\right\]+\mu^{2}+1.$](https://render.githubusercontent.com/render/math?math=%24%5Csigma%5E%7B2%7D%3D%20%5Cfrac%7B%5Calpha%7D%7B%5Csqrt%7B2%20%5Cpi%7D%7D%20%5Cint_%7Bc_%7B1%7D%7D%5E%7Bc_%7B2%7D%7D%5Cleft(x-%5Cmu%5Cright)%5E%7B2%7D%20%5Cexp%20%5Cleft(-x%5E%7B2%7D%20%2F%202%5Cright)%20%5Cmathrm%7Bd%7D%20x%20%3D%20%5Cfrac%7B%5Calpha%7D%7B%5Csqrt%7B2%20%5Cpi%7D%7D%5Cleft%5B%5Cexp%20%5Cleft(-c_%7B1%7D%5E%7B2%7D%20%2F%202%5Cright)%5Cleft(c_%7B1%7D-2%20%5Cmu%5Cright)%5Cright.%5Cleft.-%5Cexp%20%5Cleft(-c_%7B2%7D%5E%7B2%7D%20%2F%202%5Cright)%5Cleft(c_%7B2%7D-2%20%5Cmu%5Cright)%5Cright%5D%2B%5Cmu%5E%7B2%7D%2B1.%24)


Accordingly, the truncated PDF will look as in the figure below,

![Constrained Standard Normal PDF](https://zachamida.github.io/assets/img/posts/post1/TruncNorm.png)

## Generalizing The PDF Truncation Method:
Applying the PDF truncation for higher dimentions (i.e. multivariate Normal distribution) requires additional mathematical operations. The PDF truncation is applied to one of the multivariate distribution dimentions at a time. The method is tractable in a sense that the multivariate distribution is transformed into a one dimentional standard Normal case, similar to what was presented earlier. An example application is by considering a mutivariate Normal distribution with two components (![$x:X\sim(\[\mu_1\; \mu_2\]^\intercal,\Sigma)$](https://render.githubusercontent.com/render/math?math=%24x%3AX%5Csim(%5B%5Cmu_1%5C%3B%20%5Cmu_2%5D%5E%5Cintercal%2C%5CSigma)%24)
).
The constraints can be expressed as in,

![$c_{1} \leq D x_t \leq c_{2}$](https://render.githubusercontent.com/render/math?math=%24c_%7B1%7D%20%5Cleq%20D%20x_t%20%5Cleq%20c_%7B2%7D%24)


The constrained state is defined by ![$\tilde{x}$](https://render.githubusercontent.com/render/math?math=%24%5Ctilde%7Bx%7D%24)
. The transformation to one dimentional problem is done through the follwoing equation,

![$z_{t}=S W^{-1 / 2} T^{\intercal}\left(x_{t}-\tilde{x}_{t}\right),$](https://render.githubusercontent.com/render/math?math=%24z_%7Bt%7D%3DS%20W%5E%7B-1%20%2F%202%7D%20T%5E%7B%5Cintercal%7D%5Cleft(x_%7Bt%7D-%5Ctilde%7Bx%7D_%7Bt%7D%5Cright)%2C%24)

where ![$x_{t}$](https://render.githubusercontent.com/render/math?math=%24x_%7Bt%7D%24)
 is the original state and $![$\tilde{x}_{t}$](https://render.githubusercontent.com/render/math?math=%24%5Ctilde%7Bx%7D_%7Bt%7D%24)
 is the constrained state.
The matrix ![$\mathbf{S}$](https://render.githubusercontent.com/render/math?math=%24%5Cmathbf%7BS%7D%24)
 is obtained from the Gram–Schmidt orthogonalisation (Moon and Stirling 2000) as in,

![$S_{t} W_{t}^{1/2} T_{t}^{\intercal} D_{t}=\[(D_{t}^{\intercal}\Sigma_{t}D_{t})^{1/2}\; 0\;\cdots\; 0\]^{\intercal}$](https://render.githubusercontent.com/render/math?math=%24S_%7Bt%7D%20W_%7Bt%7D%5E%7B1%2F2%7D%20T_%7Bt%7D%5E%7B%5Cintercal%7D%20D_%7Bt%7D%3D%5B(D_%7Bt%7D%5E%7B%5Cintercal%7D%5CSigma_%7Bt%7DD_%7Bt%7D)%5E%7B1%2F2%7D%5C%3B%200%5C%3B%5Ccdots%5C%3B%200%5D%5E%7B%5Cintercal%7D%24)


where ![$\mathbf{W}$](https://render.githubusercontent.com/render/math?math=%24%5Cmathbf%7BW%7D%24)
 and ![$\mathbf{T}$](https://render.githubusercontent.com/render/math?math=%24%5Cmathbf%7BT%7D%24)
 are obtained from the Jordan canonical as in,

![$\bf{T}_{t} \bf{W}_{t} \bf{T}_{t}^{\intercal}=\tilde{\Sigma}_{t}$](https://render.githubusercontent.com/render/math?math=%24%5Cbf%7BT%7D_%7Bt%7D%20%5Cbf%7BW%7D_%7Bt%7D%20%5Cbf%7BT%7D_%7Bt%7D%5E%7B%5Cintercal%7D%3D%5Ctilde%7B%5CSigma%7D_%7Bt%7D%24)

The Jordan canonical provides a matrix ![$\bf{W}$](https://render.githubusercontent.com/render/math?math=%24%5Cbf%7BW%7D%24)
 with similar eigen values. For a covariance matrix, the eigenvectors represent the directions of the largest variance of the data, while the eigenvalues represent the magnitude of this variance in those directions.

Following the calculation of the matrices ![$\mathbf{W}$](https://render.githubusercontent.com/render/math?math=%24%5Cmathbf%7BW%7D%24)
, ![$\mathbf{T}$](https://render.githubusercontent.com/render/math?math=%24%5Cmathbf%7BT%7D%24)
 and ![$\mathbf{S}$](https://render.githubusercontent.com/render/math?math=%24%5Cmathbf%7BS%7D%24)
, the transformed upper bound for a standard Normal distribution is derived as in,

![$D^{\intercal} x_t \leq c_{2}$](https://render.githubusercontent.com/render/math?math=%24D%5E%7B%5Cintercal%7D%20x_t%20%5Cleq%20c_%7B2%7D%24)


![$D^{\intercal} T W^{1/2} S^{\intercal} z_{t}+D^{\intercal} \tilde{x}_{t}  \leq c_{2}$](https://render.githubusercontent.com/render/math?math=%24D%5E%7B%5Cintercal%7D%20T%20W%5E%7B1%2F2%7D%20S%5E%7B%5Cintercal%7D%20z_%7Bt%7D%2BD%5E%7B%5Cintercal%7D%20%5Ctilde%7Bx%7D_%7Bt%7D%20%20%5Cleq%20c_%7B2%7D%24)


![$\frac{\left(D^{\intercal} T W^{1 / 2} S^{\intercal}\right) z_{t}}{\left(D^{\intercal}\tilde{\Sigma}_{t} D\right)^{1 / 2}} \leq \frac{c_{2}-D^{\intercal} \tilde{x}_{t}}{\left(D^{\intercal} \tilde{\Sigma}_{t} D\right)^{1 / 2}}$](https://render.githubusercontent.com/render/math?math=%24%5Cfrac%7B%5Cleft(D%5E%7B%5Cintercal%7D%20T%20W%5E%7B1%20%2F%202%7D%20S%5E%7B%5Cintercal%7D%5Cright)%20z_%7Bt%7D%7D%7B%5Cleft(D%5E%7B%5Cintercal%7D%5Ctilde%7B%5CSigma%7D_%7Bt%7D%20D%5Cright)%5E%7B1%20%2F%202%7D%7D%20%5Cleq%20%5Cfrac%7Bc_%7B2%7D-D%5E%7B%5Cintercal%7D%20%5Ctilde%7Bx%7D_%7Bt%7D%7D%7B%5Cleft(D%5E%7B%5Cintercal%7D%20%5Ctilde%7B%5CSigma%7D_%7Bt%7D%20D%5Cright)%5E%7B1%20%2F%202%7D%7D%24)


![$\[1\; 0 \cdots 0\] z_{t}  \leq \tilde{c}_{2}(k)$](https://render.githubusercontent.com/render/math?math=%24%5B1%5C%3B%200%20%5Ccdots%200%5D%20z_%7Bt%7D%20%20%5Cleq%20%5Ctilde%7Bc%7D_%7B2%7D(k)%24)



Similarly, the lower bound is given by,

![$\left\[\begin{array}{cccc}{1} & {0} & {\cdots} & {0}\end{array}\right\] z_{t} \geq \frac{c_{1}-D^{\intercal} \tilde{x}_{t}}{\left(D^{\intercal} \tilde{\Sigma}_{t} D\right)^{1 / 2}}$](https://render.githubusercontent.com/render/math?math=%24%5Cleft%5B%5Cbegin%7Barray%7D%7Bcccc%7D%7B1%7D%20%26%20%7B0%7D%20%26%20%7B%5Ccdots%7D%20%26%20%7B0%7D%5Cend%7Barray%7D%5Cright%5D%20z_%7Bt%7D%20%5Cgeq%20%5Cfrac%7Bc_%7B1%7D-D%5E%7B%5Cintercal%7D%20%5Ctilde%7Bx%7D_%7Bt%7D%7D%7B%5Cleft(D%5E%7B%5Cintercal%7D%20%5Ctilde%7B%5CSigma%7D_%7Bt%7D%20D%5Cright)%5E%7B1%20%2F%202%7D%7D%24)


![$\[1\; 0 \cdots 0\] z_{t}  \ge \tilde{c}_{1}(k)$](https://render.githubusercontent.com/render/math?math=%24%5B1%5C%3B%200%20%5Ccdots%200%5D%20z_%7Bt%7D%20%20%5Cge%20%5Ctilde%7Bc%7D_%7B1%7D(k)%24)


The transformed upper and lower bounds are applied on a standard Normal PDF (![$X\sim\mathcal{N}(0,1)$](https://render.githubusercontent.com/render/math?math=%24X%5Csim%5Cmathcal%7BN%7D(0%2C1)%24)
), in a similar manner to the first example.

The resulting mean ![$\mu$](https://render.githubusercontent.com/render/math?math=%24%5Cmu%24)
 and variance ![$\sigma^2$](https://render.githubusercontent.com/render/math?math=%24%5Csigma%5E2%24)
 are then back-transformed through the following equations,

![$\begin{array}{l} {\tilde{x}_{t}=T W^{1 / 2} S^{\intercal} \tilde{\mu}+{x}_{t}} \\ {\tilde{\Sigma}_{t}=T W^{1 / 2} S^{\intercal} \tilde{\sigma}_{t}^2 S W^{1 / 2} T^{\intercal}} \end{array}$](https://render.githubusercontent.com/render/math?math=%24%5Cbegin%7Barray%7D%7Bl%7D%20%7B%5Ctilde%7Bx%7D_%7Bt%7D%3DT%20W%5E%7B1%20%2F%202%7D%20S%5E%7B%5Cintercal%7D%20%5Ctilde%7B%5Cmu%7D%2B%7Bx%7D_%7Bt%7D%7D%20%5C%5C%20%7B%5Ctilde%7B%5CSigma%7D_%7Bt%7D%3DT%20W%5E%7B1%20%2F%202%7D%20S%5E%7B%5Cintercal%7D%20%5Ctilde%7B%5Csigma%7D_%7Bt%7D%5E2%20S%20W%5E%7B1%20%2F%202%7D%20T%5E%7B%5Cintercal%7D%7D%20%5Cend%7Barray%7D%24)


## Limitations:
1- The expected value of the constrained state is shifted towards the feasible region. This may cause a biase in the constrained estimate.

## Matlab Code:
[1] https://academic.csuohio.edu/simond/kalmanconstrained/

[2] https://github.com/zachamida/ConstrainedKalmanFilter

### Resources:

[1] Dan Simon and Donald L Simon. Constrained kalman filtering via density function truncation for turbofan engine health estimation. International Journal of Systems Science, 41(2):159–171.

[2] Khan Academy: https://www.khanacademy.org/math/linear-algebra/alternate-bases/orthonormal-basis/v/linear-algebra-the-gram-schmidt-process


