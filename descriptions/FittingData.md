---
title: Fitting Data
layout: default
nav_order: 2
parent: Description
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

# Fitting Data
{: .no_toc}

This page outlines how to fit observational data and simulated galactic atmospheres with **ExpCGM** models.

{: .warning}
This page is still under construction.

<details closed markdown="block">
  <summary>
    Contents
  </summary>
   {: .text-delta}
- TOC
{:toc}  
</details>

## Input Parameters

All **ExpCGM** models for galactic atmospheres are based on a user-specified gravitational potential function $\varphi(\mathbf{r})$ and a user-specified shape function $\alpha(\mathbf{r})$ for the atmosphere's pressure profile. Both functions can depend on a three-dimensional position vector $\mathbf{r}$. However, spherically symmetric models depending only on $r = \| \mathbf{r} \|$ are often sufficient. This introductory page focuses on just spherically symmetric models.

Two parametric models represent the mininum input for generating an **ExpCGM** model atmosphere: 

* **Gravitational Potential,** $\varphi( ~r~ \| ~v_\varphi , r_{\rm s} , ... )$: The gravitational potential model requires only one parameters, the maximum circular velocity $v_\varphi$ of the confining halo's gravitational potential. It may also include a scale radius $r_{\rm s}$ determining where $v_{\rm c} = v_\varphi$ along with other parameters describing a more detailed potential well.

* **Shape Function,** $\alpha( ~r~ \| ~\alpha , \alpha_{\rm in} , \alpha_{\rm out} , r_\alpha , ... )$: The shape function can be a constant value of $\alpha$. It can also be a function that goes from one limiting value ($\alpha_{\rm in}$) at small radii to another one ($\alpha_{\rm out}$) near a crossover radius ($r_\alpha$). (See the [Pressure Profiles](PressureProfiles) page for some other physically motivated options.) 

Two additional parametric functions may be added to represent how the thermalization fraction $f_{\rm th}$ and force modification factor $f_\varphi$ depend on radius. Their default values are $f_{\rm th} = f_\varphi = 1$. (See the [Essentials](Essentials) page for definitions.)

That set of four parametric functions determines the atmosphere's temperature profile
    $$T(r) = \left( {f_{\rm th} f_\varphi} \right) \frac {\mu m_p v_{\rm c}^2} {\alpha_{\rm eff}}$$  
in which the generalized shape function $\alpha_{\rm eff}(r)$ reduces to $\alpha(r)$ if the thermalization factor $f_{\rm th}$ is independent of radius.

The final input parameter for a complete **ExpCGM** model is the normalization $P_0 = P(r_0)$ of the atmosphere's pressure profile at a chosen fiducial radius $r_0$.

### Isothermal Atmosphere

Users seeking to minimize degrees of freedom can simply opt to keep $v_{\rm c}$ and $\alpha$ constant and set $f_{\rm th}$ and $f_\varphi$ to unity. The atmosphere's temperature $T = \mu m_p v_\varphi^2 / \alpha$ is then constant with radius, its pressure profile is the power law $P(r) = P_0 (r / r_0)^{-\alpha}$, and its density profile is 
  $$\rho(r) = \frac {\alpha P_0} {v_\varphi^2} \left( \frac {r} {r_0} \right)^{-\alpha}$$
This isothermal atmosphere model has three degrees of freedom $(P_0,v_\varphi,\alpha)$, because $r_0$ is degenerate with $P_0$.

### Double Power-Law Atmosphere

### NFW-like Models

### NFW Halo + Central Galaxy

### NFW Halo + Central Galaxy + SMBH


## Model Output

### Global Properties

### Thermodynamic Profiles

### Projected Profiles


## Scaling Laws


