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

The simplest physically justifiable pressure profiles are pure power laws, with
$$P(r) = P_0 \left( \frac {r} {r_0} \right)^{-\alpha}$$
and $\alpha \gtrsim 3/2$. An atmosphere with $\alpha = 3/2$ is only marginally bound to its halo, and atmospheric layers with $\alpha < 3/2$ may not be gravitationally bound (see the [Confinement](Confinement) page for an explanation). Consequently, external pressure forces must somehow confine galactic atmospheres that have $\alpha < 3/2$ at all radii. 

## Cosmological Profiles

Cosmological structure formation shapes the large-scale properties of galaxy-cluster atmospheres. Both observations and numerical simulations of galaxy clusters show that their cosmologically-determined pressure profiles are not pure power laws, especially at large radii. Typically, the shape functions of those pressure profiles are flatter than $\alpha = 3/2$ at small radii and steeper than $\alpha = 3/2$ at large radii. As a result, the cosmological pressure profiles of galaxy clusters are often represented with a fitting formula equivalent to the shape function
$$\alpha (r) = \alpha_{\rm in} + \left( \alpha_{\rm out} - \alpha_{\rm in} \right) \left[ \frac {(r / r_{\rm tr})^{\alpha_{\rm tr}}} { 1 +(r / r_{\rm tr})^{\alpha_{\rm tr}}} \right]$$
The parameter $r_{\rm tr}$ is a transitional radius for the profile's slope that is approximately twice the scale radius $r_{\rm s}$ of an NFW fit to the halo's gravitational potential. The other parameters represent the pressure profile's power-law slope in the appropriate limits. Fits to observations of thermal pressure in galaxy clusters give an inner slope $\alpha_{\rm in} \approx 0.3$, an outer slope $\alpha_{\rm out} \approx 4.3$, and $\alpha_{\rm tr} \approx 1.1$. 

## Entropy-Based Profiles

Another approach to specifying cosmological pressure profiles, also useful for modifying them, focuses on the normalization factor $K$ of the polytropic equation of state $P = K \rho^\gamma$ relating $P$ to $\rho$. The quantity $K$ is sometimes called the atmosphere's ``entropy" because changes in $\ln K$ are directly proportional to changes in specific entropy (see the [Cooling](Cooling) page). A pressure profile's shape function can therefore be separated into two terms
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

## Precipitation Limited Profiles

## Evolving the Shape Function
