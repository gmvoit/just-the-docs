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

* **Gravitational Potential,** $\varphi( ~r~ \| ~v_\varphi , r_{\rm s} , ... ~)$: The gravitational potential model requires only one parameter, the maximum circular velocity $v_\varphi$ of the confining halo's gravitational potential. It may also include a scale radius $r_{\rm s}$ determining how the potential's circular velocity $v_{\rm c}$ depends on $r$. Descriptions of more detailed potential wells may require additional parameters.

* **Shape Function,** $\alpha( ~r~ \| ~\alpha , \alpha_{\rm in} , \alpha_{\rm out} , r_\alpha , ... ~)$: The atmosphere's shape function can be just a constant value of $\alpha$. It can also shift from one limiting value ($\alpha_{\rm in}$) at small radii to another one ($\alpha_{\rm out}$) at large radii in the vicinity of a crossover radius ($r_\alpha$). The [Pressure Profiles](PressureProfiles) page discusses how $\alpha (r)$ is related to the processes that shape a galaxy's atmosphere and provides parametric expressions for several physically motivated options.

Once $\alpha(r)$ has been specified, the pressure profile of an **ExpCGM** atmosphere model depends on just the normalization factor $P_0$ at a user-chosen fiducial radius $r_0$. The atmosphere's pressure profile becomes
  $$P(r) = P_0 \cdot \exp \left[ - \int_1^{r/r_0} \frac {\alpha(x)} {x} dx \right]$$
Dividing $v_{\rm c}^2 (r)$ by $\alpha(r)$ gives the temperature profile for a thermally supported atmosphere: 
  $$kT(r) = \frac {\mu m_p v_{\rm c}^2(r)} {\alpha(r)}$$
Its density profile then depends on just $\varphi(r)$, $\alpha(r)$, and $P_0$:
  $$\rho(r) = \frac {\alpha(r) P(r)} {v_{\rm c}^2(r)}$$
However, $T(r)$ and $\rho(r)$ may also depend on two additional parametric functions representing the atmosphere's thermalization fraction $f_{\rm th}$ and force modification factor $f_\varphi$ (see the [Essentials](Essentials) page for definitions of those functions).

### Isothermal Atmosphere

Users seeking to minimize degrees of freedom can opt for an isothermal potential well with constant circular velocity ($v_{\rm c} = v_\varphi$) confining an atmosphere with constant $\alpha$. Setting $f_{\rm th}$ and $f_\varphi$ to unity then gives an atmospheric temperature $kT = \mu m_p v_\varphi^2 / \alpha$ that is independent of radius and power-law profiles for pressure and density: 
  $$P(r) = P_0 \left( \frac {r} {r_0} \right)^{-\alpha} ~~~,~~~ \rho(r) = \frac {\alpha P_0} {v_\varphi^2} \left( \frac {r} {r_0} \right)^{-\alpha}$$
This isothermal atmosphere model has three degrees of freedom $(P_0,v_\varphi,\alpha)$, because $r_0$ is degenerate with $P_0$. 

### Double Power-Law Atmospheres

Cosmological structure formation produces gaseous atmospheres in which $\alpha(r)$ increases with radius. An **ExpCGM** model can describe that increase via the four-parameter fitting formula
  $$\alpha(r) = \alpha_{\rm in} + ( \alpha_{\rm out} - \alpha_{\rm in} ) \left[ \frac {(r / r_\alpha)^{\alpha_{\rm tr}}} {1 + (r / r_\alpha)^{\alpha_{\rm tr}}} \right]$$
If all four parameters are left free, the resulting model for an atmosphere in an isothermal potential well has six degrees of freedom instead of three. Its pressure profile is 
  $$P(r) \propto \left( \frac {r} {r_\alpha} \right)^{-\alpha_{\rm in}} \left[ 1 + \left( \frac {r} {r_\alpha} \right)^{\alpha_{\rm tr}} \right]^{- \frac {\alpha_{\rm out} - \alpha_{\rm in}} {\alpha_{\rm tr}}}$$
and its gas temperature declines from $kT \approx \mu m_p v_\varphi^2 / \alpha_{\rm in}$ at small radii toward $kT \approx \mu m_p v_\varphi^2 / \alpha_{\rm out}$ at large radii (assuming $\alpha_{\rm out} > \alpha_{\rm in}$).

{: .note}
Restricting the double power-law atmosphere model by setting $\alpha_{\rm in} = 0$ and $\alpha_{\rm tr} = 2$ results in 
  $$P(r) \propto \left[ 1 + \left( \frac {r} {r_\alpha} \right)^2 \right]^{-\frac {\alpha_{\rm out}} {2}}$$ 
This relation is nearly equivalent to the classic "beta model" for galaxy-cluster atmospheres, but with thermal pressure replacing gas density. However, the model has a drawback: It results in a gas-temperature profile that diverges at small $r$ in an isothermal potential well with constant $v_\varphi$. This undesirable feature is less problematic in gravitational potentials with small values of $v_{\rm c}$ at small radii.

### NFW-like Models

Cosmological structure formation also produces halo potential wells that are not quite isothermal. Typically, the circular velocity profile of a cosmological halo rises with radius at small $r$ and declines with radius at large $r$. The most common fitting formula accounting for that feature is the Navarro-Frenk-White (NFW) model, which can be represented by the circular velocity profile
  $$v_{\rm NFW}^2(x) = A_{\rm NFW} v_\varphi^2 \left[ \frac {\ln (1 + x)} {x} - \frac {1} {1 + x} \right]$$
with $x =  r / r_{\rm s}$ and $A_{\rm NFW} = 4.625$. 

The [Essentials](Essentials) page presents a simple example with four degrees of freedom $(P_0,v_\varphi,r_{\rm s}, \alpha)$ describing a power-law atmosphere in an NFW potential well. Expanding that example using all four parameters of a double power-law atmosphere yields a model with seven degrees of freedom $(P_0,v_\varphi,r_{\rm s},\alpha_{\rm in},\alpha_{\rm out},\alpha_{\rm tr},r_\alpha)$. Choosing to make $r_\alpha$ a constant multiple of $r_{\rm s}$ and keeping $\alpha_{\rm tr}$ fixed are options for reducing the dimensionality of the parameter space.
 
### NFW Halo + Central Galaxy

An NFW halo model coupled with a shape function model that has $\alpha_{\rm in} > 0$ results in an atmospheric temperature that formally approaches zero at small radii. Adding a central galaxy to the gravitational potential model helps to mitigate that potentially problematic issue.

One option is to use a ***Hernquist model*** to represent the galaxy's contribution to the potential's circular velocity profile:
  $$v_{\rm H}^2(r) =  \frac {G M_{\rm H} r} {r + r_{\rm H}}$$
In this expression, $M_{\rm H}$ represents the galaxy's total mass and the ***Hernquist radius*** $r_{\rm H}$ is a scale radius determining how the galaxy's mass profile converges toward $M_{\rm H}$. 

The model parameters $M_{\rm H}$ and $r_{\rm H}$ can be free, or they can be fixed at values consistent with the observed stellar mass and effective radius of the halo's central galaxy. If both $M_{\rm H}$ and $r_{\rm H}$ are allowed to be free, then adding a central galaxy to the gravitational potential model increases dimensionality of the overall **ExpCGM** atmosphere model by two degrees of freedom.

{: .note}
Most central galaxies have a maximum circular velocity $(G M_{\rm H} / 4 r_{\rm H})^{1/2}$ similar to the maximum circular velocity $v_\varphi$ of the surrounding halo. It is therefore reasonable to reduce the dimensionality of the parameter space by applying the restriction $r_{\rm H} = G M_{\rm H} / 4 v_\varphi^2$, so that $\max (v_{\rm H}) = v_\varphi$. However, applying that restriction is unwise for galaxy-cluster models, because the maximum circular velocity of a central cluster galaxy is significantly smaller than the maximum circular velocity of its halo. 

### NFW Halo + Central Galaxy + BH

A supermassive black hole may dominate the galaxy's gravitational potential at the smallest radii. Its contribution can be included in an **ExpCGM** model using the Newtonian formula
  $$v_{\rm BH}^2 (r) = \frac {G M_{\rm BH}} {r}$$
where $M_{\rm BH}$ is the central black hole's mass. Adding a black hole to the potential model, so that
  $$v_{\rm c}^2 (r) = v_{\rm NFW}^2 (r) + v_{\rm H}^2 (r) + v_{\rm BH}^2 (r)$$
then ensures that the model atmosphere's temperature does not formally go to zero at the center.

### Thermalization Fraction

Observational constraints can be placed on an atmosphere's thermalization fraction $f_{\rm th}$ if the overall data set contains information complementary to the atmospheric temperature profile. 

An **ExpCGM** user interested in thermalization may choose to add a parametric model for $f_{\rm th}$ that depends on radius. Combining that model with $\alpha (r)$ gives
  $$\alpha_{\rm eff} (r) = \alpha (r) + \frac {d \ln f_{\rm th}} {d \ln r}$$
The predicted temperature profile then becomes
  $$kT(r) = \frac {\mu m_p v_{\rm c}^2 (r)} {\alpha_{\rm eff} (r)} f_{\rm th} (r)$$
Furthermore, assuming that isotropic turbulence provides the rest of the support needed for force balance gives the prediction
  $$\sigma_{\rm 1D}^2 (r) = \frac {2} {3} \frac {v_{\rm c}^2 (r)} {\alpha_{\rm eff} (r)} \left[ 1 - f_{\rm th} (r) \right]$$
in which $\sigma_{\rm 1D}$ is the one-dimensional velocity dispersion of turbulent support.

Fitting such an **ExpCGM** atmosphere model to a data set containing information about both $T(r)$ and $v_{\rm c}^2 (r)$ or $T(r)$ and $\sigma_{\rm 1D}$ then jointly constrains both $v_{\rm c} (r)$ and $f_{\rm th}(r)$.

## Model Output

**ExpCGM** models for galactic atmospheres predict a variety of potentially observable atmospheric characteristics that can be used to test the models and to constrain the input parameters.

### Radial Profiles

An input parameter set determines the radial profiles of thermal pressure, temperature, and gas density as described in the previous section. Those predicted profiles can be directly compared with deprojected versions of $P(r)$, $T(r)$, and $\rho (r)$ derived from observational data. Fitting an **ExpCGM** model to deprojected profiles constrains the input parameters. However, reliable deprojections generally require high-quality data and approximate spherical symmetry.

### Projected Profiles

Forward modeling of projected observables enables more robust fits to observational data ...

#### Surface Mass Density

A spherical atmosphere's surface mass density along a line of sight at a projected radius $r_\perp$ is 

<p>
  $$\Sigma_{\rm CGM} (r_\perp) = \int_{-\infty}^{\infty} \rho(r) ~dr_\parallel$$ 
</p>

where $r_\parallel$ is the component of $\mathbf{r}$ parallel to the line of sight. Bringing the dimensional factors outside of the integral gives

<p>
  $$\Sigma_{\rm CGM} (r_\perp) = r_\perp \rho(r_\perp) \int_{-\infty}^{\infty} \frac {\rho(r)} {\rho(r_\perp)} d \left( \frac {r_\parallel} {r_\perp} \right)$$
</p>

The structure-factor integral to be performed is then a number of order unity usually calculated by numerical integration. However, the result for an isothermal power-law atmosphere can be expressed in terms of gamma functions:

<p>
  $$\Sigma_{\rm CGM} (r_\perp) = \left[ \frac {\pi^{1/2} \Gamma ()} {\gamma ()} \right] r_\perp \rho(r_\perp) $$
</p>

### Global Properties

Data sets with many objects ...

## Scaling Laws

### Dependence on Halo Mass

### Dependence on Redshift



