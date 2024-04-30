---
title: Pressure Profiles
layout: default
nav_order: 5
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

# Pressure Profiles
{: .no_toc}

All galactic atmosphere models in the **ExpCGM** framework depend on a user-specified shape function $\alpha (r) \equiv - d \ln P / d \ln r$ for the thermal pressure profile. The framework then determines how the normalization of that pressure profile responds to changes in atmospheric mass and energy. Users therefore need to consider the physical conditions they are intending to model when specifying $\alpha (r)$.

This page is an **ExpCGM** user's guide to choosing a shape function. Its first section discusses the simplest choices, which are constant values of $\alpha$. Its second section presents a shape-function formula for purely cosmological atmospheres. The section that follows introduces entropy-based methods for specifying $\alpha (r)$, and the next one derives the characteristic shape functions for cooling flows, isentropic cores, and precipitation-limited atmospheres. The concluding section discusses how the **ExpCGM** framework can be extended to handle an evolving pressure-profile shape function.

<details closed markdown="block">
  <summary>
    Contents
  </summary>
   {: .text-delta}
- TOC
{:toc}  
</details>

## Power Laws

The simplest **ExpCGM** pressure profiles are pure power laws, with
$$P(r) = P_0 \left( \frac {r} {r_0} \right)^{-\alpha}$$
However, not all values of $\alpha$ are physically justifiable. For example, an atmosphere with $\alpha = 3/2$ everywhere is only marginally bound to its halo (see the [Confinement](Confinement) page for an explanation). Galactic atmospheres that have $\alpha < 3/2$ at all radii must therefore be confined by external pressure forces.

{: .note}
For many applications, a power-law pressure profile with constant $\alpha$ might be all that an **ExpCGM** user needs.

## Cosmological Profiles

Cosmological structure formation produces pressure profiles in which $\alpha (r)$ increases with radius. Both observations and numerical simulations of galaxy clusters show that their pressure profiles are flatter than $\alpha = 3/2$ at small radii and steeper than $\alpha = 3/2$ at large radii. As a result, the cosmological pressure profiles of galaxy clusters are often represented with a fitting formula equivalent to the shape function
  $$\alpha (r) = \alpha_{\rm in} + \left( \alpha_{\rm out} - \alpha_{\rm in} \right) \left[ \frac {(r / r_\alpha)^{\alpha_{\rm tr}}} { 1 +(r / r_\alpha)^{\alpha_{\rm tr}}} \right]$$
The parameter $r_\alpha$ is a transitional radius for the profile's slope that is approximately twice the scale radius $r_{\rm s}$ of an NFW fit to the halo's gravitational potential. The other parameters represent the pressure profile's power-law slope in the appropriate limits: $\alpha_{\rm in}$ is the inner slope, $\alpha_{\rm out}$ is the outer slope, and $\alpha_{\rm tr}$ describes the sharpness of the transition near $r_\alpha$ from inner slope to outer slope. 

Fits to observations of thermal pressure profiles in galaxy clusters give an inner slope $\alpha_{\rm in} \approx 0.3$, an outer slope $\alpha_{\rm out} \approx 4.3$, and $\alpha_{\rm tr} \approx 1.1$, with greater dispersion in $\alpha_{\rm in}$ than in $\alpha_{\rm out}$. 

{: .note}
This shape function produces what is sometimes called a *generalized NFW profile*. Integrating it after defining $x \equiv r / r_\alpha$ gives a pressure profile with the form
  $$P(r) \propto x^{-\alpha_{\rm in}} (1 + x^{\alpha_{\rm tr}})^{-(\alpha_{\rm out} - \alpha_{\rm in})/\alpha_{\rm tr}}$$
Choosing $\alpha_{\rm in} = 1$, $\alpha_{\rm out} = 3$, and $\alpha_{\rm tr} = 1$ then makes this pressure profile like an NFW matter density profile, which has the form $\rho(r) \propto x^{-1} (1 + x)^{-2}$.


## Entropy-Based Profiles

A more sophisticated approach to specifying atmospheric pressure profiles focuses on the normalization factor $K$ of the polytropic equation of state $P = K \rho^\gamma$ relating $P$ to $\rho$. The quantity $K$ is sometimes called the atmosphere's "entropy" because changes in $\ln K$ are directly proportional to changes in specific entropy (see the [Cooling](Cooling) page). 

Entropy-based methods separate a pressure profile's shape function into two terms
  $$\alpha (r) = \frac {3} {2} \frac {d \ln K} {d \ln r} - \frac {5} {2} \frac {d \ln T} {d \ln r}$$
representing how an atmosphere's entropy and temperature gradients combine to produce the shape function for thermal pressure in an atmosphere with $\gamma = 5/3$, in which $P \propto T^{5/2} K^{-3/2}$.

### Nearly Isothermal Regions

Decomposition of the pressure shape function into an entropy term and a temperature term is particularly useful if the entropy profile's shape function
  $$\alpha_K (r) \equiv \frac {d \ln K} {d \ln r}$$
is approximately constant, as is often the case. Both cosmological simulations and observations of galaxy clusters are consistent with $\alpha_K \approx 1.1$ outside a cluster's central regions. A cosmological galactic atmosphere should therefore have $\alpha \approx 1.7$ in regions that are nearly isothermal. However, both deviations from isothermality and non-cosmological changes in $\alpha_K$ can result in deviations from $\alpha \approx 1.7$. 

### Outer Temperature Decline

At large radii a cosmological atmosphere's thermal pressure profile has $\alpha > 1.7$ because of a radial decline in atmospheric temperature. An atmosphere's equilibrium temperature in the **ExpCGM** framework follows from the force balance equation
$$\frac {d} {dr} \frac {P} {f_{\rm th}} = - \rho f_\varphi \frac {v_{\rm c}^2} {r}$$
in which $f_{\rm th}$ is the fraction of total support that thermal pressure $P$ provides and $f_\varphi$ accounts for modifications to purely gravitational compression (see the [Essentials](Essentials) page). That equilibrium temperature profile is
  $$T(r) = \left( \frac {\mu m_p} {k} \right)
                \frac {f_{\rm th} f_\varphi v_{\rm c}^2} 
                        {\alpha_{\rm eff}}$$
in which $\alpha_{\rm eff} = \alpha + (d \ln f_{\rm th} / d \ln r)$. 

Three factors on the right-hand side of the equation for $T(r)$ contribute to the temperature decline: 

* **Declining Circular Velocity.** The circular velocity $v_{\rm c}$ of a cosmological halo declines with radius in its outer parts, and so the atmosphere's equilibrium gas temperature $T$ decreases in proportion to $v_{\rm c}^2$.

* **Declining Thermalization.** Everything that happens in a galaxy's atmosphere happens more slowly at larger radii than at smaller radii because of the longer dynamical time ($\sim r / v_{\rm c}$) at larger radii. That includes thermalization of the kinetic energy of accreting gas. Consequently, the fraction $f_{\rm th}$ of atmospheric support provided by thermal pressure declines at large radii, enhancing the decline in $T(r)$ that stems from a decline in $v_{\rm c}^2(r)$.

* **Declining Momentum Confinement.** As accreting gas slows down and becomes incorporated into a galaxy's atmosphere, it transfers its inward momentum to the atmosphere and assists gravitational confinement, so that $f_\varphi > 1$ (see the [Accretion](Accretion) page). In the deceleration zone where momentum transfer occurs, the extra confinement can boost $T(r)$, perhaps partially offsetting the decline in $v_c^2 (r)$ with radius. However, $f_\varphi$ decreases in the outer parts of the deceleration zone, where the extra confinement lessens, making the decline in $T(r)$ is greater than it would otherwise be.

### Simplified Cosmological Profile

A simple entropy-based method that automatically reproduces a temperature decline at large radii is to use the shape function approximation
  $$\alpha (r) \approx {1.7} \left( \frac {2 r / r_{\rm max}} {1 + r/r_{\rm max}} \right)$$
to represent a cosmological atmosphere. It is designed to have $\alpha \approx 1.7$ near the radius $r_{\rm max} = 2.163 r_{\rm s}$, where $v_{\rm c}^2 (r)$ peaks in an NFW gravitational potential with scale radius $r_{\rm s}$. Coupling that approximation with a power-law entropy profile having $\alpha_K \approx 1.1$ ensures a nearly isothermal temperature profile in the vicinity of $r_{\rm max}$, where the gravitational potential is nearly isothermal. It also produces a temperature decline resembling those observed at larger radii, where $v_{\rm c}$ declines. Furthermore, the asymptotic values of $\alpha(r)$ at large and small radii are similar to the values measured from galaxy-cluster observations.

## Entropy Modification

Entropy-based methods for determining $\alpha (r)$ are particularly useful for characterizing non-cosmological processes, such as radiative cooling and feedback energy input from the central galaxy, because those energy sinks and sources modify the entropy profile that would otherwise result from cosmological structure formation. 

The **ExpCGM** framework includes three physically justifiable non-cosmological modifications of $\alpha_K$ that users may wish to implement:

### Cooling Flow Profile 

The central regions of a galactic atmosphere generally have a radiative cooling time $t_{\rm cool}$ that is less than the universe's age. If heat input from the central galaxy falls short of replacing the energy lost to radiation, the central gas loses entropy and sinks into the central galaxy on a timescale $\sim t_{\rm cool}$. If that flow of gas is homogeneous, it is called a *cooling flow*. Its characteristic entropy gradient follows from the relationship
  $$\alpha_K = \frac {d \ln K} {d \ln r} = - t_{\rm flow} \frac {d \ln K} {d t} = \frac {t_{\rm flow}} {t_{\rm cool}}$$
in which $t_{\rm flow} \equiv r / v_{\rm in}$ is the inflow timescale corresponding to the inflow speed $v_{\rm in} = - dr/dt$ of the cooling flow (see the [Cooling](Cooling) page for a more complete explanation). 

Gravitational compression in a potential well that is nearly isothermal keeps the temperature of homogeneous radiating gas nearly constant as it flows inward. The gas is "cooling" because its specific entropy is declining, even though its temperature is not changing very much. The cooling flow carries gas inward at the rate

<p>
  $$\dot{M}_{\rm cool}
            = 4 \pi r^2 \rho v_{\rm in}
            = \frac {4 \pi r^3 \rho} {t_{\rm flow}}
            = \frac {4 \pi r^3 \rho} {\alpha_K t_{\rm cool}}$$
</p>


A steady cooling flow, in which $\dot{M}\_{\rm cool}$ does not depend on radius, therefore has $\rho / t_{\rm cool} \propto r^3$. In a nearly isothermal potential well that maintains a nearly constant atmospheric temperature, one then finds $t_{\rm cool} \propto \rho^{-1}$, implying that $P (r) \propto \rho (r) \propto r^{-3/2}$ and $\alpha \approx 3/2$ in a steady cooling flow.

{: .note}
The shape function approximation $\alpha_{\rm cool} \approx 1.5$ for a steady and nearly isothermal cooling flow has a numerical value similar to the power-law index $\alpha \approx 1.7$ describing the nearly isothermal parts of a cosmological atmosphere, but it has a completely different physical origin. In an **ExpCGM** atmospheric model, using $\alpha_{\rm cool} \approx 1.5$ is justified at radii where $t_{\rm cool}$ is much shorter than the universe's age and within which radiative cooling greatly exceeds feedback energy input. However, the cooling flow is likely to become inhomogeneous where $t_{\rm cool}$ drops below the atmosphere's dynamical time ($\sim r / v_{\rm c}$), calling for a different approach to specifying $\alpha$ at those radii.

### Isentropic Core

At the other extreme from a pure cooling flow is centralized heat input that greatly exceeds radiative cooling. Under those conditions, heating raises the atmosphere's central entropy level, causing convection wherever the radial entropy gradient becomes negative. Convection then maintains $\alpha_K$ near zero, leading to development of an isentropic core with a constant entropy level. 

#### Isentropic Shape Function

Isentropic gas with the equation of state $P = K \rho^{5/3}$ has $T \propto P^{2/5}$. The temperature profile of a hydrostatic isentropic core therefore satisfies

<p>
  $$\frac {dT} {d \ln r} ~=~ - \frac {2} {5} \frac {\mu m_p v_{\rm c}^2 (r)} {k} ~=~ - \frac {4} {5} T_\varphi (r)$$ 
</p>


in which $T_\varphi (r)$ is the gravitational temperature profile defined on the [Essentials](Essentials) page. Integrating this equation gives
$$T(r) = T_{\rm core} + \frac {4} {5}\int_r^{r_{\rm core}} T_\varphi (r) \frac {dr} {r}$$
The ***core radius*** parameter $r_{\rm core}$ is the outer radius of the isentropic region, and $T_{\rm core} = T(r_{\rm core})$ is the atmospheric temperature at that radius. 

Inserting $T(r)$ into the equation $\alpha (r) = 2 T_\varphi (r) / T(r)$
then gives the pressure profile's shape function at $r < r_{\rm core}$. It reduces to
  $$\alpha (r) \approx \frac {2 T_\varphi (r)} {T_{\rm core} + 0.8 T_\varphi (r) \cdot \ln (r_{\rm core}/r) }$$
in a potential well that is approximately isothermal.

#### Central Heating

The core radius of the isentropic region depends on how the amount $\Delta Q$ of central heating compares with the atmosphere's cumulative thermal energy profile preceding the heating event. Heating of gas at temperature $T$ raises its entropy $S$ by

<p>
  $$\Delta \ln K ~=~ \frac {2} {3k} \left( \frac {\mu m_p} {M_{\rm gas}} \right) \Delta S  ~=~ \frac {2} {3} \left( \frac {\mu m_p} {M_{\rm gas}} \right) \frac {\Delta Q} {k T}$$
</p>


where $M_{\rm gas}$ is the mass of heated gas, and the numerical factor 2/3 pertains to a gas with a thermal energy density of $3P/2$ (see the [Cooling](Cooling) page). Centralized thermalization of the energy $\Delta Q$ therefore produces an isentropic core out to a radius $r_{\rm core}$ containing a gas mass
  $$M_{\rm gas} \sim \frac {2} {3} \left( \frac {\mu m_p} {k T_\varphi} \right) \Delta Q$$
In other words, the isentropic region consists of gas that had an original thermal energy content ($\sim 3 k T_\varphi /2$ per particle) comparable to the energy input $\Delta Q$.

#### Core Radius Approximation

To estimate $r_{\rm core}$, consider an atmosphere with a pressure profile that is initially a pure power law: $P = P_0 (r / r_0)^{-\alpha_0}$. The cumulative thermal energy profile of that atmosphere is
  $$E_{\rm th} (r) = 4 \pi r_0^3 P_0 \int_0^{r/r_0} x^{2 - \alpha_0} dx
                   = \frac {4 \pi r_0^3 P_0} {3 - \alpha_0} 
                        \left( \frac {r} {r_0} \right)^{3 - \alpha_0}$$
The relationship $E_{\rm th} (r_{\rm core}) \approx \Delta Q$ then implies
  $$r_{\rm core} \approx \: r_0 \times \left[ \frac {( 3 - \alpha_0) \Delta Q} {4 \pi r_0^3 P_0} \right]^{1/ (3 - \alpha_0)}$$
and the new shape function at $r < r_{\rm core}$ after the central heating event is
  $$\alpha (r) \approx \frac {\alpha_0} { 1 + 0.4 \, \alpha_0 \,  \ln (r_{\rm core}/r) }$$
Similar estimates can be made for more complicated initial pressure profiles.

{: .note}
A caveat: Centralized heating of an atmosphere that is not spherically symmetric does not necessarily produce an isentropic core, especially if the heated gas escapes along low-density channels with a collective solid angle substantially less than $4 \pi$. For example, a rotating cooling flow naturally forms a cold, high-density disk inside of where its rotation speed approaches the potential's circular velocity (see the [Rotation](Rotation) page).  Heat input near the center of that disk tends to drive a bipolar outflow along the disk's rotation axis without significantly affecting the disk. It can therefore drive atmospheric circulation, in which the gas inflow through the rotating cooling flow roughly balances the bipolar gas outflow driven by central heating, when averaged over long time periods.

### Precipitation Limited Profiles

Observations of galaxy groups and clusters with a short central cooling time ($t_{\rm cool} \lesssim 1 ~{\rm Gyr}$) show that the shape functions of their central pressure profiles ($r \lesssim 20~{\rm kpc}$) are in between expectations for a pure cooling flow ($\alpha \approx 3/2$) and centralized heating ($\alpha < 1$). Neither heating nor cooling dominates. Instead, interplay between heating and cooling appears to be suspending the atmosphere in a quasisteady state.

One observed characteristic of that seemingly quasisteady atmospheric state is a constant ratio of cooling time to dynamical time, as represented by a freefall time $t_{\rm ff} \equiv (2 r / g)^{1/2}$ based on the local gravitational acceleration $g$. In a nearly isothermal potential (with $t_{\rm ff} \propto r$), a constant $t_{\rm cool}/t_{\rm ff}$ ratio implies $\rho \propto t_{\rm cool}^{-1} \propto r^{-1}$, because the atmosphere's temperature remains approximately constant with radius. The shape function for an atmosphere in that state is $\alpha \approx 1$.

Numerical simulations suggest that the configuration of such an atmosphere is shaped by susceptibilty to condensation. If $t_{\rm cool}/t_{\rm ff} < 1$, then atmospheric gas can cool and condense faster than it can fall to the center of the potential well. It is therefore prone to forming a *multiphase medium* consisting of volume-filling gas (at temperature $T \sim T_\varphi$) containing much cooler embedded clouds (with $T \ll T_\varphi$) that can be orders of magnitude denser (for a more extensive description, see the [Multiphase Gas](MultiphaseGas) page). 

Observations show that $\min (t_{\rm cool}/t_{\rm ff}) \sim 10$ is a typical lower limit for the ratio of cooling time to freefall time in galaxy clusters and groups. A plausible explanation for this minimum ratio is that "precipitation" of dense clouds out of the ambient medium may be exponentially sensitive to $t_{\rm cool}/t_{\rm ff}$ in the range $3 \lesssim t_{\rm cool}/t_{\rm ff} \lesssim 20$. According to that hypothesis, an atmosphere at the low end of the range is overly prone to formation of cold clouds that rain down upon the central galaxy and fuel energetic feedback that expands the atmosphere and raises the value of $t_{\rm cool}$. The **ExpCGM** framework therefore calls pressure profiles with $\alpha \approx 1$ and $t_{\rm cool}/t_{\rm ff} \sim 10$ at small radii ***precipitation limited.*** 

There are at least two options for imposing a precipitation limit on a cosmological entropy profile to obtain a precipitation-limited shape function:

* **Linked Normalization.** The simpler method forces a shape function that is cosmological at large radii to morph into a pressure profile with an approximately constant $t_{\rm cool} / t_{\rm ff}$ ratio at small radii. That can be done by setting $\alpha_{\rm in} = 1$ in the formula for a cosmological shape function or by modifying the formula's simplified version, so that 
  $$\alpha (r) \approx 1 + 0.7 \left( \frac {2 r / r_{\rm max}} { 1 + r / r_{\rm max} } \right)$$
If this method is used, increases in $E_{\rm CGM}$ change the pressure profile's normalization factor $P_0$ without changing its shape function, meaning that the pressure normalization of the inner region, which may be limited by precipitation, is *linked* to the normalization of the outer region, where the shape function's slope is cosmological.  The method implicitly assumes that feedback energy input is evenly spread over the entire atmosphere, so that $K(r)$ rises by the same factor at all radii.

* **Unlinked Normalization.** A more complex method represents a halo's precipitation-limited entropy profile with two power laws. The first is a cosmological entropy profile with 
  $$K_{\rm c} (r) = K_{\rm c,0} \left( \frac {r} {r_0} \right)^{1.1}$$
in which the normalization factor $K_{\rm c,0}$ comes from cosmological numerical simulations.
The second expresses the precipitation limit in terms of
  $$K_{\rm p} (r) = K_{\rm p,0} \left( \frac {r} {r_0} \right)^{2/3}$$
in which the normalization factor $K_{\rm p,0}$ corresponds to a limiting $t_{\rm cool}/t_{\rm ff}$ ratio. The two normalization factors are *unlinked* in the sense that feedback energy input from the central galaxy raises $K_{\rm p}$ but not $K_{\rm c}$. Summing the cosmological and precipitation limited entropy profiles then gives the combined entropy profile
  $$K(r) = K_{\rm p,0} \left( \frac {r} {r_0} \right)^{2/3} + K_{\rm c,0} \left( \frac {r} {r_0} \right)^{1.1}$$
with the entropy shape function
  $$\alpha_K(x) = \frac {2} {3} \left( \frac {1} {1+y} \right) + \frac {11} {10} \left( \frac {y} {1+y} \right)$$
in which $y = (K_{\rm c,0} / K_{\rm p,0}) (r / r_0)^{13/30}$. In an atmosphere with a negligible temperature gradient, this expression for $\alpha_K$ gives the approximation
  $$\alpha (r) \approx \frac {1 + 1.7 y} {1+y}$$ 
for the pressure shape function. Users who do not want to neglect the temperature gradient can implement the approximation
  $$\alpha (r) \approx \frac {1} {1+y} + 1.7 \left( \frac {2 r / r_{\rm max}} 
                                { 1 + r / r_{\rm max}} \right)
                                \frac {y} {1+y}$$
which reduces to the *Simplified Cosmological Profile* above in the limit $K_{\rm c,0} \gg K_{\rm p,0}$.

{: .note}
A potential complication with the *Unlinked Normalization* method is that $K_{\rm p,0}$ is temperature-dependent, and the equilibrium value of $T$ is determined by the shape function $\alpha$ that one wants to calculate. That complication can be mitigated if $T$ depends only weakly on $r$, in which case $t_{\rm cool} \propto \rho^{-1}$ implies $\alpha \approx 1$ and $kT \approx \mu m_p v_{\rm c}^2$. However, an iterative solution for $K_{\rm p,0}$ may be necessary if the atmosphere's temperature gradient is significant.

## Evolving the Shape Function

The primary motivation for having multiple options for $\alpha (r)$ is that the spatial distribution of feedback energy input within a galaxy's atmosphere remains unknown. Comparisons of the different options with both observations and numerical simulations will eventually show which options most faithfully represent various modes of feedback energy input. Also, an atmosphere's shape function may evolve with time as feedback modes and the radial profile of energy input change.

In principle, changes in $\alpha (r)$ during a time interval $\Delta t$ can be managed in two distinct steps, but that approach has not yet been implemented in **ExpCGM**. The first step computes the atmosphere's mass change $\Delta M_{\rm CGM}$ and total energy change $\Delta E_{\rm CGM}$, based on the atmosphere's current shape function. The second step determines the atmosphere's structure based on the new values of $M_{\rm CGM}$ and $E_{\rm CGM}$ and also an updated shape function that satisfies user-defined constraints imposed upon it.

For example, consider a time interval that starts with a power-law shape function and ends with an isentropic core resulting from an increment $\Delta E_{\rm fb}$ of centralized feedback energy. Radiative losses determined from the power-law configuration reduce $E_{\rm CGM}$ by $\dot{E}\_{\rm rad} \cdot \Delta t$ during the time interval, while feedback and cosmological accretion boost $E_{\rm CGM}$ by $\Delta E_{\rm fb}$ and $\Delta E_{\rm acc}$, respectively. Those processes also add gas mass increments $\Delta M_{\rm fb}$ and $\Delta M_{\rm acc}$ to $M_{\rm CGM}$. Meanwhile, radiative cooling removes $\dot{M}\_{\rm cool} \cdot \Delta t$. That concludes the first step.

The second step proceeds with a calculation of $r_{\rm core}$, based on the initial power-law configuration, giving
  $$r_{\rm core} \approx r_0 \times \left[ \frac {( 3 - \alpha_0) \Delta E_{\rm fb}} {4 \pi r_0^3 P_0} \right]^{1/ (3 - \alpha_0)}$$
With that value of $r_{\rm core}$, a new shape function for the atmosphere can be calculated, based on the *Isentropic Core* model outlined above. The usual **ExpCGM** method can then be used to determine the atmosphere's new configuration, based on the new shape function, the updated values of $E_{\rm CGM}$ and $M_{\rm CGM}$, and possibly also updated values of $v_\varphi^2$ and $r_{\rm s}$. 

{: .note}
These two steps can be generalized to accommodate other kinds of changes in $\alpha (r)$. However, restrictions on $\Delta t$ may be necessary to ensure that the resulting changes in $\alpha (r)$ are at least qualitatively consistent with the physical rationale for those changes. Keep in mind that the overall aim of **ExpCGM** modeling is to explore how feedback from a halo's central galaxy both shapes its atmosphere and responds to the atmosphere's configuration. Therefore, one of its main goals is to test the community's *assumptions* about the relationship between feedback and the atmosphere's shape function (including the assumptions implicit in numerical simulations) and to assess those assumptions through comparisons of **ExpCGM** models with observations. 
