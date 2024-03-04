---
title: Cooling
layout: default
nav_order: 3
parent: Extensions
---

<head>

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

# Cooling
{: .no_toc}

Radiative cooling causes the spatial distribution of baryons in a cosmological halo to deviate from what purely cosmological structure formation would produce. Emission of photons removes the original cosmological specific energy of the baryons, allowing cold gas to accumulate at the halo's center. There it can form stars and give birth to a galaxy. Some of the galaxy's baryons may accrete onto a central black hole. The resulting feedback energy supply from supernova explosions and black-hole accretion then further alters the atmosphere.

This page describes how the **ExpCGM** framework treats radiative cooling and proposes some extensions of the basic framework. Its first section discusses the cooling time of atmospheric gas. Its second section outlines how cooling is related to specific entropy, which determines an atmosphere's structure as discussed on the [Pressure Profiles](PressureProfiles) page. Its third section quantifies how losses of specific entropy result in cooling flows. The fourth section considers extensions of **ExpCGM** that account for atmospheric inhomogeneity and how it alters cooling and inflow rates. A concluding section summarizes how **ExpCGM** determines an atmosphere's total radiative cooling rate $\dot{E}\_{\rm rad}$.

<details closed markdown="block">
  <summary>
    Contents
  </summary>
   {: .text-delta}
- TOC
{:toc}  
</details>

## Cooling Time

Collisional excitation of photons removes thermal energy from a galaxy's atmosphere, allowing it to contract. The rate of energy emission per unit volume is proportional to $\rho^2$, because collisional excitation depends on two-particle interactions. In an atmosphere with a well-defined temperature, it is also proportional to $\Lambda (T)$, a ***cooling function*** that depends on temperature, because $T$ determines both the ionization state of the atmosphere and the speeds of the colliding particles.

Astrophysical cooling functions are typically defined in terms of electron density $n_e$ and ion density $n_i$ (or proton density $n_p$), so that 
 $$n_e n_i \Lambda (T)$$
represents the emission rate of radiative energy per unit volume. Sometimes, the **ExpCGM** documentation uses an alternative cooling function
  $$\Lambda_\rho (T) \equiv \frac {n_e n_i} {\rho^2} \Lambda (T)$$
defined so that $\rho \Lambda_\rho (T)$ is the emission rate of radiative energy per unit mass, also known as the ***specific cooling rate***.

Steady emission radiates an amount of energy equivalent to an atmosphere's thermal  energy on a timescale
  $$t_{\rm cool} = \frac {3} {2} \frac {P} {\rho^2 \Lambda_\rho} = \frac {3} {2} \frac {P} {n_e n_i \Lambda (T)} \approx \frac {3kT} {n_e \Lambda (T)}$$
called a ***cooling time***. However, an atmosphere's temperature does not necessarily decline on that timescale because of gravitational compression. Its temperature remains approximately constant if $t_{\rm cool}$ is longer than the atmosphere's dynamical timescale $t_{\rm dyn} = r / v_{\rm c}$ and stays close to
  $$T \approx \left( \frac {2 f_{\rm th}} {\alpha_{\rm eff}} \right) T_\varphi  = \left( \frac {2  f_{\rm th}} {\alpha_{\rm eff}} \right) \frac {\mu m_p v_{\rm c}^2} {2k}$$
(See the [Essentials](Essentials) page for an explanation and definitions of symbols.) 

{: .note} 
A galactic atmosphere's cooling function $\Lambda (T)$ may also depend on density if the atmosphere's density is low enough for ionizing background radiation to alter its ionization state. Atmosphere models for low-mass galaxies or early eras when the ionizing background is particularly intense may need to account for this change in the cooling function.

## Specific Entropy

When gravitational compression is acting, radiative losses reduce an atmosphere's specific entropy more directly than they reduce its temperature. According to the first law of thermodynamics, the total entropy $S$ within a volume $V$ of radiatively cooling atmospheric gas with thermal energy $U$ declines as 
  $$T \frac {dS} {dt} ~=~ \frac {dU} {dt} +  P \frac {dV} {dt} ~=~ - \frac {U} {t_{\rm cool}}$$
The thermal energy of an atmosphere consisting of non-relativistic particles with no internal degrees of freedom is $U = 3PV/2$. Dividing the first law by $PV$ therefore yields 
  $$\frac {T} {PV} \frac {dS} {dt} ~=~ \frac {5} {2} \frac {1} {V} \frac {dV} {dt} + \frac {3} {2} \frac {1} {P} \frac {dP} {dt} ~=~ - \frac {3} {2} \frac {1} {t_{\rm cool}}$$
which implies
  $$\frac {d} {dt} \ln (P V^{5/3} ) ~=~ - \frac {1} {t_{\rm cool}}$$
The quantity $K = P \rho^{-5/3}$ in an atmospheric gas sample of constant mass $\rho V$ then does not change if $t_{\rm cool}$ is large, giving the gas a polytropic equation of state
  $$P = K \rho^{\gamma}$$
with $\gamma = 5/3$. 

Changes in the logarithm of the constant of proportionality $K$ in this equation of state are directly proportional to changes in the specific entropy $S / (\rho V)$:
  $$\Delta \ln K = \frac {2} {3} \frac {\Delta S} {k(\rho V)}$$
The value of $K$ therefore specifies the *adiabat* of a sample of atmospheric gas. Uncompensated radiative cooling changes the adiabat of that sample according to the entropy equation:
  $$\frac {d \ln K} {dt} = - \frac {1} {t_{\rm cool}}$$

{: .note}
In the scientific literature on galactic atmospheres, the quantity $K$ is often loosely called the "entropy" of atmospheric gas. It is in fact the *logarithm* of specific entropy and is specified using a variety of units that may seem to be incompatible with each other. However, the units of $K$ have no operational significance, because they correspond to a normalization factor that vanishes when differences in $\ln K$ are assessed. Only *changes* in $\ln K$ have physical meaning. One is therefore free to choose units for $K$ that reflect the observations used to measure it.  

## Homogeneous Cooling Flows

Uncompensated cooling causes a galactic atmosphere to contract, resulting in a ***cooling flow*** with an inward mass flux

<p>
  $$\dot{M}_{\rm cool} = 4 \pi r^2 \rho v_{\rm in} = \frac {4 \pi r^3 \rho} {t_{\rm flow}}$$
</p>

in which $t_{\rm flow} = r / v_{\rm in}$ is the inflow timescale corresponding to the inflow speed $v_{\rm in} = - dr/dt$. The ratio of flow time to cooling time is related to the gradient of specific entropy via
  $$\alpha_K ~\equiv~ \frac {d \ln K} {d \ln r} ~=~ r \left( \frac {dt} {dr} \right)^{-1} \frac {d \ln K} {d t} ~=~ \frac {t_{\rm flow}} {t_{\rm cool}}$$
Therefore, the inflow speed of a pure homogeneous cooling flow is
  $$v_{\rm in} (r) = \frac {r} {\alpha_K t_{\rm cool} (r)}$$
and atmospheric gas flows inward at the rate
<p>
  $$\dot{M}_{\rm cool} (r) = \frac {4 \pi r^3 \rho (r)} {\alpha_K t_{\rm cool}(r)}$$
</p>
An isothermal steady-state cooling flow in which $t_{\rm cool} \propto 1 / \rho$ therefore has a density profile with $\rho \propto r^{-3/2}$, implying $\alpha = 3/2$ and $\alpha_K = 1$. 

## Inhomogeneous Cooling

Observations show that inhomogeneity is commonplace in galactic atmospheres, with adjacent regions having densities and temperatures that can differ by orders of magnitude. Therefore, the local cooling time of atmospheric gas can greatly differ from a value of $t_{\rm cool}$ derived by averaging $\rho$ and $T$ over the atmospheric shell at radius $r$. This section discusses extensions of **ExpCGM** that can account for those inhomogeneities.

### Log-Normal Distributions

The **ExpCGM** framework presumes that inhomogeneous atmospheric gas is in pressure equilibrium and can be extended to represent moderate inhomogeneity in terms of log-normal distributions. Isobaric density, temperature, and entropy fluctuation amplitudes relative to the mean are therefore related through
  $$\delta \ln \rho = - \delta \ln T = - \frac {3} {5} \, \delta \ln K$$
and the standard deviations of those fluctuation amplitudes are related through
  $$\sigma_{\ln \rho} = - \sigma_{\ln T} = - \frac {3} {5} \sigma_{\ln K}$$
Cooling time also has a log-normal distribution, as long as the cooling function's dependence on temperature can be described with the power-law parameter
  $$\lambda \equiv \frac {d \ln \Lambda (T)} {d \ln T}$$
In that case, one finds

<p>
  $$\delta \ln t_{\rm cool} ~=~ - (2 - \lambda) \delta \ln \rho ~=~ \frac {3 (2 - \lambda)} {5} \, \delta \ln K$$
  </p>

for an atmosphere in pressure equilibrium.

### Cooling-Time Fluctuations

Density fluctuations can result in inhomogenous cooling that spurs development of a multiphase medium. For example, consider an atmospheric layer with density fluctuations having a log-normal dispersion $\sigma_{\ln \rho} \approx 0.8$. The cooling-time fluctuations in that layer generally have a greater amplitude than the density fluctuations. They are particularly pronounced in the temperature range $10^5 \, {\rm K} < T < 10^7 \, {\rm K}$, in which $\lambda$ approaches $-1$, giving
  $$\delta \ln t_{\rm cool} \approx - 3 \, ( \delta \ln \rho )$$
Density fluctuations with $\sigma_{\ln \rho} \approx 0.8$ then correspond to cooling-time fluctuations with $\sigma_{\ln t_{\rm cool}} \approx 2.4$, meaning that $t_{\rm cool}$ in the densest regions is more than an order of magnitude shorter than the layer's mean value of $t_{\rm cool}$. Those regions therefore lose entropy much faster than the more diffuse regions, causing the contrast between the layer's high and low density regions to increase, no matter what the mean heating rate, unless the heat input is somehow strongly skewed toward the layer's denser and cooler regions.

The situation just described is a classic astrophysical thermal instability. However, when such an instability develops in a stratified galactic atmosphere, buoyancy produces complex consequences as the denser regions of each layer start to sink toward lower altitude. A future *Thermal Instability* page will discuss the development and implications of such a multiphase atmosphere in greater depth.

## Radiative Losses

In the **ExpCGM** framework, the total radiative loss rate of thermal energy within the bounding radius $r_{\rm CGM}$ of a galaxy's atmosphere is

<p>
  $$\dot{E}_{\rm rad} ~=~ \int_0^{M_{\rm CGM}} \rho \Lambda_\rho ~dM_{\rm gas} ~=~ \frac {3} {2} \int_0^{r_{\rm CGM}} \frac {P(r)} {t_{\rm cool} (r)} 4 \pi r^2 dr $$
</p>

In the latter integral, the cooling time

<p>
 $$t_{\rm cool} (r) = \frac {3} {2} \frac {P(r)} {\bar{\rho}(r) \langle \rho \Lambda_\rho \rangle}$$
</p>

is an average cooling time based on the mean density $\bar{\rho}(r)$ and specific cooling rate $\langle \rho \Lammbda_\rho \rangle$ of the atmospheric shell at radius $r$.

### Homogenous Atmosphere

Simple **ExpCGM** models assume homogeneous gas. Making the substitutions $P(r) = P_0 f_P (r / r_0)$ and $r_{\rm CGM} = r_0 x_{\rm CGM}$ therefore gives

<p>
  $$\dot{E}_{\rm rad} =  \frac {3} {2} \cdot \frac {4 \pi P_0 r_0^3} {t_{\rm cool}(r_0)} \cdot \int_0^{x_{\rm CGM}} \left[ \frac {\Lambda (r) / T^2 (r)}  { \Lambda (r_0) / T^2(r_0)} \right] x^2 f_P^2 (x)  dx$$
</p>

for a homogeneous atmosphere. The temperature-dependent factor in square brackets is approximately constant with radius in a nearly isothermal potential well. Therefore, the pressure profile's shape function $\alpha (r)$, from which $f_P (x)$ is derived, determines how radiative losses are distributed as a function of radius. (See the [Essentials](Essentials) page for definitions of the symbols used here.)

Notice that the integral for $\dot{E}\_{\rm rad}$ diverges at small $r$ with $f_P (x) = x^{-\alpha}$ and $\alpha \geq 3/2$, if the factor in square brackets is a constant or declining function of radius. In that case, something needs to be done to truncate the integral at small radii. One option is to truncate the integral at $r_{\rm gal}$. Another is to use a shape function with some curvature, so that $\alpha < 3/2$ at $r \ll r_0$ and $\alpha > 3/2$ at $r \gg r_0$. (See the [Pressure Profiles](PressureProfiles) page for some physically motivated shape function options.) 

Observations of galaxy clusters and groups show that their pressure profiles do indeed have shape functions with the curvature properties necessary for $\dot{E}_{\rm rad}$ in a homogeneous atmosphere to converge at both small and large $r$.

### Log-Normal Inhomogeneity

An inhomogeneous atmosphere has density fluctuations that boost $\dot{E}\_{\rm rad}$ above expectations for a homogeneous atmosphere. Assuming isobaric log-normal density fluctuations with a dispersion $\sigma_{\ln \rho}$ that is independent of radius leads to a boost factor
  $$f_{\rm rad} \approx ( 2 - \lambda ) \sigma_{\ln \rho}$$
Multiplying $\dot{E}\_{\rm rad}$ for the homogeneous case by this boost factor accounts for the enhanced emissivity of high-density fluctuations. 

### Multiphase Inhomogeneity

A multiphase medium with embedded clouds orders of magnitude denser than the ambient gas may experience even greater radiative losses that require a more complex assessment of the boost factor. The radiative loss rate per unit mass from a multiphase gas sample of mass $M_{\rm gas}$ can be expressed as

<p>
  $$ \langle \rho \Lambda_\rho \rangle  = \frac {1} {M_{\rm gas}} \int_{M_{\rm gas}} \rho \Lambda_\rho ~d M_{\rm gas}$$
</p>

Users of **ExpCGM** who wish to incorporate additional radiative losses from a multiphase medium need to supply an algorithm that determines the boost factor
  $$f_{\rm rad} = \frac {\langle \rho \Lambda_\rho \rangle} {\bar{\rho} \Lambda_\rho(T)}$$
This factor represents how the specific radiative loss rate of a multiphase gas sample of mean density $\bar{\rho}$ compares with the specific radiative loss rate of homogeneous gas with the same pressure $P$ and therefore a temperature $T = \mu m_p P / k \bar{\rho}$. 

Such an algorithm should be informed by high-resolution numerical simulations of turbulent radiative mixing layers in thermally unstable gas. It may depend on the local pressure and temperature of the volume-filling gas that presumably confines the denser clouds. It may also depend on the fraction $f_{\rm cool}$ of the gas mass with $T \ll T_\varphi$ and the velocity dispersion $\sigma_{\rm 1D,cool}$ of the cool-cloud population. (See the [Multiphase Gas](MultiphaseGas) page for more on how $f_{\rm cool}$ and $\sigma_{\rm 1D,cool}$ are related to the atmosphere's thermalization fraction $f_{\rm th})$. 
