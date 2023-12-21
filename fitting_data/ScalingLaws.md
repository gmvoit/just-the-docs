---
title: Scaling Laws
layout: default
nav_order: 3
parent: Fitting Data
---

<head>
  <title>MathJax tests</title>

  <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>

  <script>
    MathJax = {
     tex: {
      inlineMath: [['$', '$']],
      displayMath: [ ['$$','$$'], ["\\(","\\)"] ],
      processEscapes: true
      }
     };
  </script>

 <script id="MathJax-script" async
     src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js">
  </script>
</head>


# Scaling Laws
{: .no_toc}

One of the **ExpCGM** framework's primary purposes is to help constrain galactic feedback models using multiple data sets that provide complementary information about how atmospheric properties scale with halo mass and redshift.

The page describes the parametric models used to fit those scaling laws in the **ExpCGM** framework.


## Mass Dependence

The simplest parametric fitting formulae for halo-mass scaling of atmospheric properties assume power-law dependences. However, halo mass is not one of the **ExpCGM** input parameters. Instead, mass scaling of model parameters in the **ExpCGM** framework is represented with power-law dependences on $v_\varphi$ rather than on a value of $M_{\rm tot}$ defined with respect to a particular density contrast $\Delta_{\rm c}$.  

Making that choice has three benefits:
 1. It eliminates the "pseudoevolution" of scaling relations that occurs simply because $\rho_{\rm cr} (z)$ changes with time.
 2. The maximum circular velocity of a cosmological halo usually changes slowly with time, meaning that fits of redshift evolution at fixed $v_\varphi$ is closer to representing time-dependent changes in a particular population of halos than is redshift evolution at fixed $M_{\rm halo}$.
 3. The importance of cooling and feedback processes in an **ExpCGM** model depends on how much they change the ratio of atmospheric specific energy to $v_\varphi^2$ as described on the [Essentials](Essentials) page.

The symbol $\beta$ represents the power-law dependence of a parameter on $v_\varphi$ in the **ExpCGM** framework. For example, the pressure normalization is assumed to scale as
  $$P_0 \propto v_\varphi^{\beta_P}$$

### Self-Similar Halos

A self-similar population of cosmological atmospheres has $\beta_P$

### NFW Halos

Other parameters that may scale with $v_\varphi$ include $\alpha$ and $f_{\rm th}$.

## Redshift Dependence

Model parameters in **ExpCGM** are also assumed to have power-law dependences on $1+z$ ... For example, 
  $$P_0(v_\varphi,z) \propto v_\varphi^{\beta_P} (1+z)^{\zeta_P}$$
  $$f_{\rm th} (v_\varphi,z) \propto v_\varphi^{\beta_{\rm th}} (1+z)^{\zeta_{\rm th}}$$

## Intrinsic Dispersion

Log-normal scatter in $P_0$ at fixed $v_\varphi$ is represented with $\sigma_{\ln P \| \varphi}$. It is constrained by observations...

There may also be scatter in $\alpha$ ...

## Summary of Scaling Parameters

| Parameter | Description |
| :-------: | ----------- |
|  $\beta_P$  |  Dependence of $P_0$ on potential well depth  |
|  $\zeta_P$   |  Dependence of $P_0$ on redshift  |
|  $\beta_{\rm th}$  |  Dependence of thermalization on potential well depth  |
|  $\zeta_{\rm th}$   |  Dependence of thermalization on redshift  |
|  $\sigma_{\ln P \| \varphi}$ | Log-normal scatter in $P_0$ at fixed $v_\varphi$ | 
|  $\sigma_{r_{\rm s} \| \varphi}$ | Scatter in $r_0$ at fixed $v_\varphi$ | 
