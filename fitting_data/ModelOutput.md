
---
title: Model Output
layout: default
nav_order: 2
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

# Model Output
{: .no_toc}

A variety of potentially observable atmospheric characteristics can be used to test **ExpCGM** models for galactic atmospheres, to derive contraints on model parameters, and to compare observations with simulations within the input parameter space.

This page describes the observational predictions provided with the output of an **ExpCGM** model.

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

## Radial Profiles

An **ExpCGM** model's radial profiles of thermal pressure, temperature, and gas density depend on the input parameter set as described in the previous section. Those profiles can be directly compared with deprojected versions of $P(r)$, $T(r)$, and $\rho (r)$ derived from observational data. Fits of an **ExpCGM** model to deprojected observations therefore constrain the model's input parameters. However, reliable deprojections require high-quality data and approximate spherical symmetry.

## Projected Profiles

Projected **ExpCGM** models provide many observable predictions that can be compared more directly with observational data and combined to obtain joint constraints on a model's input parameters.

### Surface Mass Density

A spherical atmosphere's surface mass density along a line of sight at a projected radius $r_\perp$ is 
  $$\Sigma_{\rm CGM} (r_\perp) = \int_{-\infty}^{\infty} \rho(r) ~dr_\parallel$$ 
where $r_\parallel = \pm ( r^2 - r_\perp^2 )^{1/2}$ is the component of $\mathbf{r}$ parallel to the line of sight. Bringing dimensional factors outside of the integral gives
  $$\Sigma_{\rm CGM} (r_\perp) = r_\perp \rho(r_\perp) \int_{-\infty}^{\infty} \frac {\rho(r)} {\rho(r_\perp)} ~d \left( \frac {r_\parallel} {r_\perp} \right)$$
The integral to be performed is then a structure factor of order unity found through numerical integration. 

For an isothermal power-law atmosphere, the integral can be expressed in terms of gamma functions, giving
  $$\Sigma_{\rm CGM} (r_\perp) = \left[ \frac {\pi^{1/2} \Gamma \left( \frac {\alpha - 1} {2} \right)} {\Gamma \left( \frac {\alpha} {2} \right)} \right] r_\perp \rho(r_\perp) $$
For example, the result for $\alpha = 2$ and constant $v_{\rm c}$ is
  $$\Sigma_{\rm CGM} (r_\perp) ~=~ \pi r_\perp \rho (r_\perp) ~=~ \frac {2 \pi r_0 P_0} {v_\varphi^2} \left( \frac {r_\perp} {r_0} \right)^{-1}$$

{: .note}
The approximation 
  $$\Sigma_{\rm CGM} \approx \frac {2 \pi r_0 P_0} {v_\varphi^2} \left( \frac {r_\perp} {r_0} \right)^{1-\alpha}$$ 
for isothermal power-law atmospheres is good to within 10% for $\alpha$ in the range $1.7 \leq \alpha \leq 5.5$.


### Hydrogen Column Density

Dividing the atmosphere's surface mass density by its mean mass per hydrogen nucleus $\mu_{\rm H} m_p$ gives the total hydrogen column density along a line of sight at $r_\perp$:
  $$N_{\rm H} (r_\perp) = \frac {\Sigma_{\rm CGM} (r_\perp)} {\mu_{\rm H} m_p}$$
A primordial atmosphere has $\mu_{\rm H} = 1.33$. An atmosphere with solar abundance ratios has $\mu_{\rm H} = 1.42$.

{: .note}
Some of the observational constraints on $N_{\rm H}$ and its dependence on $r_\perp$ around galaxies like the Milky Way come from circumgalactic clouds that absorb UV light from background quasars. Photoionization modeling is necessary for determining $N_{\rm H}$ and its uncertainty range from those observations. Also, the fraction of gas without a UV absorption signature is often unknown, meaning that column density contraints derived from UV observations should be treated as lower limits on $N_{\rm H} (r_\perp)$.


### Electron Column Density

Dividing the atmosphere's surface mass density by its mean mass per electron $\mu_e m_p$ gives the electron column density along a line of sight at $r_\perp$:
  $$N_e (r_\perp) = \frac {\Sigma_{\rm CGM} (r_\perp)} {\mu_e m_p}$$
A fully ionized primordial atmosphere has $\mu_e = 1.14$. A fully ionized atmosphere with solar abundances has $\mu_e = 1.18$.

Dispersion-measure observations of fast radio bursts that pass through galactic atmospheres place constraints on $N_e (r_\perp)$.


### Compton Parameter

Compton scattering distorts the microwave background spectrum along lines of sight through hot galactic atmospheres. This distortion is known as the *Thermal Sunyaev-Zeldovich Effect* or *tSZ* for short. It depends on frequency and is proportional to the ***Compton parameter***
  $$y = \int \left( \frac {kT} {m_e c^2} \right) n_e \sigma_{\rm T} ~d r_\parallel$$
in which $\sigma_{\rm T}$ is the Thomson cross section for electron scattering.

A spherical **ExpCGM** atmosphere model gives the predicted tSZ distortion profile
  $$y (r_\perp) = \left( \frac {\mu \sigma_{\rm T}} {\mu_e m_e c^2} \right) r_\perp P (r_\perp) \int_{-\infty}^{\infty} \frac {f_P (r)} {f_P (r_\perp)} ~d \left( \frac {r_\parallel} {r_\perp} \right)$$ 
As with the surface density, the integral is a structure factor of order unity to be numerically integrated. 

For an isothermal power-law atmosphere, the integral results in
  $$y (r_\perp) = \left( \frac {\mu \sigma_{\rm T}} {\mu_e m_e c^2} \right) \left[ \frac {\pi^{1/2} \Gamma \left( \frac {\alpha - 1} {2} \right)} {\Gamma \left( \frac {\alpha} {2} \right)} \right] r_0 P_0 \left( \frac {r_\perp} {r_0} \right)^{1-\alpha}$$
When $\alpha = 2$ it further reduces to
  $$y (r_\perp) = \left( \frac {\mu \sigma_{\rm T}} {\mu_e m_e c^2} \right) \pi r_0 P_0 \left( \frac {r_\perp} {r_0} \right)^{-1}$$
 

{: .note}
Inserting physical quantities characteristic of galaxy groups gives a reference value
  $$y \approx 5 \times 10^{-7} \left( \frac {r_\perp} {100~ {\rm kpc}} \right) \left( \frac {n} {10^{-3}~ {\rm cm}^{-3}} \right) \left( \frac {T} {10^7~ {\rm K}} \right)$$
for atmospheres with $\alpha \approx 2$.


### Emission Measure

Collisional excitation of emission lines produces a signal proportional to the integral of $\rho^2$ along a line of sight through a galactic atmosphere. The literature on nebular emission excited by electron collisions defines the ***emission measure*** along a line of sight to be
  $${\rm EM} \equiv \int n_e n_{\rm H} ~d r_\parallel$$
The emission measure profile of an **ExpCGM** galactic atmosphere model is therefore
  $${\rm EM} (r_\perp) = \frac {r_\perp \rho^2 (r_\perp)} {\mu_e \mu_{\rm H} m_p^2} \int_{-\infty}^{\infty} \frac {\rho^2 (r)} {\rho^2 (r_\perp)} ~d \left( \frac {r_\parallel} {r_\perp} \right)$$
Once again, the integral is a structure factor of order unity, to be determined numerically. 

In the special case of an isothermal power-law atmosphere, the structure-factor integral simplifies to
  $$\int_{-\infty}^{\infty} \left( \frac {r} {r_\perp} \right)^{-2\alpha} ~d \left( \frac {r_\parallel} {r_\perp} \right) =  \frac {\pi^{1/2} \Gamma \left( \alpha - \frac {1} {2} \right)} {\Gamma \left( \alpha \right)}$$
and reduces to $\pi/2$ for $\alpha = 2$. 

{: .note}
Be aware the integral of $n_e n_{\rm H}$ over volume (rather along a line of sight) is sometimes called an "emission measure." That volume integral is called an "emission normalization" in the **ExpCGM** framework and is defined among the *Cumulative Profiles* described below.


### Line Intensity

A particular emission line has a temperature-dependent emissivity $\epsilon_{\rm line} (T)$, defined so that $4 \pi n_e n_{\rm H} \epsilon_{\rm line}(T)$ is the emission rate of line energy per unit volume. Isotropic emission in an isothermal atmosphere therefore produces a line of intensity
  $$I_{\rm line} (r_\perp) = \epsilon_{\rm line}(T) \cdot {\rm EM} (r_\perp)$$
at projected radius $r_\perp$. The line's intensity then has units of energy/time/area/solid angle. 

Line emission from an atmosphere that is not isothermal requires numerical integration to obtain
  $$I_{\rm line} (r_\perp) = \int_{-\infty}^{\infty} n_e n_{\rm H} \epsilon_{\rm line} (T) ~d r_\parallel$$
Currently, **ExpCGM** models do not automatically provide $I_{\rm line}$ as standard output. However, a user interested in the line-intensity profile of a non-isothermal atmopshere can perform the necessary integration using the radial-profile output.

{: .note}
Sometimes the emission line of interest will be from a region of the atmosphere with a temperature distinctly different from most of the atmosphere, such as a low-temperature cloud in pressure equilibrium with the rest of the gas at its location. In that case, the integral should be limited to that region. See the [Multiphase Gas](MultiphaseGas) page for more on how gas components with $T \ll T_\varphi$ are treated in the **ExpCGM** framework.


### Spectral Intensity

A galactic atmosphere's overall spectrum comes from both line emission and continuum emission processes collectively represented by the quantity $\epsilon_\nu (T)$. This version of emissivity is defined so that $4 \pi n_e n_{\rm H} \epsilon_\nu ~ \Delta \nu$ is the rate of energy emission per unit volume within a narrow frequency interval $\Delta \nu$ containing the frequency $\nu$. An **ExpCGM** atmosphere model therefore predicts that emission at frequency $\nu$ has an intensity 
  $$I_\nu (r_\perp | Z) = \int_{-\infty}^{\infty} n_e n_{\rm H} \epsilon_\nu (T,Z) ~d r_\parallel$$
at projected radius $r_\perp$. This ***spectral intensity*** has units of energy/time/area/solid angle/frequency and usually depends on the mass fraction $Z$ of elements other than H and He. An **ExpCGM** user interested in $I_\nu$ may specify as an auxilliary input parameter the mass fraction $Z$ of heavy elements in units of the solar value $Z_\odot$.

{: .note}
Forward modeling that convolves $I_\nu (r_\perp)$ with an instrumental response function and fits the result to an instrument's signal is an alternative to deprojection that makes the uncertainties in input model parameters easier to assess.


### Surface Brightness

Integrating $I_\nu$ over all frequencies gives the atmosphere's ***bolometric surface brightness*** 
  $$I_{\rm bol} (r_\perp | Z) = \int_0^\infty I_\nu (r_\perp | Z) ~d\nu$$
Observations of surface brightness generally remain within a specific frequency band going from $\nu_{\rm min}$ to $\nu_{\rm max}$, in which the ***band surface brightness*** is 
  $$I_{\rm band | Z } (~ r_\perp ~|~ Z , \nu_{\rm min} , \nu_{\rm max} ~) = \int_{\nu_{\rm min}}^{\nu_{\rm max}} I_\nu (r_\perp | Z) ~d\nu$$
**ExpCGM** users can specify the desired band range $[\nu_{\rm min} , \nu_{\rm max}]$ for model output.

{: .note}
More precise comparisons with observations may require multiplying the **ExpCGM** model prediction for $I_\nu$ by a more complex frequency-dependent kernel function before integrating it over frequency. For example, soft X-ray absorption by the Milky Way's interstellar medium can attenuate the X-ray signal coming from an extragalactic source. If the interstellar X-ray optical depth along that direction is $\tau_\nu$, then the kernel function accounting for it is $\exp ( - \tau_\nu )$.

### Minimum Pressure

Another constraint on projected **ExpCGM** models comes from UV absorption-line observations. Photoionization models of such an absorbing cloud at projected radius $r_\perp$ provide a ***minimum pressure*** estimate $P_{\rm min} (r)$ at $r \approx r_\perp$. That estimate is likely to be a lower limit on the atmospheric pressure at $r$ for two reasons:

1. The cloud's actual distance from the atmosphere's center may be greater than $r_\perp$, and thermal pressure usually declines with radius in a galactic atmosphere.
2. Thermal pressure may not be the only source of pressure keeping the cloud from being compressed by its surroundings. 


## Cumulative Profiles

Sometimes the only information we can gather about a galactic atmosphere is spatially unresolved and corresponds to a projected model quantity integrated over solid angle. The output of an **ExpCGM** model can provide predictions for some of those integrated properties. 

### Integrated Compton Parameter Profile

Integrating a spherical atmosphere's Compton parameter over projected radius yields the ***integrated Compton parameter***
  $$Y_{\rm SZ}(r_\perp) = 2 \pi \int_0^{r_\perp} y(r_\perp) ~r_\perp d r_\perp$$
The integral has units of area and diverges at small radii unless $\alpha_{\rm in} < 3$.

Dividing $Y_{\rm SZ}$ by the effective area of the full sky at a halo's redshift $z$ gives the dimensionless quantity
  $$Y_{\rm SZ,obs}(r_\perp) = \frac {Y_{\rm SZ}} {4 \pi d_{\rm A}^2 (z)}$$
Here, $d_{\rm A} (z)$ is the cosmological angular size distance at the atmosphere's redshift $z$. The detectability of an unresolved tSZ source depends on the magnitude of $Y_{\rm SZ,obs}$.

{: .note}
The integral for $Y_{\rm SZ}$ diverges at small radii for $\alpha_{\rm in} > 3$ because the atmosphere's thermal energy unphysically diverges there. It diverges at large radii for $\alpha_{\rm out} < 3$ because the atmosphere's total thermal energy then does not converge.  


### Bolometric Luminosity Profile

Integrating the bolometric surface brightness over projected area gives the atmosphere's bolometric luminosity profile:
  $$L_{\rm bol} (r_\perp) = 8 \pi^2 \int_0^{r_\perp} I_{\rm bol} (r_\perp | Z) ~r_\perp d r_\perp$$
This quantity may depend on the atmosphere's heavy-element abundance $Z$, which a user can specify in units of $Z_\odot$ along with the input parameter set.

{: .note}
This integral for $L_{\rm bol}$ and the luminosity integrals that follow all diverge at small radii for $\alpha_{\rm in} > 3/2$ and at large radii for $\alpha_{\rm out} < 3/2$. To obtain converged values of $Y_{\rm SZ}$ and $L_{\rm bol}$ from **ExpCGM** models, users need to specify a shape function that has $\alpha_{\rm in} < 3/2$ and $\alpha_{\rm out} > 3$.

### Band Luminosity Profile

Integrating the band surface brightness over projected area gives the atmosphere's luminosity in the band $[\nu_{\rm min} , \nu_{\rm max}]$:
  $$L_{\rm band} (Z) = 8 \pi^2 \int I_{\rm band}(r_\perp | Z) ~r_\perp d r_\perp$$
This quantity also may depend on the heavy-element abundance $Z$.

### Emission Normalization Profile

Integrating ${\rm EM}(r_\perp)$ over projected area gives the ***emission normalization*** corresponding to an integral of $n_e n_{\rm H}$ over the cylindrical volume within $r_\perp$:
  $${\rm EN} (r_\perp) = 2 \pi \int_0^{r_\perp} {\rm EM}(r_\perp) ~r_\perp d r_\perp$$
Mulitplying ${\rm EN} (r_\perp)$ by $4 \pi \epsilon_{\rm line} (T)$ gives the line luminosity profile $L_{\rm line} (r_\perp)$ for an isothermal atmosphere. Users interested in line-luminosity predictions for non-isothermal atmospheres should use a model's radial-profile output to perform the necessary integrals.

{: .note}
The **ExpCGM** framework calls ${\rm EN} (r_\perp)$ an "emission normalization" because the volume integral
  $$\frac {10^{-14}} {4 \pi d_{\rm A}^2 (1 + z)^2} \int n_e n_{\rm H} dV$$
gives a quantity often called a "normalization" in the X-ray astronomy literature. Elsewhere, ${\rm EN}$ is sometimes called an "emission measure."

### Density Contrast Profile

Many **ExpCGM** users will want to know an atmosphere's predicted luminosity or $Y_{\rm SZ}$ value within a radius corresponding to a particular mass density contrast $\Delta_{\rm c}$ relative to the cosmological critical density $\rho_{\rm cr} (z) = 3 H^2 (z) / 8 \pi G$. For that purpose, the **ExpCGM** framework provides along with the atmosphere model output both a total mass profile      
  $$M_{\rm tot} (r) = \frac {v_{\rm c}^2 (r) r} {G}$$
and a density contrast profile 
  $$\Delta_{\rm c} (r) = \frac {2 v_c^2 (r)} {H^2(z) r^2}$$ 
Both profiles include all of the mass components specified in the input parameter set. Having that information provides users with the flexibility to use their preferred value of $\Delta_{\rm c}$ and to convert among observable properties corresponding to different values of $\Delta_{\rm c}$.

## Summary of Model Output

### Radial Profiles

| Output | Description |
| :-------: | ----------- |
|  $P(r)$  | Thermal pressure profile |
|  $T(r)$  | Temperature profile |
| $\rho (r)$ | Gas density profile | 
| $f_{\rm th} (r)$ | Thermalization profile (if $f_{\rm th}$ is a parametric function of $r$) |

### Projected Profiles

| Output | Description |
| :-------: | ----------- |
|  $\Sigma_{\rm CGM}(r_\perp)$  | Surface mass density profile |
|  $N_{\rm H}(r_\perp)$  | Hydrogen column density profile |
|  $N_e(r_\perp)$  | Electron column density profile |
|  $y(r_\perp)$  | Compton parameter profile |
|  ${\rm EM}(r_\perp)$  | Emission measure profile |
|  $I_\nu(r_\perp \| Z)$  | Spectral intensity profile |
|  $I_{\rm bol}(r_\perp \| Z)$  | Bolometric intensity profile |
|  $I_{\rm band}(r_\perp \| Z , \nu_{\rm min} , \nu_{\rm max})$  | Band intensity profile |

### Cumulative Profiles

| Output | Description |
| :-------: | ----------- |
|  $Y_{\rm SZ}(r_\perp)$  | Integrated Compton parameter profile |
|  $L_{\rm bol}(r_\perp \| Z)$  | Bolometric luminosity profile |
|  $L_{\rm band}(r_\perp \| Z , \nu_{\rm min} , \nu_{\rm max})$  | Band luminosity profile |
|  ${\rm EN}(r_\perp)$ | Emission normalization profile |
|  $M_{\rm tot} (r)$   | Total mass profile |
|  $\Delta_{\rm c}(r)$ | Spherical density contrast profile, relative to $\rho_{\rm cr}(z)$ |


