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
and $\alpha \gtrsim 3/2$. An atmosphere with $\alpha = 3/2$ is only marginally bound to its halo, and atmospheric layers with $\alpha < 3/2$ may not be gravitationally bound (for an explanation, see \textbf{\textsc{ExpCGM:} Confinement}). Consequently, external pressure forces must somehow confine galactic atmospheres that have $\alpha < 3/2$ at all radii. 

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
is approximately constant, as is often the case. Both cosmological simulations and observations of galaxy clusters are consistent with $\alpha_K \approx 1.1$--$1.2$ outside a cluster's central regions. A cosmological galactic atmosphere should therefore have $\alpha \approx 1.7$ in regions that are nearly isothermal. However, both deviations from isothermality and changes in $\alpha_K$ result in deviations from $\alpha \approx 1.7$. 

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
$$\alpha (r) \approx {1.7} \left( \frac {2 \, r / r_{\rm max}} {1 + r/r_{\rm max}} \right)$$
to represent a cosmological atmosphere. It is designed to have $\alpha \approx 1.7$ near the radius $r_{\rm max} = 2.163 r_{\rm s}$, where $v_{\rm c}^2 (r)$ peaks, because that is where the atmosphere is closest to being isothermal, and its asymptotic values at large and small $r$ are inspired by fits to equation (\ref{eq:AlphaFit}).

Therefore, entropy-based methods for determining $\alpha (r)$ are more useful for characterizing non-cosmological processes, such as radiative cooling and energy input from the central galaxy. Those energy sinks and sources alter the pressure profile's shape function $\alpha (r)$ by modifying the entropy profile that would otherwise result from cosmological structure formation. The next three sections outline three physically justifiable non-cosmological modifications of $\alpha_K$ that users of the **ExpCGM** framework may wish to implement.

## Cooling Flow Profile 

The central regions of a galactic atmosphere generally have a radiative cooling time $t_{\rm cool}$ that is less than the universe's age. If heat input from the central galaxy falls short of replacing the energy lost to radiation, the central gas loses entropy and sinks into the central galaxy on a timescale $\sim t_{\rm cool}$. If the flow of gas is homogeneous, it is called a \textit{cooling flow} and has a characteristic entropy profile that follows from the relationship
$$\alpha_K = \frac {d \ln K} {d \ln r} 
             = - t_{\rm flow} \frac {d \ln K} {d t} 
             = \frac {t_{\rm flow}} {t_{\rm cool}}$$
in which $t_{\rm flow} \equiv r / v_{\rm in}$ is the inflow timescale corresponding to the inflow speed $v_{\rm in} = - dr/dt$ of the cooling flow  (see the [Cooling](Cooling) page for a more complete explanation). 

Gravitational compression in a potential well that is nearly isothermal keeps the temperature of homogeneous radiating gas nearly constant as it flows inward. The gas is ``cooling" because its specific entropy is declining, even though its temperature is not changing very much. The cooling flow carries gas inward at the rate
$$\dot{M}_{\rm cool}
            = 4 \pi r^2 \rho v_{\rm in}
            = \frac {4 \pi r^3 \rho} {t_{\rm flow}}
            = \frac {4 \pi r^3 \rho} {\alpha_K t_{\rm cool}}$$
A steady cooling flow, in which $\dot{M}_{\rm cool}$ does not depend on radius, therefore has $\rho / t_{\rm cool} \propto r^3$. In a nearly isothermal potential well that maintains a nearly constant atmospheric temperature, one then finds $t_{\rm cool} \propto \rho^{-1}$, implying that $P (r) \propto \rho (r) \propto r^{-3/2}$ in a steady cooling flow.

The shape function approximation $\alpha_{\rm cool} \approx 1.5$ for a steady and nearly isothermal cooling flow has a numerical value similar to the power-law index $\alpha \approx 1.7$ describing the nearly isothermal parts of a cosmological atmosphere, but it has a completely different physical origin. In an \textsc{ExpCGM} atmospheric model, using $\alpha_{\rm cool} \approx 1.5$ is justified at radii where $t_{\rm cool}$ is much shorter than the universe's age and within which radiative cooling greatly exceeds feedback energy input. However, the cooling flow is likely to become inhomogeneous where $t_{\rm cool}$ drops below the atmosphere's dynamical time ($\sim r / v_{\rm c}$), calling for a different approach to specifying $\alpha$.

## Isentropic Core

At the other extreme from a pure cooling flow is centralized heat input that greatly exceeds radiative cooling. Under those conditions, heating raises the atmosphere's central entropy level, causing convection wherever $\alpha_K$ drops below zero. Convection then maintains $\alpha_K$ near zero, leading to development of an isentropic core with a constant entropy level. The temperature profile of a hydrostatic isentropic core (which has $T \propto P^{2/5}$) satisfies
$$\frac {dT} {d \ln r} 
        ~=~ - \frac {2} {5} \frac {\mu m_p v_{\rm c}^2 (r)} {k} 
        ~=~ - \frac {4} {5} T_\varphi (r)$$ 
where $T_\varphi (r) \equiv \mu m_p v_c^2 (r) / 2 k$ is the gravitational temperature corresponding to $v_{\rm c}^2$. Integrating that equation gives
$$T(r) = T_{\rm core} + \frac {4} {5}\int_r^{r_{\rm core}} T_\varphi (r) \frac {dr} {r}$$
where $r_{\rm core}$ is the outer radius of the isentropic region and $T_{\rm core} = T(r_{\rm core})$ is the atmospheric temperature at that radius. After integrating to find $T(r)$, you can use the equation
$$\alpha (r) = 2 \frac {T_\varphi (r)} {T(r)}$$
to get the pressure profile's shape function at $r < r_{\rm core}$. That equation reduces to
$$\alpha (r) \approx \frac {2 T_\varphi (r)} {T_{\rm core} + 0.8 \, T_\varphi (r) 
                \cdot \ln (r_{\rm core}/r) }$$
for an approximately isothermal potential well.

The core radius of the isentropic region depends on how the amount $\Delta Q$ of central heating compares with the cumulative thermal energy profile preceding the heating event. Heating of gas at temperature $T$ raises its entropy $S$ by
$$\Delta \ln K ~=~ \frac {2} {3k}
                        \left( \frac {\mu m_p} {M_{\rm gas}} \right) 
                        \Delta S 
              ~=~ \frac {2} {3} 
                    \left( \frac {\mu m_p} {M_{\rm gas}} \right)
                    \frac {\Delta Q} {k T}$$
where $M_{\rm gas}$ is the mass of heated gas, and the numerical factor 2/3 pertains to a gas with a thermal energy density of $3P/2$ (see \textbf{\textsc{ExpCGM:} Cooling}). Centralized thermalization of the energy $\Delta Q$ therefore produces an isentropic core out to a radius $r_{\rm core}$ containing a gas mass
$$M_{\rm gas} \sim \frac {2} {3} \left( \frac {\mu m_p} {k T_\varphi} \right) \Delta Q$$
In other words, the isentropic region consists of gas that had an original thermal energy content ($\sim 3 k T_\varphi /2$ per particle) comparable to the energy input $\Delta Q$.

To estimate $r_{\rm core}$, consider an atmosphere with a pressure profile that is initially a pure power law: $P = P_0 (r / r_0)^{-\alpha_0}$. The cumulative thermal energy profile of that atmosphere is
$$E_{\rm th} (r) = 4 \pi r_0^3 P_0 \int_0^{r/r_0} x^{2 - \alpha_0} \, dx
                   = \frac {4 \pi r_0^3 P_0} {3 - \alpha_0} 
                        \left( \frac {r} {r_0} \right)^{3 - \alpha_0}$$
The relationship $E_{\rm th} (r_{\rm core}) \approx \Delta Q$ then implies
$$r_{\rm core} \approx \: r_0 \times \left[ \frac {( 3 - \alpha_0) \Delta Q} {4 \pi r_0^3 P_0} \right]^{1/ (3 - \alpha_0)}$$
and the new shape function at $r < r_{\rm core}$ after the central heating event is
$$\alpha (r) \approx \frac {\alpha_0} { 1 + 0.4 \, \alpha_0 \,  \ln (r_{\rm core}/r) }$$
Similar estimates can be made for more complicated initial pressure profiles.

Notice that centralized heating can greatly reduce the rate at which a cooling flow feeds gas into a galaxy. If the galaxy's outer radius $r_{\rm gal}$ is much smaller than the radius of the newly isentropic core, then $\dot{M}_{\rm cool} (r_{\rm gal})$ drops by a factor $\sim (r_{\rm core} / r_{\rm gal})^{2 \alpha_0}$. However, the isentropic core is more prone to runaway thermal instability than a stratified atmosphere, because buoyancy cannot suppress development of a multiphase medium there (see \textsc{\textbf{ExpCGM: Multiphase Gas}}). 

There is also an important caveat to keep in mind: Centralized heating of an atmosphere that is not spherically symmetric does not necessarily produce an isentropic core, especially if the heated gas escapes along low-density channels with a collective solid angle substantially less than $4 \pi$. For example, a rotating cooling flow naturally forms a cold, high-density disk inside of where its rotation speed approaches the potential's circular velocity (see the [Rotation](Rotation) page).  Heat input near the center of that disk tends to drive a bipolar outflow along the disk's rotation axis without significantly affecting the disk. It can therefore drive atmospheric circulation, in which the gas inflow through the rotating cooling flow roughly balances the bipolar gas outflow driven by central heating, when averaged over long time periods.

## Precipitation Limited Profiles 

Observations of galaxy groups and clusters with a short central cooling time ($t_{\rm cool} \lesssim 1 \, {\rm Gyr}$) show that the shape functions of their central pressure profiles ($r \lesssim 20 \, {\rm kpc}$) are in between expectations for a pure cooling flow ($\alpha \approx 3/2$) and centralized heating ($\alpha < 1$). Neither heating nor cooling dominates. Instead, interplay between heating and cooling appears to be suspending the atmosphere in a quasi-steady state.

One observed characteristic of that seemingly quasisteady atmospheric state is a constant ratio of cooling time to dynamical time, as represented by a freefall time $t_{\rm ff} \equiv (2 r / g)^{1/2}$ based on the local gravitational acceleration $g$. In a nearly isothermal potential (with $t_{\rm ff} \propto r$), a constant $t_{\rm cool}/t_{\rm ff}$ ratio implies $\rho \propto t_{\rm cool}^{-1} \propto r^{-1}$, because the atmosphere's temperature remains approximately constant with radius. The shape function for a pressure profile in that state is $\alpha \approx 1$.

Numerical simulations suggest that such an atmosphere is in a marginally stable state with properties shaped by susceptibilty to condensation. If $t_{\rm cool}/t_{\rm ff} < 1$, then atmospheric gas can cool and condense faster than it can fall to the center of the potential well. It is therefore prone to forming a \textit{multiphase medium} consisting of volume-filling gas (at temperature $T \sim T_\varphi$) containing much cooler embedded clouds ($T \ll T_\varphi$) that can be orders of magnitude denser (for a more extensive description, see the [Multiphase Gas](MultiphaseGas) page). 

Observations show that $\min (t_{\rm cool}/t_{\rm ff}) \sim 10$ is the typical lower limit for the ratio of cooling time to freefall time in galaxy clusters and groups. A plausible explanation for this minimum ratio is that ``precipitation" of dense clouds out of the ambient medium may be exponentially sensitive to $t_{\rm cool}/t_{\rm ff}$ in the range $3 \lesssim t_{\rm cool}/t_{\rm ff} \lesssim 20$. According to that hypothesis, an atmosphere at the low end of the range is overly prone to formation of cold clouds that rain down upon the central galaxy and fuel energetic feedback that expands the atmosphere and raises the value of $t_{\rm cool}$.

The **ExpCGM** framework therefore calls pressure profiles with $\alpha \approx 1$ and $t_{\rm cool}/t_{\rm ff} \sim 10$ at small radii ***precipitation limited.*** There are at least three options for imposing a precipitation limit on a cosmological entropy profile:

* **Linked Normalization.** The simplest method forces a shape function that is cosmological at large radii to morph into a pressure profile with an approximately constant $t_{\rm cool} / t_{\rm ff}$ ratio at small radii. That can be done by setting $\alpha_{\rm in} = 1$ in equation (\ref{eq:AlphaFit}) or by modifying the fitting formula's simplified version, so that 
  $$\alpha (r) \approx 1 + 0.7 \left( \frac {2 r / r_{\rm max}} { 1 + r / r_{\rm max} } \right)$$
In this method, increases in $E_{\rm CGM}$ change the pressure profile's normalization factor $P_0$ without changing its shape function, meaning that the pressure normalization of the inner region, which may be limited by precipitation, is \textit{linked} to the normalization of the outer region, where the shape function's slope is cosmological. Implicitly, this method assumes that feedback energy input is evenly spread over the entire atmosphere, so that $K(r)$ rises by the same factor at all radii.

* **Unlinked Normalization.** This method is based on representing a halo's precipitation-limited entropy profile with the power law
  $$K_{\rm pre} (r) = K_{{\rm pre},0} \left( \frac {r} {r_0} \right)^{2/3}$$
and its cosmological entropy profile with 
$$ K_{\rm cos} (r) = K_{{\rm cos},0} \left( \frac {r} {r_0} \right)^{11/10}
The normalization factor $K_{{\rm pre},0}$ corresponds to a limiting $t_{\rm cool}/t_{\rm ff}$ ratio, and simulations of cosmological structure formation determine the normalization factor $K_{{\rm cos},0}$. The two normalization factors are \textit{unlinked} in the sense that feedback energy input from the central galaxy raises $K_{\rm pre}$ but not $K_{\rm cos}$. Summing the cosmological and precipitation limited entropy profiles gives the combined profile
$$K(r) = K_{{\rm pre},0} \left( \frac {r} {r_0} \right)^{2/3} + K_{{\rm cos},0} \left( \frac {r} {r_0} \right)^{11/10}$$
with the entropy shape function
$$\alpha_K(x) = \frac {2} {3} \left( \frac {1} {1+y} \right) + \frac {11} {10} \left( \frac {y} {1+y} \right)$$
in which $y = (K_{{\rm cos},0} / K_{{\rm pre},0}) (r / r_0)^{13/30}$. In an atmosphere with a negligible temperature gradient, this expression for $\alpha_K$ gives the approximation
$$\alpha (r) \approx \frac {1 + 1.7 y} {1+y}$$ 
for the pressure shape function. Users who do not want to neglect the temperature gradient can implement the approximation
$$\alpha (r) \approx \frac {1} {1+y} + 1.7 \left( \frac {2 r / r_{\rm max}} 
                                { 1 + r / r_{\rm max}} \right)
                                \frac {y} {1+y}$$
which reduces to equation (\ref{eq:AlphaFitSimplified}) in the limit $K_{{\rm cos},0} \gg K_{{\rm pre},0}$. When this method is used, feedback energy input reduces $K_{{\rm cos},0} / K_{{\rm pre},0}$ because it changes only the inner normalization factor $K_{{\rm pre},0}$. Implicitly, both feedback and cooling are assumed to affect the precipitation-limited inner part of the atmosphere more than its cosmological outer part. Feedback therefore shifts the transition in entropy slope at $y \approx 1$ to larger radii and cooling shifts it to smaller radii.

* **Entropy Ramp.**
Another approach to placing a precipitation limit on the inner part of a cosmological pressure profile is to impose an entropy ramp, constrained to have constant $t_{\rm cool} / t_{\rm ff}$, that is analogous to the entropy floor imposed by centralized heating. Choosing the definition $K = kT n_e^{-2/3}$ for $K$, we can express the cooling time as a function of $K$ and $T$:
$$t_{\rm cool} = \left( \frac {3n} {2 n_i} \right)
                    \frac {K^{3/2}} {(kT)^{1/2} \Lambda (T)}$$
The precipitation-limited entropy profile corresponding to a constant value of $\chi \equiv t_{\rm cool} / t_{\rm ff}$ is then
$$K_{\rm pre} = K_{{\rm pre},0} \left( \frac {r} {r_0} \right)^{2/3}$$
with
$$K_{{\rm pre},0} = \left[ \frac {2^{1/2}} {3} 
                                    \left( \frac {2 n_i} {n} \right)
                                    \frac {(kT)^{1/2} \Lambda (T)}
                                          {v_{\rm c}}
                                    \right]^{2/3}
                                    \chi^{2/3} r_0^{2/3}$$
This is the value of $K_{{\rm pre},0}$ one would use to evaluate $y$ in the previous method. A potential complication with both methods is that $K_{{\rm pre},0}$ depends on $T$, and the equilibrium value of $T$ is determined by the shape function $\alpha$ that we are trying to calculate. That complication can be mitigated if $T$ depends only weakly on $r$, in which case $\alpha \approx 1$ and $kT \approx \mu m_p v_{\rm c}^2$ for $K \approx K_{\rm pre}$. Inserting that temperature approximation into the expression for $K_{{\rm pre},0}$ then yields 
$$P_{\rm pre} (r) \approx \frac {n} {n_e} 
                                  \frac {(\mu m_p v_{\rm c}^2 )^{5/2}} {K_{{\rm pre},0}^{3/2}}
                                  \left( \frac {r} {r_0} \right)^{-1}$$
for the precipitation-limited part of the pressure profile, which ends where $K_{\rm pre} = K_{\rm cos}$. However, an iterative solution for $K_{{\rm pre},0}$ and $P_{\rm pre} (r)$ is necessary if the temperature gradient is significant.

## Evolving the Shape Function

The reason for having multiple options for $\alpha (r)$ is that the spatial distribution of feedback energy input within a galaxy's atmosphere remains unknown. Comparisons of the different options with both observations and numerical simulations will eventually show which options most faithfully represent various modes of feedback energy input. Also, as feedback modes and the radial profile of energy input change, an atmosphere's shape function may evolve with time.

In the **ExpCGM** framework, changes in $\alpha (r)$ during a time interval $\Delta t$ can be managed in two distinct steps. The first step is a computation of the atmosphere's mass change $\Delta M_{\rm CGM}$ and total energy change $\Delta E_{\rm CGM}$, based on the atmosphere's current shape function. The second step determines the atmosphere's structure based on the new values of $M_{\rm CGM}$ and $E_{\rm CGM}$ and an updated shape function that satisfies user-defined constraints imposed upon it.

For example, consider a time interval that starts with a power-law shape function and ends with an isentropic core produced by an increment $\Delta E_{\rm fb}$ of centralized feedback energy. Radiative losses determined from the power-law configuration reduce $E_{\rm CGM}$ by $\dot{E}\_{\rm rad} \cdot \Delta t$ during the time interval, while feedback and cosmological accretion boost $E_{\rm CGM}$ by $\Delta E_{\rm fb}$ and $\Delta E_{\rm acc}$, respectively. Those processes also add gas mass increments $\Delta M_{\rm fb}$ and $\Delta M_{\rm acc}$ to $M_{\rm CGM}$, while radiative cooling removes $\dot{M}_{\rm cool} \cdot \Delta t$. That concludes the first step.

The second step begins with a calculation of $r_{\rm core}$, based on the initial power-law configuration, giving
$$r_{\rm core} \approx r_0 \times 
                \left[ \frac {( 3 - \alpha_0) \Delta E_{\rm fb}} 
                             {4 \pi r_0^3 P_0} \right]^{1/ (3 - \alpha_0)}$$
With that value of $r_{\rm core}$, a new shape function for the atmosphere can be calculated using the procedure outlined in section \ref{sec:Isentropic}. Then, the usual \textsc{ExpCGM} method can be used to determine the atmosphere's new configuration, based on the new shape function, the updated values of $E_{\rm CGM}$ and $M_{\rm CGM}$, and possibly also updated values of $v_\varphi^2$ and $r_{\rm s}$, if the potential well's characteristics have changed. 

These two steps can be generalized to accommodate other kinds of changes in $\alpha (r)$. However, restrictions on $\Delta t$ may be necessary to ensure that the resulting changes in $\alpha (r)$ are at least qualitatively consistent with the physical rationale for those changes. Keep in mind that the overall aim of \textsc{ExpCGM} modeling is to explore how feedback from a halo's central galaxy both shapes its atmosphere and responds to the atmosphere's configuration. Therefore, one of its main goals is to test the community's \textit{assumptions} about the relationship between feedback and the atmosphere's shape function (including the assumptions implicit in numerical simulations) and to assess those assumptions through comparisons of **ExpCGM** models with observations. 
