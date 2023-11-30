---
title: Multiphase Gas
layout: default
nav_order: 6
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

# Multiphase Gas
{: .no_toc}

{: .warning}
This page is still under construction.

The most basic **ExpCGM** models assume that a galactic atmosphere is homogeneous, with uniform values of pressure, gas density, and temperature at each radius. However, atmospheric gas with a radiative cooling time $t_{\rm cool}$ much shorter than the dissipation timescale $t_{\rm diss}$ of its non-thermal support energy is unlikely to remain homogeneous, as it is prone to thermal instability. 

High-density perturbations within such an atmosphere typically cool more rapidly than low-density perturbations, and dissipation is not rapid enough to replenish those radiative losses. Differential cooling therefore enhances any density contrasts that arise, producing gas clumps much denser than their surroundings, as long as thermal instability can progress to nonlinear amplitudes. (See the [Thermal Instability](ThermalInstability) page for more detail.) 

Astronomers often call gas with large density and temperature contrasts a *multiphase medium*. The properties of a multiphase galactic atmosphere can significantly differ from those of a homogeneous atmosphere with the same total mass $M_{\rm CGM}$ and total energy $E_{\rm CGM}$, altering both the gas supply to the central galaxy and the atmosphere's overall pattern of evolution. This page outlines how algorithms representing those characteristics of a multiphase atmosphere can be incorporated into the **ExpCGM** modeling framework.

<details closed markdown="block">
  <summary>
    Contents
  </summary>
   {: .text-delta}
- TOC
{:toc}  
</details>

## Objectives

Multiphase gas algorithms in **ExpCGM** need to be modestly scoped, because detailed modeling of a multiphase medium requires computational methods far beyond the sophistication of the **ExpCGM** framework. Remember that the framework's primary objective is to represent the global structure of a galactic atmosphere so that its coupling with the central galaxy can be modeled. It tracks how cosmological accretion augments the atmosphere's total energy $E_{\rm CGM}$ and mass $M_{\rm CGM}$. It determines how radiative losses reduce $E_{\rm CGM}$, allowing some of the accreted gas to enter the central galaxy. And it accounts for the central galaxy's feedback response to that gas supply, which adds both energy and gas mass to its atmosphere. 

An **ExpCGM** algorithm for multiphase gas must therefore focus on how a galactic atmosphere's inhomogeneity affects $\dot{E}\_{\rm CGM}$ and $\dot{M}\_{\rm CGM}$. For example, density contrasts can enhance radiative losses and raise the cooling-flow limit on the galaxy's gas supply. They can also alter how turbulent dissipation or shock fronts convert the atmosphere's kinetic energy into heat.

Different user-defined schemes for representing multiphase gas can be tested within the **ExpCGM** framework, to see how they affect $\dot{E}\_{\rm CGM}$ and $\dot{M}\_{\rm CGM}$ and alter the atmosphere's thermalization fraction $f_{\rm th}$. 

We hope that using **ExpCGM** to experiment with different multiphase gas models and to identify how the assumptions inherent in those models affect various observable properties of galactic atmospheres will yield useful constraints on the astrophysical processes determining those properties.

## Cool Gas Fraction

To represent multiphase gas, the **ExpCGM** framework defines an atmosphere's ***cool gas fraction*** $f_{\rm cool}$ to be the fraction of an atmosphere's gas mass that is "cool" compared to the atmosphere's gravitational temperature $T_\varphi$. The complementary "hot" gas fraction ($1-f_{\rm cool}$) has a pressure
 $$P = (1 - f_{\rm cool}) \frac {\bar{\rho} kT} {\mu m_p}$$
where $\bar{\rho}$ is the mean mass density of an atmosphere in which gas with $T \ll T_\varphi$ occupies a negligible fraction of the volume. 

Applying the usual **ExpCGM** assumption of force balance then gives
  $$\frac {P} {f_{\rm th}} = \frac {\bar{\rho} f_\varphi v_{\rm c}^2} {\alpha_{\rm eff}}$$
for a multiphase atmosphere, implying that 
  $$f_{\rm th} = (1 - f_{\rm cool}) \left( \frac {\alpha_{\rm eff}} {2 f_\varphi} \right) \frac {T} {T_\varphi}$$
According to this last equation, an atmosphere's thermalization fraction $f_{\rm th}$ is similar to the fraction $(1 - f_{\rm cool})$ of the atmospheric gas mass that remains hot, because the $\alpha_{\rm eff} / 2 f_\varphi$ factor is generally of order unity and the temperature $T$ of the hot phase is always similar to $T_\varphi$, because of how the hot phase is defined. 

Including multiphase gas within **ExpCGM** models therefore necessitates a reinterpretation of the $f_{\rm th}$ parameter. For example, $f_{\rm th}$ in a homogenous **ExpCGM** model with $t_{\rm cool} \ll f_{\rm th} t_{\rm diss}$ declines in proportion to $T$. Meanwhile, the velocity dispersion $\sigma_{\rm 1D}^2$ of isotropic turbulence increases. A decline in $f_{\rm th}$ is linked to a complementary rise in $\sigma_{\rm 1D}^2$ because the force balance condition ensures that
  $$\sigma_{\rm 1D}^2 = \left( 1 - f_{\rm th} \right) \frac {f_\varphi v_{\rm c}^2} {\alpha_{\rm eff}}$$
However, the equivalent condition in a multiphase **ExpCGM** model is
  $$f_{\rm cool}\sigma_{\rm 1D,cool}^2 + \left( 1 - f_{\rm cool} \right) \sigma_{\rm 1D,hot}^2 = \left( 1 - f_{\rm th} \right) \frac {f_\varphi v_{\rm c}^2} {\alpha_{\rm eff}}$$
in which $\sigma_{\rm 1D, hot}$ and $\sigma_{\rm 1D, cool}$ are the velocity dispersions of the hot and cool components, respectively. In that case, a drop in $f_{\rm th}$ corresponds to a rise in $f_{\rm cool}$ and a complementary decline in the hot gas fraction $(1 - f_{\rm cool})$ rather than a decline in $T$.

## Velocity Coupling

The presence of two velocity dispersions in the force balance constraint for a multiphase atmosphere raises an interesting question: How are those two velocity dispersions related? 

### Strong Coupling

If the motions of the cool component are strongly coupled to the motions of the hot component, meaning that $\sigma_{\rm 1D,hot} \approx \sigma_{\rm 1D,cool}$, then the force balance condition for a multiphase atmosphere becomes essentially identical to the one for a homogeneous atmosphere. The only conceptual difference is the interpretation of the $f_{\rm th}$ parameter. 

{: .note}
A multiphase atmosphere in which discrete cool clouds have column densities much smaller than the overall column density of the hot phase will be close to the strong coupling limit, because drag on a cold cloud moving through the hot medium then reduces its speed relative to the hot phase on a timescale that is short compared to the cloud's orbital timescale ($\sim r / v_{\rm c}$).

### Weak Coupling

The motions of cool clouds in an atmosphere jointly supported by turbulence and thermal pressure are essentially ballistic if hydrodynamical drag cannot couple them to the hot component on an orbital timescale. Each temperature component in an equilibrium **ExpCGM** atmosphere must then separately satisfy its own force balance condition. 

The force balance condition for the cool component becomes
  $$\sigma_{\rm 1D, cool}^2 = \frac {f_\varphi} {\alpha_{\rm eff}} v_{\rm c}^2$$
Force balance in the hot component independently implies a temperature
  $$T = \left( \frac {f_{\rm th}} {1 - f_{\rm cool}} \right) \left( \frac {2 f_\varphi} {\alpha_{\rm eff}} \right) T_\varphi$$
and a velocity dispersion
  $$\sigma_{\rm 1D, hot}^2 = \left( 1 - \frac {f_{\rm th}} {1 - f_{\rm cool}} \right) \frac {f_\varphi} {\alpha_{\rm eff}} v_{\rm c}^2$$
The ratio $f_{\rm th} / (1 - f_{\rm cool})$ determines the breakdown between thermal and turbulent support of the hot medium and needs to be of order unity to satisfy the stipulation $T \sim T_\varphi$.

{: .note}
The force balance condition given above for cool gas assumes an isotropic velocity dispersion, which might not be a good approximation for a population of ballistic clouds. 

### Sedimentation

Sedimentation occurs in between the strong-coupling and weak-coupling limits. A small amount of drag gradually drains orbital energy from clouds that would otherwise be ballistic, causing them to sink toward the center. That radial drift speed can be a model parameter, or it can be estimated based on a user-defined model for cool-cloud properties. 

{: .note}
A future Cloud Survival document can consider the conditions under which drifting clouds persist.

### Dissipation of Turbulent Support

Reduction of a multiphase atmosphere's turbulent support is easiest to model in the strong-coupling limit, because dissipation of turbulence in the hot medium also reduces the velocity dispersion of the cool clouds. 

Dissipation of kinetic energy in the weak-coupling limit is more model dependent. One approach to modeling it is to estimate the rate of inelastic cloud-cloud collisions in a population of cool ballistic clouds. Some of the kinetic energy of two colliding clouds initially dissipates into heat, presumably in shock fronts, and can contribute to $\dot{E}\_{\rm rad}$. 

What happens next depends on how quickly such collisions turn heat into radiative energy: 

* Rapid cooling converts the majority of the dissipated kinetic energy into photons, thereby reducing $E_{\rm CGM}$. 

* Slow cooling may transfer some of the heated gas from the atmosphere's cool component and to the hot component.

The details of how to model cloud-cloud collisions in a multiphase **ExpCGM** model atmosphere are currently left to the user.

## Mass Exchange

Conversion of hot gas to cool gas and back again can happen through many different channels in a multiphase galactic atmosphere. The figure below schematically represents at least some of those channels. An **ExpCGM** atmospheric model cannot represent all of them.  

[FIGURE GOES HERE]

The key questions for **ExpCGM** modeling are therefore: 

1. Which channels dominate the atmosphere's overall radiative energy loss rate ($\dot{E}\_{\rm rad}$)?
2. Which channels feed the most gas into the central galaxy and therefore determine the rate ($\dot{E}\_{\rm fb}$) at which feedback adds energy to the atmosphere?

{: .note}
In the figure, TRML stands for *turbulent radiative mixing layers*. 

### Condensation

Consider first the timescale on which cooling converts hot gas into cool gas in atmosphere with $t_{\rm cool} \ll f_{\rm th} t_{\rm diss}$. The cooling time of hot gas with a homogeneous mass density $\rho$ is
  $$t_{\rm cool} = \left( \frac {3 kT} {2 \mu m_p} \right) \frac {1} {\rho \Lambda_\rho}$$
in which $\rho \Lambda_{\rho}$ is the radiative energy loss rate per particle. W

In a multiphase atmosphere with mean gas-mass density $\bar{\rho}$, the hot-phase cooling time becomes
  $$t_{\rm cool} = \left( \frac {3 kT} {2 \mu m_p} \right) \frac {1} { (1 - f_{\rm cool}) \bar{\rho} \Lambda_\rho}$$
Mass transfer from the hot phase to the cool phase therefore causes the hot-gas cooling time rise and slows this particular channel for mass exchange, which asymptotically abates as $t_{\rm cool}$ approaches the universe's age.

{: .note}
The abatement of condensation in a multiphase galactic atmosphere was notably emphasized by Maller & Bullock (2004). It places an upper limit on the value of $f_{\rm cool}$ acheivable through condensation and ensures that at least some volume-filling hot gas remains in a condensing atmosphere.

In the **ExpCGM** framework, mass exchange through this channel can come into equilibrium before the cooling time exceeds the universe's age. The atmosphere's overall thermal support fraction evolves according to 
  $$\frac {d f_{\rm th}} {dt} = \frac {1 - f_{\rm th}} {t_{\rm diss}} - \frac {(1 - f_{\rm th}) f_{\rm th}} {t_{\rm cool}} + \frac {(f_{\rm in,th} - f_{\rm th})} {t_{\rm in}}$$
(see [Essentials](Essentials) for a derivation of this equation and the definitions of its parameters). Cooling transfers gas from the hot phase to the cool phase until 

<p>
  $$f_{\rm th} = \frac {t_{\rm cool}} {t_{\rm diss}}\left[ 1 +  \left( \frac {f_{\rm in,th} - f_{\rm th}}{1 - f_{\rm th}} \right)\frac {t_{\rm diss}} {t_{\rm in}} \right]
</p>


because the thermal support fraction $f_{\rm th}$ then stabilizes on a timescale $\sim t_{\rm diss}$. The hot component in this state of balance radiates thermal energy at a rate that matches the total thermal energy input entering teh atmosphere both directly, from sources of thermal energy, and indirectly, through dissipation of kinetic energy input. 

### Turbulent Heating

How to model an atmosphere with $t_{\rm cool} \gg f_{\rm th} t_{\rm diss}$ is less clear. According to the thermalization equation, the thermal support fraction $f_{\rm th}$ should rise, because dissipation is generating thermal energy faster than radiative losses can shed it. If dissipation is indeed reducing $f_{\rm cool}$ in proportion to $1 - f_{\rm th}$, then the hot-gas mass fraction of a multiphase galactic atmosphere evolves toward 
  $$1 - f_{\rm cool}  = \left( \frac {2 f_\varphi} {\alpha_{\rm eff}} \frac {T_\varphi} {T} \right) \left[ 1 + \left( \frac {f_{\rm in,th} - f_{\rm th}} {1 - f_{\rm th}} \right)\frac {t_{\rm diss}} {t_{\rm in}} \right]\frac {t_{\rm cool}} {t_{\rm diss}}$$
on a timescale $\sim t_{\rm cool}$. In that case, multiphase galactic atmospheres in the **ExpCGM** framework converge toward a hot-gas mass fraction $(1 - f_{\rm cool}) \sim t_{\rm cool} / t_{\rm diss}$.

However, dissipation that exceeds radiative cooling does not necessarily reduce $f_{\rm cool}$. Instead, it can expand the volume of the hot component without changing the mass fraction in the cool component. Wherever that circumstance arises, $f_{\rm cool}$ and $f_{\rm th}$ need to be treated separately. Defining a thermal support fraction
  $$f_{\rm th, hot}  \equiv \frac {f_{\rm th}} { 1 - f_{\rm cool}} = \frac {kT} {kT + \mu m_p \sigma_{\rm 1D,hot}^2}$$
for just the hot gas leads to the evolution equation

<p>
  $$\frac {d f_{\rm th, hot}} {dt} = \frac {\dot{f}_{\rm th}  - f_{\rm th,hot} \dot{f}\_{\rm cool}}{ 1 - f_{\rm cool}}$$
</p>


The cool gas fraction $f_{\rm cool}$ then evolves according to some combination of the processes schematically pictured in the figure above. 

### Future Considerations

Given the complexity of the overall mass-exchange network, future development of the **ExpCGM** framework will need to focus separately on those channels for changing $f_{\rm cool}$. But before concluding this document, we will take its line of reasoning one step further and consider what follows from assuming $t_{\rm diss} \sim r / \sigma_{\rm 1D}$. 

The expected hot-gas mass fraction of a multiphase galactic atmosphere near equilibrium is then
  $$1 - f_{\rm cool}  \sim \left( \frac {\sigma_{\rm 1D}}  {v_{\rm c}} \right) \frac {t_{\rm cool}} {t_{\rm ff}}$$
where $t_{\rm ff} \equiv (2 r / g)^{1/2} = 2^{1/2} (r/ v_{\rm c})$ is the freefall time at radius $r$ in a potential well with local gravitational acceration $g$. If the assumptions leading to equation are valid, then the gas mass of a galactic atmosphere with $t_{\rm cool} \ll t_{\rm ff}$ should be dominated by the cool component, because $\sigma_{\rm 1D} \gg v_{\rm c}$ is unphysical in the context of an equilibrium **ExpCGM** model. Conversely, an atmosphere with $t_{\rm cool} \gg t_{\rm ff}$ should consist primarily of hot gas, unless its supply of cold gas comes from sources other than radiative cooling of the hot atmosphere. 

These inferences suggest an observational test of the overall scenario presented on this page. If the masses of both atmospheric temperature components can be measured, along with $\sigma_{\rm 1D}$, $v_{\rm c}$, and the $t_{\rm cool}/t_{\rm ff}$ ratio, then the scaling-relation prediction in the equation above can be checked. Large disagreements, particularly a small hot-gas fraction in an atmosphere with $t_{\rm cool} / t_{\rm ff} \gg 1$ and $\sigma_{\rm 1D} \sim v_{\rm c}$, would indicate either that dissipation of kinetic energy does not elevate cool gas into the hot component or that abundant cold gas in a kinetically supported atmosphere with a long cooling time is out of equilibrium.
