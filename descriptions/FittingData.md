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

This page outlines how to fit both observed and simulated galactic atmospheres with **ExpCGM** models.

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

* **Gravitational Potential,** $\varphi( ~r~ \| ~v_\varphi , r_{\rm s} , ... ~)$: The gravitational potential model requires only one parameter, the maximum circular velocity $v_\varphi$ of the confining halo's gravitational potential. It may also include a scale radius $r_{\rm s}$ determining how the potential's circular velocity $v_{\rm c}$ depends on $r$. Descriptions of more detailed potential wells require additional parameters.

* **Shape Function,** $\alpha( ~r~ \| ~\alpha , \alpha_{\rm in} , \alpha_{\rm out} , r_\alpha , ... ~)$: The atmosphere's shape function can be a constant value of $\alpha$. It can also be a function that goes from one limiting value ($\alpha_{\rm in}$) at small radii to another one ($\alpha_{\rm out}$) near a crossover radius ($r_\alpha$). The [Pressure Profiles](PressureProfiles) page provides parametric expressions for some physically motivated options.

Two additional parametric functions may be added to represent how the thermalization fraction $f_{\rm th}$ and force modification factor $f_\varphi$ depend on radius. Their default values are $f_{\rm th} = f_\varphi = 1$. (See the [Essentials](Essentials) page for definitions.)

That set of four parametric functions determines the atmosphere's temperature profile
    $$T(r) = \left( {f_{\rm th} f_\varphi} \right) \frac {\mu m_p v_{\rm c}^2 (r)} {\alpha_{\rm eff} (r) }$$  
in which the generalized shape function $\alpha_{\rm eff}(r)$ reduces to $\alpha(r)$ if the thermalization factor $f_{\rm th}$ is independent of radius.

The final input parameter needed for a complete **ExpCGM** model is the normalization $P_0 = P(r_0)$ of the atmosphere's pressure profile at a user-chosen fiducial radius $r_0$. 

### Isothermal Atmosphere

Users seeking to minimize degrees of freedom can opt for an isothermal potential well with constant circular velocity $v_{\rm c} = v_\varphi$ and an atmosphere with constant $\alpha$ while setting $f_{\rm th}$ and $f_\varphi$ to unity. The atmosphere's temperature $T = \mu m_p v_\varphi^2 / \alpha$ is then constant with radius, its pressure profile is the power law $P(r) = P_0 (r / r_0)^{-\alpha}$, and its density profile is 
  $$\rho(r) = \frac {\alpha P_0} {v_\varphi^2} \left( \frac {r} {r_0} \right)^{-\alpha}$$
This isothermal atmosphere model therefore has three degrees of freedom $(P_0,v_\varphi,\alpha)$, because $r_0$ is degenerate with $P_0$. Its temperature for a given value of $v_\varphi$ depends inversely on $\alpha$.

### Double Power-Law Atmospheres

Cosmological structure formation produces gaseous atmospheres in which $\alpha(r)$ increases with radius. An **ExpCGM** model can account for this increase using the four-parameter fitting formula
  $$\alpha(r) = \alpha_{\rm in} + ( \alpha_{\rm out} - \alpha_{\rm in} ) \left[ \frac {(r / r_\alpha)^{\alpha_{\rm tr}}} {1 + (r / r_\alpha)^{\alpha_{\rm tr}}} \right]$$
It results in a model with six degrees of freedom $(P_0,v_\varphi,\alpha_{\rm in},\alpha_{\rm out},\alpha_{\rm tr},r_\alpha)$ for an atmosphere in an isothermal potential well, with a pressure profile 
  $$P(r) \propto \left( \frac {r} {r_\alpha} \right)^{-\alpha_{\rm in}} \left[ 1 + \left( \frac {r} {r_\alpha} \right)^{\alpha_{\rm tr}} \right]^{- \frac {\alpha_{\rm out} - \alpha_{\rm in}} {\alpha_{\rm tr}}}$$
Gas temperature in this atmosphere model declines from $T \approx \mu m_p v_\varphi^2 / \alpha_{\rm in}$ at small radii toward $T \approx \mu m_p v_\varphi^2 / \alpha_{\rm out}$ at large radii as long as $\alpha_{\rm out} > \alpha_{\rm in}$.

{: .note}
Restricting the double power law model by setting $\alpha_{\rm in} = 0$ and $\alpha_{\rm tr} = 2$ results in 
  $$P(r) \propto \left[ 1 + \left( \frac {r} {r_\alpha} \right)^2 \right]^{-\frac {\alpha_{\rm out}} {2}}$$ 
This relation is equivalent to the classic *beta model* for galaxy-cluster atmospheres, but with thermal pressure replacing gas density. This model has a drawback: The fundamental force-balance assumption of **ExpCGM** results in a gas-temperature profile that diverges at small $r$ in an isothermal potential well with constant $v_\varphi$. This undesirable feature of a beta model can be mitigated by using a gravitational potential in which $v_{\rm c}$ becomes small at small radii.

### NFW-like Models

Cosmological structure formation also produces halo potential wells that are not quite isothermal. Typically, the circular velocity profile of a cosmological halo rises with radius at small $r$ and declines with radius at large $r$. The most common fitting formula accounting for that feature is the Navarro-Frenk-White (NFW) model, which can be represented as
  $$v_{\rm NFW}^2(x) = A_{\rm NFW} v_\varphi^2 \left[ \frac {\ln (1 + x)} {x} - \frac {1} {1 + x} \right]$$
with $x =  r / r_{\rm s}$ and $A_{\rm NFW} = 4.625$. The simple example on the [Essentials](Essentials) page presents a model with four degrees freedom $(P_0,v_\varphi,r_{\rm s}, \alpha)$ for a power-law atmosphere in an NFW potential well. 

Expanding that example to accommodate a double power-law atmosphere yields a model with seven degrees of freedom $(P_0,v_\varphi,r_{\rm s},\alpha_{\rm in},\alpha_{\rm out},\alpha_{\rm tr},r_\alpha)$. It can be reduced to a six-parameter model by choosing to make $r_\alpha$ a constant multiple of $r_{\rm s}$ and to a five-parameter model by keeping $\alpha_{\rm tr}$ fixed.
 
### NFW Halo + Central Galaxy

An NFW halo model coupled with a shape function model that has $\alpha_{\rm in} > 0$ results in an atmospheric temperature that approaches zero at small radii. Adding a central galaxy to the gravitational potential model helps to address this potentially problematic issue.

One option in the **ExpCGM** framework is to use a ***Hernquist model*** to represent the galaxy's contribution to the potential's circular velocity profile:
  $$v_{\rm H}^2(r) =  $$

### NFW Halo + Central Galaxy + SMBH

### Thermalization Factor


## Model Output

### Global Properties

### Thermodynamic Profiles

### Projected Profiles


## Scaling Laws


