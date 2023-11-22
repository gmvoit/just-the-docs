---
title: Pressure Profiles
layout: default
nav_order: 5
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

# Pressure Profiles
{: .no_toc}

To construct an equilibrium atmospheric model in the **ExpCGM** framework, users must specify a shape function $\alpha (r) \equiv - d \ln P / d \ln r$ for the thermal pressure profile ... 

<details closed markdown="block">
  <summary>
    Table of contents
  </summary>
   {: .text-delta}
- TOC
{:toc}  
</details>

## Power Laws

The simplest **ExpCGM** pressure profiles are pure power laws, with
$$P(r) = P_0 \left( \frac {r} {r_0} \right)^{-\alpha}$$
However, not all values of $\alpha$ are physically justifiable. For example, an atmosphere with $\alpha = 3/2$ everywhere is only marginally bound to its halo (see the [Confinement](Confinement) page for an explanation). Atmospheres that have $\alpha < 3/2$ at all radii must therefore be confined by external pressure forces.

{: .note}
For many applications, a power-law pressure profile with constant $\alpha$ might be all that an **ExpCGM** user needs.

## Cosmological Profiles

Cosmological structure formation produces pressure profiles in which $\alpha (r)$ increases with radius. Both observations and numerical simulations of galaxy clusters show that their pressure profiles are flatter than $\alpha = 3/2$ at small radii and steeper than $\alpha = 3/2$ at large radii. As a result, the cosmological pressure profiles of galaxy clusters are often represented with a fitting formula equivalent to the shape function
$$\alpha (r) = \alpha_{\rm in} + \left( \alpha_{\rm out} - \alpha_{\rm in} \right) \left[ \frac {(r / r_{\rm tr})^{\alpha_{\rm tr}}} { 1 +(r / r_{\rm tr})^{\alpha_{\rm tr}}} \right]$$
The parameter $r_{\rm tr}$ is a transitional radius for the profile's slope that is approximately twice the scale radius $r_{\rm s}$ of an NFW fit to the halo's gravitational potential. The other parameters represent the pressure profile's power-law slope in the appropriate limits. Fits to observations of thermal pressure profiles in galaxy clusters give an inner slope $\alpha_{\rm in} \approx 0.3$, an outer slope $\alpha_{\rm out} \approx 4.3$, and $\alpha_{\rm tr} \approx 1.1$. 

{: .note}
Observations of galaxy clusters show a greater dispersion in $\alpha_{\rm in}$ than in $\alpha_{\rm out}$, presumably because the inner parts of a galaxy cluster's atmosphere respond more strongly to radiative cooling and outbursts of feedback energy from the central galaxy.

## Entropy-Based Profiles

Another approach to specifying cosmological pressure profiles, also useful for modifying them, focuses on the normalization factor $K$ of the polytropic equation of state $P = K \rho^\gamma$ relating $P$ to $\rho$. The quantity $K$ is sometimes called the atmosphere's "entropy" because changes in $\ln K$ are directly proportional to changes in specific entropy (see the [Cooling](Cooling) page). A pressure profile's shape function can therefore be separated into two terms
$$\alpha (r) = \frac {3} {2} \frac {d \ln K} {d \ln r} 
        - \frac {5} {2} \frac {d \ln T} {d \ln r}$$
representing how an atmosphere's entropy and temperature gradients combine to produce the shape function for thermal pressure in a $\gamma = 5/3$ atmosphere (which has $P \propto T^{5/2} K^{-3/2}$).

This decomposition into an entropy part and a temperature part is particularly useful if the entropy profile's shape function
$$\alpha_K (r) \equiv \frac {d \ln K} {d \ln r}$$
is approximately constant, as is often the case. Both cosmological simulations and observations of galaxy clusters are consistent with $\alpha_K \approx 1.1 - 1.2$ outside a cluster's central regions. A cosmological galactic atmosphere should therefore have $\alpha \approx 1.7$ in regions that are nearly isothermal. However, both deviations from isothermality and changes in $\alpha_K$ result in deviations from $\alpha \approx 1.7$. 

The steepening at large radii of a cosmological atmosphere's thermal pressure profile beyond $\alpha \approx 1.7$ results from a radial decline in atmospheric temperature. In the **ExpCGM** framework, an atmosphere's equilibrium temperature follows from the force balance equation
$$\frac {d} {dr} \frac {P} {f_{\rm th}} 
        = - \rho f_\varphi \frac {v_{\rm c}^2} {r}$$
in which $f_{\rm th}$ is the fraction of total support that thermal pressure $P$ provides and $f_\varphi$ accounts for modifications to purely gravitational compression. That equilibrium temperature profile is
$$T(r) = \left( \frac {\mu m_p} {k} \right)
                \frac {f_{\rm th} f_\varphi v_{\rm c}^2} 
                        {\alpha_{\rm eff}}$$
in which $\alpha_{\rm eff} = \alpha + (d \ln f_{\rm th} / d \ln r)$. 

Three of the non-constant factors on the right-hand side of equation (\ref{eq:TemperatureProfile}) can contribute to the decline in $T(r)$: 

* **Declining Circular Velocity.** The circular velocity of a cosmological halo declines with radius in its outer parts, and so the atmosphere's equilibrium gas temperature $T$ decreases in proportion to $v_{\rm c}^2$.

* **Declining Momentum Confinement.** As accreting gas slows down and becomes incorporated into a galaxy's atmosphere, it transfers its inward momentum to the atmosphere and assists gravitational confinement, so that $f_\varphi > 1$ (see the [Accretion](Accretion) page). In the deceleration zone where momentum transfer occurs, the extra confinement can boost $T(r)$, perhaps partially offsetting the decline in $v_c^2 (r)$ with radius. However, in the outer parts of the deceleration zone, where the extra confinement lessens (and $f_\varphi$ decreases), the decline in $T(r)$ is even greater than it would otherwise be.

* **Declining Thermalization.** Everything that happens in a galaxy's atmosphere happens more slowly at larger radii than at smaller radii because of the longer dynamical time ($\sim r / v_{\rm c}$) at larger radii. That includes thermalization of the kinetic energy of accreting gas. Consequently, the fraction $f_{\rm th}$ of atmospheric support provided by thermal pressure declines at large radii, enhancing the decline in $T(r)$ coming from $v_{\rm c}^2(r)$.

The fourth non-constant factor ($\alpha_{\rm eff}$) depends on changes in the other three and is less consequential.

In principle, users of the **ExpCGM** framework can specify $v_{\rm c}^2 (r)$, $f_{\rm th} (r)$, and $f_\varphi (r)$ and then derive $\alpha (r)$ from that information. However, that approach is needlessly complex, given the guidance that observations and simulations provide about $\alpha (r)$ in cosmological atmospheres. If the confining halo is represented by an NFW potential well with a scale radius $r_{\rm s}$, the simplest approach is to use the shape function approximation
$$\alpha (r) \approx {1.7} \left( \frac {2 r / r_{\rm max}} {1 + r/r_{\rm max}} \right)$$
to represent a cosmological atmosphere. It is designed to have $\alpha \approx 1.7$ near the radius $r_{\rm max} = 2.163 r_{\rm s}$, where $v_{\rm c}^2 (r)$ peaks, because that is where the atmosphere is closest to being isothermal, and its asymptotic values at large and small $r$ are inspired by fits to equation (eq:AlphaFit).

Therefore, entropy-based methods for determining $\alpha (r)$ are more useful for characterizing non-cosmological processes, such as radiative cooling and energy input from the central galaxy. Those energy sinks and sources alter the pressure profile's shape function $\alpha (r)$ by modifying the entropy profile that would otherwise result from cosmological structure formation. 

The next three sections outline three physically justifiable non-cosmological modifications of $\alpha_K$ that users of the **ExpCGM** framework may wish to implement.

## Cooling Flow Profile 

The central regions of a galactic atmosphere generally have a radiative cooling time $t_{\rm cool}$ that is less than the universe's age. If heat input from the central galaxy falls short of replacing the energy lost to radiation, the central gas loses entropy and sinks into the central galaxy on a timescale $\sim t_{\rm cool}$. If the flow of gas is homogeneous, it is called a *cooling flow*. Its characteristic entropy gradient follows from the relationship
$$\alpha_K = \frac {d \ln K} {d \ln r} = - t_{\rm flow} \frac {d \ln K} {d t} = \frac {t_{\rm flow}} {t_{\rm cool}}$$
in which $t_{\rm flow} \equiv r / v_{\rm in}$ is the inflow timescale corresponding to the inflow speed $v_{\rm in} = - dr/dt$ of the cooling flow (see the [Cooling](Cooling) page for a more complete explanation). 

Gravitational compression in a potential well that is nearly isothermal keeps the temperature of homogeneous radiating gas nearly constant as it flows inward. The gas is "cooling" because its specific entropy is declining, even though its temperature is not changing very much. The cooling flow carries gas inward at the rate

<p>
  $$\dot{M}_{\rm cool}
            = 4 \pi r^2 \rho v_{\rm in}
            = \frac {4 \pi r^3 \rho} {t_{\rm flow}}
            = \frac {4 \pi r^3 \rho} {\alpha_K t_{\rm cool}}$$
</p>


A steady cooling flow, in which $\dot{M}\_{\rm cool}$ does not depend on radius, therefore has $\rho / t_{\rm cool} \propto r^3$. In a nearly isothermal potential well that maintains a nearly constant atmospheric temperature, one then finds $t_{\rm cool} \propto \rho^{-1}$, implying that $P (r) \propto \rho (r) \propto r^{-3/2}$ in a steady cooling flow.

The shape function approximation $\alpha_{\rm cool} \approx 1.5$ for a steady and nearly isothermal cooling flow has a numerical value similar to the power-law index $\alpha \approx 1.7$ describing the nearly isothermal parts of a cosmological atmosphere, but it has a completely different physical origin. In an **ExpCGM** atmospheric model, using $\alpha_{\rm cool} \approx 1.5$ is justified at radii where $t_{\rm cool}$ is much shorter than the universe's age and within which radiative cooling greatly exceeds feedback energy input. However, the cooling flow is likely to become inhomogeneous where $t_{\rm cool}$ drops below the atmosphere's dynamical time ($\sim r / v_{\rm c}$), calling for a different approach to specifying $\alpha$.

## Isentropic Core

At the other extreme from a pure cooling flow is centralized heat input that greatly exceeds radiative cooling. Under those conditions, heating raises the atmosphere's central entropy level, causing convection wherever $\alpha_K$ drops below zero. Convection then maintains $\alpha_K$ near zero, leading to development of an isentropic core with a constant entropy level. 

### Isentropic Shape Function

Isentropic gas with the equation of state $P = K \rho^{5/3}$ has $T \propto P^{2/5}$. The temperature profile of a hydrostatic isentropic core therefore satisfies

<p>
  $$\frac {dT} {d \ln r} ~=~ - \frac {2} {5} \frac {\mu m_p v_{\rm c}^2 (r)} {k} ~=~ - \frac {4} {5} T_\varphi (r)$$ 
</p>


in which $T_\varphi (r)$ is the gravitational temperature profile defined on the [Essentials](Essentials) page. Integrating this equation gives
$$T(r) = T_{\rm core} + \frac {4} {5}\int_r^{r_{\rm core}} T_\varphi (r) \frac {dr} {r}$$
where $r_{\rm core}$ is the outer radius of the isentropic region and $T_{\rm core} = T(r_{\rm core})$ is the atmospheric temperature at that radius. 

Inserting $T(r)$ into the equation
$$\alpha (r) = 2 \frac {T_\varphi (r)} {T(r)}$$
then gives the pressure profile's shape function at $r < r_{\rm core}$. That equation reduces to
$$\alpha (r) \approx \frac {2 T_\varphi (r)} {T_{\rm core} + 0.8 T_\varphi (r) \cdot \ln (r_{\rm core}/r) }$$
for an approximately isothermal potential well.

### Core Radius

The core radius of the isentropic region depends on how the amount $\Delta Q$ of central heating compares with the cumulative thermal energy profile preceding the heating event. Heating of gas at temperature $T$ raises its entropy $S$ by

<p>
  $$\Delta \ln K ~=~ \frac {2} {3k} \left( \frac {\mu m_p} {M_{\rm gas}} \right) \Delta S  ~=~ \frac {2} {3} \left( \frac {\mu m_p} {M_{\rm gas}} \right) \frac {\Delta Q} {k T}$$
</p>


where $M_{\rm gas}$ is the mass of heated gas, and the numerical factor 2/3 pertains to a gas with a thermal energy density of $3P/2$ (see the [Cooling](Cooling) page). Centralized thermalization of the energy $\Delta Q$ therefore produces an isentropic core out to a radius $r_{\rm core}$ containing a gas mass
$$M_{\rm gas} \sim \frac {2} {3} \left( \frac {\mu m_p} {k T_\varphi} \right) \Delta Q$$
In other words, the isentropic region consists of gas that had an original thermal energy content ($\sim 3 k T_\varphi /2$ per particle) comparable to the energy input $\Delta Q$.

To estimate $r_{\rm core}$, consider an atmosphere with a pressure profile that is initially a pure power law: $P = P_0 (r / r_0)^{-\alpha_0}$. The cumulative thermal energy profile of that atmosphere is
$$E_{\rm th} (r) = 4 \pi r_0^3 P_0 \int_0^{r/r_0} x^{2 - \alpha_0} dx
                   = \frac {4 \pi r_0^3 P_0} {3 - \alpha_0} 
                        \left( \frac {r} {r_0} \right)^{3 - \alpha_0}$$
The relationship $E_{\rm th} (r_{\rm core}) \approx \Delta Q$ then implies
$$r_{\rm core} \approx \: r_0 \times \left[ \frac {( 3 - \alpha_0) \Delta Q} {4 \pi r_0^3 P_0} \right]^{1/ (3 - \alpha_0)}$$
and the new shape function at $r < r_{\rm core}$ after the central heating event is
$$\alpha (r) \approx \frac {\alpha_0} { 1 + 0.4 \, \alpha_0 \,  \ln (r_{\rm core}/r) }$$
Similar estimates can be made for more complicated initial pressure profiles.

Notice that centralized heating can greatly reduce the rate at which a cooling flow feeds gas into a galaxy. If the galaxy's outer radius $r_{\rm gal}$ is much smaller than the radius of the newly isentropic core, then $\dot{M}\_{\rm cool} (r_{\rm gal})$ drops by a factor $\sim (r_{\rm core} / r_{\rm gal})^{2 \alpha_0}$. However, the isentropic core is more prone to runaway thermal instability than a stratified atmosphere, because buoyancy cannot suppress development of a multiphase medium there (see the [Multiphase Gas](MultiphaseGas) page). 

There is also an important caveat to keep in mind: Centralized heating of an atmosphere that is not spherically symmetric does not necessarily produce an isentropic core, especially if the heated gas escapes along low-density channels with a collective solid angle substantially less than $4 \pi$. For example, a rotating cooling flow naturally forms a cold, high-density disk inside of where its rotation speed approaches the potential's circular velocity (see the [Rotation](Rotation) page).  Heat input near the center of that disk tends to drive a bipolar outflow along the disk's rotation axis without significantly affecting the disk. It can therefore drive atmospheric circulation, in which the gas inflow through the rotating cooling flow roughly balances the bipolar gas outflow driven by central heating, when averaged over long time periods.

## Precipitation Limited Profiles

## Evolving the Shape Function
