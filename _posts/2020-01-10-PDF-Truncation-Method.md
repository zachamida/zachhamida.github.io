---
title: "State-Space Models Constraints Using PDF Truncation"
date: 2020-01-10 00:34:00 +0800
categories: [Blogging, Tutorial]
tags: [State-Space Models, PDF Truncation]
toc: false
---
## Introduction
State-space models are well suited for time-series data and allow estimating the hidden states of the system from imperfect observations.
In some applications, the hidden state is known to be within a specific domain. Therefore, it can be useful to apply state constraints on the state vector components.\
An example application, is given the system state at time $t$, where 

$$0\le x_t \le 10$$

. State constraints can be imposed through the PDF Truncation Method proposed by Simon et. al [ref].\
to be continued.
[ref] Dan Simon and Donald L Simon. Constrained kalman filtering via den- sity function truncation for turbofan engine health estimation. Interna- tional Journal of Systems Science, 41(2):159–171.

<script id="MathJax-script" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>