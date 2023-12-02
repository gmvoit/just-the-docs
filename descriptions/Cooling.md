---
title: Cooling
layout: default
nav_order: 4
parent: Description
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

Radiative cooling is what causes a galactic atmosphere to deviate from what purely cosmological structure formation would produce. Emission of photons removes some of the atmosphere's cosmological specific energy, allowing cold gas to accumulate at the halo's center, where it can form stars and accrete onto a central black hole. The resulting feedback energy released by supernova explosions and black-hole growth then further alters the atmosphere.

This page describes how the **ExpCGM** framework accounts for radiative cooling. Its first section defines the cooling time of atmospheric gas. Its second section outlines the relationship between cooling and specific entropy. Radiative cooling limits the central galaxy's gas supply as discussed in the third section. The fourth section considers atmospheric inhomogeneity and how to account for it. The concluding section explains how **ExpCGM** computes the atmosphere's total radiative cooling rate $\dot{E}\_{\rm rad}$.

<details closed markdown="block">
  <summary>
    Contents
  </summary>
   {: .text-delta}
- TOC
{:toc}  
</details>

## Cooling Time

Collisional excitation of photons removes thermal energy from a galaxy's atmosphere, allowing it to contract. The rate of energy emission per unit volume is proportional to $\rho^2$, because collisional excitation depends on two-particle collisions. In an atmosphere with a well-defined temperature, it is also proportional to $\Lambda (T)$, a ***cooling function*** that depends on temperature, because $T$ determines both the ionization state of the atmosphere and the speeds of the colliding particles.

Astrophysical cooling functions are usually defined in terms of electron density $n_e$ and ion density $n_i$ (or proton density $n_p$), so that 
 $$n_e n_i \Lambda (T)$$
represents the emission rate of radiative energy per unit volume. An alternative cooling function sometimes used in **ExpCGM** documentation is $\Lambda_\rho$, defined so that $\rho \Lambda_\rho (T)$ is the rate of energy emission per gas particle.

Steady emission radiates an amount of energy equivalent to an atmosphere's thermal  energy on a timescale
  $$t_{\rm cool} = \frac {3} {2} \frac {kT} {\rho \Lambda_\rho} = \frac {3} {2} \frac {P} {n_e n_i \Lambda (T)} \approx \frac {3kT} {n_e \Lambda (T)}$$
called the ***cooling time***. However, the atmosphere's temperature does not necessarily decline on that timescale. Gravitational compression keeps the atmosphere's temperature approximately constant if $t_{\rm cool}$ is longer than the atmosphere's dynamical timescale ($\sim r / v_{\rm c})$. It therefore remains close to
  $$T \approx \left( \frac {2 f_{\rm th}} {\alpha_{\rm eff}} \right) T_\varphi  = \left( \frac {2  f_{\rm th}} {\alpha_{\rm eff} k} \right) \frac {\mu m_p v_{\rm c}^2} {2}$$
(See the [Essentials](Essentials) page for an explanation and definitions of symbols.) 

{: .note} 
Be aware that ionizing background radiation can alter $\Lambda (T)$ by further raising the atmosphere's ionization state if the atmosphere's density is low enough. Atmosphere models for low-mass galaxies or early eras when the ionizing background is particularly intense may need to account for this change in the cooling function.

## Specific Entropy

Radiative losses reduce an atmosphere's specific entropy more directly than they reduce its temperature. According to the first law of thermodynamics, the total entropy $S$ within a volume $V$ of radiatively cooling atmospheric gas with thermal energy $U$ declines as 
  $$T \frac {dS} {dt} ~=~ \frac {dU} {dt} +  P \frac {dV} {dt} ~=~ - \frac {U} {t_{\rm cool}}$$
The thermal energy of an atmosphere consisting of non-relativistic particles with no internal degrees of freedom is $U = 3PV/2$. Dividing the first law by $PV$ therefore yields 
  $$\frac {T} {PV} \frac {dS} {dt} ~=~ \frac {5} {2} \frac {1} {V} \frac {dV} {dt} + \frac {3} {2} \frac {1} {P} \frac {dP} {dt} ~=~ \frac {3} {2} \frac {d} {dt} \ln (P V^{5/3} ) ~=~ - \frac {3} {2} \frac {1} {t_{\rm cool}}$$
The quantity $K = P \rho^{-5/3}$ in an atmospheric gas sample of mass $\rho V$ then does not change if $t_{\rm cool}$ is large, giving the gas a polytropic equation of state
  $$P = K \rho^{\gamma}$$
with $\gamma = 5/3$. 

Changes in the logarithm of the constant of proportionality $K$ in this equation of state are directly proportional to changes in the specific entropy $S / (nV)$:
  $$\Delta \ln K = \frac {2} {3} \frac {\Delta S} {k(nV)}$$
The value of $K$ therefore specifies the *adiabat* of a sample of atmospheric gas, and the entropy equation
  $$\frac {d \ln K} {dt} = - \frac {1} {t_{\rm cool}}$$
determines how radiative cooling changes the adiabat of that sample.

{: .note}
In the scientific literature on galactic atmospheres, the quantity $K$ is often loosely called the "entropy" of atmospheric gas, when in fact it is logarithm of specific entropy. It is also specified using a variety of units that may seem to be incompatible with each other. However, the units of $K$ have no operational significance, because they correspond to a normalization factor that vanishes when differences in $\ln K$ are assessed. Only *changes* in $\ln K$ have physical meaning. One is therefore free to choose units for $K$ that reflect the observations used to measure it.  

## Cooling Flows

Uncompensated cooling causes a galactic atmosphere to contract, resulting in a ***cooling flow*** with an inward mass flux

<p>
  $$\dot{M}_{\rm cool} = 4 \pi r^2 \rho v_{\rm in} = \frac {4 \pi r^3 \rho} {t_{\rm flow}}$$
</p>


in which $t_{\rm flow} = r / v_{\rm in}$ is the inflow timescale corresponding to the inflow speed $v_{\rm in} = - dr/dt$. The ratio of flow time to cooling time is related to gradient of specific entropy via
$$\alpha_K ~\equiv~ \frac {d \ln K} {d \ln r} ~=~ r \left( \frac {dt} {dr} \right)^{-1} \frac {d \ln K} {d t} ~=~ - t_{\rm flow} \frac {d \ln K} {d t} ~=~ \frac {t_{\rm flow}} {t_{\rm cool}}$$
Therefore, the equation

<p>
  $$\dot{M}_{\rm cool} (r) = \frac {4 \pi r^3 \rho (r)} {\alpha_K t_{\rm cool}(r)}$$
</p>


represents the expected cooling flow rate at radius $r$.

### Steady Cooling

The quantity $\dot{M}\_{\rm cool}$ is an *expected* cooling flow rate because we have not yet established whether it is a *steady* cooling flow rate. Its steadiness depends on whether $\dot{M}\_{\rm cool}$ is independent of radius. In an atmosphere with $P \propto r^{-\alpha}$, the local cooling flow rate $\dot{M}\_{\rm cool}$ is approximately constant with radius as long as the quantity
  $$\frac {r^3 \rho} {t_{\rm cool}} ~\propto~ r^3 P^2 \left[ \frac {\Lambda (T)} {T^3} \right] ~\propto~ r^{3-2 \alpha} \left[ \frac {\Lambda (T)} {T^3} \right]$$
is approximately constant with radius. A steady cooling flow in a nearly isothermal gravitational potential therefore has $\alpha \approx 3/2$, because gravitational compression assures that $T$ remains approximately constant with radius. Wherever those conditions apply, combining $K(r) \propto P^{-2/3} T^{5/3}$ and $\alpha \approx 3/2$ gives $\alpha_K \approx 1$ and a pure cooling flow rate

<p>
  $$\dot{M}_{\rm cool} (r) \approx \frac {4 \pi r^3 \rho (r)} {t_{\rm cool}(r)} = \frac {8 \pi r^3} {3 kT} \rho^2(r) \Lambda_\rho[T(r)]$$
</p>


### Cooling-Limited Gas Supply

The steady-cooling equation relating $\dot{M}\_{\rm cool}$ to $r^3 \rho / t_{\rm cool}$ provides a useful upper limit on how quickly radiative cooling allows atmospheric gas to flow into a halo's central galaxy. **ExpCGM** implements that estimate. Users need to define a radius $r_{\rm gal}$ for the galaxy they are modeling. The framework then evaluates $\dot{M}\_{\rm cool} (r_{\rm gal})$ to determine the maximum gas supply that atmospheric cooling allows. 

{: .important}
Cooling gas can be flowing into a halo's central galaxy even if feedback energy input exceeds total radiative cooling. What matters more is how *local* heating compares to *local* cooling in the vicinity of $r_{\rm gal}$. If cooling locally exceeds heating in at least some atmospheric regions near $r_{\rm gal}$, then a global excess of heating does not immediately choke off the galaxy's gas supply. In the **ExpCGM** framework, excess heating instead limits the central galaxy's gas supply by expanding its atmosphere, resulting in a gradual decline in $\dot{M}\_{\rm cool}(r_{\rm gal})$ as expansion lowers the atmosphere's overall density.

### Dissipation-Limited Gas Supply

In galactic atmospheres with particularly short cooling times ($t_{\rm cool} \ll r / v_{\rm c}$), the pure cooling rate $\dot{M}\_{\rm cool}(r)$ overestimates how quickly atmospheric gas at radius $r$ can descend toward the central galaxy. The central galaxy's gas supply $\dot{M}\_{\rm in}$ then depends on how rapidly the atmosphere's kinetic energy can dissipate as gas falls inward. Its ultimate limit is the ballistic gravitational infall rate obtained by replacing $t_{\rm cool}$ with a free-fall timescale $t_{\rm ff}$. See the [Multiphase Gas](MultiphaseGas) page for more about how **ExpCGM** handles galactic atmospheres with short cooling times.

## Inhomogeneous Cooling

The previous section applies to homogeneous cooling flows, but galactic atmospheres can be highly inhomogeneous. Observations show that inhomogeneity is commonplace, with adjacent regions having densities and temperatures that can differ by orders of magnitude. Therefore, the local cooling time of atmospheric gas can greatly differ from the value of $t_{\rm cool}$ derived by averaging $\rho$ and $T$ over the atmospheric shell at radius $r$. 

### Log-Normal Distributions

The **ExpCGM** framework presumes that inhomogeneous atmospheric gas is in pressure equilibrium and represents moderate inhomogeneity in terms of log-normal distributions. Isobaric density, temperature, and entropy fluctuation amplitudes relative to the mean are therefore related through
  $$\delta \ln \rho = - \delta \ln T = - \frac {3} {5} \, \delta \ln K$$
and the standard deviations of those fluctuation amplitudes are related through
  $$\sigma_{\ln \rho} = - \sigma_{\ln T} = - \frac {3} {5} \sigma_{\ln K}$$
Cooling time also has a log-normal distribution, as long as the cooling function's dependence on temperature can be described with the power-law parameter
  $$\lambda \equiv \frac {d \ln \Lambda (T)} {d \ln T}$$
In that case, one finds

<p>
  $$\delta \ln t_{\rm cool} ~=~ - (2 - \lambda) \delta \ln \rho ~=~ (2 - \lambda) \delta \ln T  ~=~ \frac {3 (2 - \lambda)} {5} \, \delta \ln K$$
  </p>

  
for an atmosphere in pressure equilibrium.

### Cooling-Time Fluctuations

Those fluctuations can result in inhomogenous cooling that spurs development of a multiphase medium. For example, consider an atmospheric layer with density fluctuations having a log-normal dispersion $\sigma_{\ln \rho} \approx 0.8$. The cooling-time fluctuations in that layer generally have a greater amplitude than the density fluctuations. They are particularly pronounced in the temperature range $10^5 \, {\rm K} < T < 10^7 \, {\rm K}$, in which $\lambda$ approaches $-1$, giving
  $$\delta \ln t_{\rm cool} \approx - 3 \, ( \delta \ln \rho )$$
Density fluctuations with $\sigma_{\ln \rho} \approx 0.8$ then correspond to cooling-time fluctuations with $\sigma_{\ln t_{\rm cool}} \approx 2.4$, meaning that $t_{\rm cool}$ in the densest regions is more than an order of magnitude shorter than the layer's mean value of $t_{\rm cool}$. Those regions therefore lose entropy much faster than the more diffuse regions, causing the contrast between the layer's high and low density regions to increase, no matter what the mean heating rate, unless the heat input is somehow strongly skewed toward the layer's denser and cooler regions.

The situation just described is a classic astrophysical thermal instability. However, when such an instability develops in a stratified galactic atmosphere, buoyancy produces complex consequences as the denser regions of each layer start to sink toward lower altitude. The [Thermal Instability](ThermalInstability) page describes the development and implications of such a multiphase atmosphere in greater depth.

## Radiative Losses

In the **ExpCGM** framework, the total radiative loss rate of thermal energy within the bounding radius $r_{\rm CGM}$ of a galaxy's atmosphere is

<p>
  $$\dot{E}_{\rm rad} = \frac {3} {2} \int_0^{r_{\rm CGM}} \left( \frac {P} {t_{\rm cool}} \right) 4 \pi r^2 dr$$
</p>


### Homogenous Atmosphere

Simple **ExpCGM** models assume homogeneous gas. Making the substitutions $P(r) = P_0 f_P (r / r_0)$ and $r_{\rm CGM} = r_0 x_{\rm CGM}$ (see the [Essentials](Essentials) page for definitions of symbols) therefore gives

<p>
  $$\dot{E}_{\rm rad} =  \frac {3} {2} \cdot \frac {4 \pi P_0 r_0^3} {t_{\rm cool}(r_0)} \cdot \int_0^{x_{\rm CGM}} \left[ \frac {\Lambda (r) / T^2 (r)}  { \Lambda (r_0) / T^2(r_0)} \right] x^2 f_P^2 (x)  dx$$
</p>


for a homogeneous atmosphere. The temperature-dependent factor in square brackets is approximately constant with radius in a nearly isothermal potential well. Therefore, the pressure profile's shape function $\alpha (r)$, from which $f_P (x)$ is derived, determines how radiative losses are distributed as a function of radius.

Notice that the integral for $\dot{E}\_{\rm rad}$ diverges at small $r$ for $f_P (x) = x^{-\alpha}$ and $\alpha \geq 3/2$, if the factor in brackets is a constant or declining function of radius. In that case, something needs to be done to truncate the integral at small radii. One option is to truncate the integral at $r_{\rm gal}$. Another is to use a shape function with some curvature, so that $\alpha < 3/2$ at $r \ll r_0$ and $\alpha > 3/2$ at $r \gg r_0$ (see the [Pressure Profiles](PressureProfiles) page for some physically motivated shape function options). Observations of galaxy clusters and groups show that their pressure profiles do indeed have shape functions with the curvature properties necessary for $\dot{E}_{\rm rad}$ in a homogeneous atmosphere to converge at both small and large $r$.

### Log-Normal Inhomogeneity

An inhomogeneous atmosphere has density fluctuations that boost $\dot{E}\_{\rm rad}$ above expectations for a homogeneous atmosphere. Assuming isobaric log-normal density fluctuations with a dispersion $\sigma_{\ln \rho}$ that is independent of radius leads to a boost factor
  $$f_{\rm rad} \approx ( 2 - \lambda ) \sigma_{\ln \rho}$$
Multiplying $\dot{E}\_{\rm rad}$ for the homogeneous case by this boost factor accounts for the enhanced emissivity of high-density fluctuations. 

### Multiphase Inhomogeneity

A fully multiphase medium with embedded clouds orders of magnitude denser than the ambient gas may experience even greater radiative losses that require a more complex assessment of the boost factor. The general integral for the specific radiative loss rate from a gas sample of mass $M_{\rm gas}$ is

<p>
  $$ \langle n \Lambda_\rho \rangle  = \frac {1} {M_{\rm gas}} \int_{M_{\rm gas}} n \Lambda_\rho ~d M_{\rm gas}$$
</p>

where $n = \rho / \mu m_p$ is the particle number density. In a homogeneous medium of pressure $P$ and temperature $T$ this loss rate is $\langle n \Lambda_\rho \rangle = P \Lambda_\rho (T) / kT$.

Users who wish to model additional radiative losses from a multiphase medium need to supply an algorithm that determines the boost factor
  $$f_{\rm rad} = \frac {kT \langle n \Lambda_\rho \rangle} {P \Lambda_\rho(T)} $$
relating the homogeneous case to the multiphase case. Such an algorithm should be informed by high-resolution numerical simulations of turbulent radiative mixing layers in thermally unstable gas. It may depend on the local pressure and temperature of the volume-filling gas that presumably confines the denser clouds. It may also depend on the fraction $f_{\rm cool}$ of the gas mass with $T \ll T_\varphi$ and the velocity dispersion $\sigma_{\rm 1D,cool}$ of the cool-cloud population. (See the [Multiphase Gas](MultiphaseGas) page for more on how $f_{\rm cool}$ and $\sigma_{\rm 1D,cool}$ are related to the atmosphere's thermalization fraction $f_{\rm th}$.) 
