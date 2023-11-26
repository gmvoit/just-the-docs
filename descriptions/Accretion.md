---
title: Accretion
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

# Accretion
{: .no_toc}

This page outlines how the **ExpCGM** framework incorporates cosmic accretion of atmospheric mass and energy. It employs a spherical collapse assumption to estimate the specific energy of incoming gas. It also accounts for gravitational compression of atmospheric gas as the confining halo's potential well deepens. A user may also wish to account for atmospheric compression caused by the incoming momentum flux. After describing these three aspects of **ExpCGM**, the page concludes with a summary of cosmological atmospheric evolution.

<details closed markdown="block">
  <summary>
    Table of contents
  </summary>
   {: .text-delta}
- TOC
{:toc}  
</details>

## Spherical Collapse

One of the energy sources responsible for both atmospheric heating and turbulence is cosmological accretion. The **ExpCGM** framework handles this contribution using the classic spherical collapse approximation.

### Equation of Motion

The equation of motion for a cosmological spherical shell of radius $R$ surrounding a constant mass $M$ is
$$\ddot{R} = - \frac {G M} {R^2} + H_0^2 \Omega_\Lambda R$$
The first term on the right accounts for Newtonian gravity. The second one accounts for dark energy in an expanding universe with a Hubble constant $H_0$ at the present time. It assumes that dark energy has a constant energy density $\Omega_\Lambda c^2$ times the current cosmological critical density $\rho_{\rm cr,0} \equiv 3 H_0^2 / 8 \pi G$. 

Multiplying that equation of motion by $\dot{R}$ and integrating over time gives the specific kinetic energy of a cosmological shell of radius $R$:
$$\frac {\dot{R}^2} {2} = \frac {G M} {R} \left( 1 - \frac {R} {R_{\rm ta}} \right) + \frac {H_0^2 \Omega_\Lambda} {2} R^2 \left( 1 - \frac {R_{\rm ta}^2} {R^2}  \right)$$
The terms containing the shell's turnaround radius $R_{\rm ta}$ (at which $\dot{R} = 0$) come from the constant of integration. The shell expands until it reaches $R_{\rm ta}$ at a turnaround time $t_{\rm ta}$. It then falls inward until it accretes onto the galaxy's cosmological halo at a collapse time $t_{\rm coll} \approx 2 t_{\rm ta}$. 

### Incoming Kinetic Energy

According to the spherical collapse approximation, infalling matter accreting onto a cosmological halo of mass $M_{\rm halo}$ and radius $R_{\rm halo}$ at time $t$ has a specific kinetic energy
$$\varepsilon_{\rm kin} (t) = \frac {G M_{\rm halo}} {R_{\rm halo}} \left[ 1 - \frac {R_{\rm halo}} {R_{\rm ta}} - \frac {\Omega_\Lambda \rho_{\rm cr,0}} {\rho_{\rm halo}} \left( \frac {R_{\rm ta}^3} {R_{\rm halo}^3} - 1 \right) \right]$$
The quantity $\rho_{\rm halo} \equiv 3 M_{\rm halo} / 4 \pi R_{\rm halo}^3$ in this expression is the halo's mean matter density at time $t$. 

### Halo Radius

A real cosmological halo does not have a distinct radius. Cosmological simulations show that gravitational scattering converts radial infall into randomly oriented orbital motion over a range of radii in the vicinity of $R_{\rm ta} / 2$. Consequently, there is some freedom to define $R_{\rm halo}$ so that it suits the situation of interest. 

Typically, a halo's radius (and consequently its mass) is defined in terms of a contrast factor
$$\Delta_{\rm c} = \frac {\rho_{\rm halo}} {\rho_{\rm cr}(t)}$$
relative to the universe's critical density $\rho_{\rm cr}(t) = 3 H^2 (t) / 8 \pi G$, in which $H(t)$ is the Hubble expansion parameter at time $t$. The standard value for **ExpCGM** models is $\Delta_{\rm c} = 200$.

For all suitable definitions of $\Delta_{\rm c}$, the dark-energy correction factor
$$\frac {\Omega_\Lambda \rho_{\rm cr,0}} {\rho_{\rm cr}(t)} = \frac {\Omega_\Lambda} {\Delta_{\rm c}} \frac {H_0^2} {H^2 (t)}$$
is less than one percent. Neglecting that factor and assuming $R_{\rm halo} \approx R_{\rm ta} / 2$ then gives the approximation
$$\varepsilon_{\rm kin} (t) \approx \frac {G M_{\rm halo}(t)} {2 R_{\rm halo}}$$
for the specific kinetic energy of infalling matter at time $t$.

{: .note}
Accounting for the gravitational potential energy of incoming matter requires a more subtle approach because the **ExpCGM** framework places the zero point of the gravitational potential in the vicinity of the central galaxy. For instance, the simple example on the [Essentials](Essentials) page puts it at $r=0$ in an NFW gravitational potential. The specific gravitational potential energy of incoming matter, $\varphi(R_{\rm halo})$, then depends on the relationship between $R_{\rm halo}$ and the potential well's scale radius $r_{\rm s}$. See the [Confinement](Confinement) page for more detail on how **ExpCGM** handles an atmosphere's gravitational potential energy.

## Adiabatic Compression

Another cosmological source of atmospheric heating is adiabatic gravitational compression. Mass that falls through a radius $r$ and remains at smaller radii increases the halo's circular velocity at $r$. None of the infalling matter needs to be baryonic but some of it may consist of baryonic mass in stars. The increase in gravitational force at $r$ compresses the atmospheric gas within $r$, thereby increasing both its temperature and turbulent velocity dispersion, if no support energy is added to offset compression.

To assess the contribution that adiabatic compression makes to the total atmospheric energy $E_{\rm CGM}$, consider the effects of a change $\Delta \varphi$ in the gravitational potential without a compensating change in the atmosphere's pressure, temperature, or density profiles. Cosmological accretion alone changes the atmosphere's total gravitational energy by
  $$\Delta E_{\varphi,{\rm cos}} = \int_0^{r_{\rm CGM}} \Delta \varphi \cdot 4 \pi r^2 \rho dr$$
while $E_{\rm th}$, $E_{\rm nt}$, and $M_{\rm CGM}$ temporarily stay the same (see [Essentials](Essentials) for definitions of symbols). If the change in $\varphi$ comes from an increase of the total mass within $r_{\rm CGM}$, then it raises the circular velocity there and also at smaller radii. The resulting change in the atmosphere's scaled specific energy $\varepsilon_{\rm CGM}/v_\varphi^2$ is 
  $$\Delta \left( \frac {\varepsilon_{\rm CGM}} {v_\varphi^2} \right) = \left[ \frac {\Delta E_{\varphi,{\rm
cos}}} {E_\varphi} \frac {E_\varphi} {E_{\rm CGM}} - \frac {\Delta v_\varphi^2} {v_\varphi^2} \right] \frac {\varepsilon_{\rm CGM}} {v_\varphi^2}$$
in which $v_\varphi$ is the normalization of the potential's circular velocity profile.

A simple renormalization of the gravitational potential, proportional to $v_\varphi^2$, causes a decrease in $\varepsilon_{\rm CGM}/v_\varphi^2$. That happens because $\Delta E_{\varphi,{\rm cos}} / E_\varphi = \Delta v_\varphi^2 / v_\varphi^2$ and $E_\varphi / E_{\rm CGM} < 1$. A change in both the normalization $v_\varphi$ and scale radius $r_{\rm s}$ of the potential well is a slightly more complicated case, giving
  $$\Delta \varphi (x) = \varphi (x) \frac {\Delta v_\varphi^2} {v_\varphi^2} - v_c^2(x) \frac {\Delta r_{\rm s}} {r_{\rm s}}$$
for the change in $\varphi(x)$ at $x = r / r_{\rm s}$. In that case one finds 
  $$\frac {\Delta E_{\varphi,{\rm cos}}} {E_\varphi}  = \frac {\Delta v_\varphi^2} {v_\varphi^2} - \frac {\Delta r_{\rm s}} {r_{\rm s}} \int_0^{x_{\rm CGM}} \frac {\alpha (x) f_{\alpha} (x)} {J_\varphi(x_{\rm CGM}) f_{\rm th}(x)} x^2 dx$$ 
when $\Delta \varphi$ is inserted into the integral for the cosmological change in $E_\varphi$. Therefore, increasing both $v_\varphi$ and $r_{\rm s}$ causes an even greater decrease in $\varepsilon_{\rm CGM} / v_\varphi^2$ than just an increase of $v_\varphi$. 

In the **ExpCGM** framework, a decrease in $\varepsilon_{\rm CGM}/v_\varphi^2$ leads to a decrease in the atmosphere's scaled equilibrium radius $x_{\rm CGM} = r_{\rm CGM} / r_{\rm s}$. The increase in the gravitational potential temporarily causes gravitational forces to exceed pressure forces. Adiabatic compression then raises the temperature of a thermally supported atmosphere by
$$\Delta T (r) \approx \frac {\Delta v_\varphi^2} {v_\varphi^2} T (r)$$
so that support energy and gravity come back into force balance. 

{: .note}
Thermal and turbulent energy input associated with mass accretion that deepens the potential well has little effect on the atmosphere's equilibrium temperature, because $T(r)$ in the **ExpCGM** framework is determined by $v_{\rm c}(r)$ and $\alpha(r)$. Instead, support energy that accreting gas adds directly to the atmosphere offsets adiabatic contraction by increasing $\varepsilon_{\rm CGM}$ and $r_{\rm CGM}$ without changing the atmosphere's equilibrium temperature.

## Incoming Momentum

Baryonic gas accreting onto a galactic atmosphere does more than just add energy. It also applies some pressure. Incoming gas that is initially cold passes through an accretion shock at the atmosphere's outer edge. In an idealized, spherically symmetric accretion flow, the inward momentum flux of accreting gas at the shock's radius $R_{\rm acc}$ is

<p>
  $$( \rho v^2 )_{\rm acc} = \frac {\dot{M}_{\rm acc} (R_{\rm acc})} { 4 \pi R_{\rm acc}^2 v_{\rm acc} ( R_{\rm acc} )}$$
</p>


where $\dot{M}\_{\rm acc} = \zeta f_{\rm b} \dot{M}\_{\rm halo}$ is the gas mass accretion rate and $v_{\rm acc} \approx  v_c (R_{\rm acc}) \cdot ( 1 - R_{\rm acc} / R_{\rm ta} )^{1/2}$ is the accretion speed of a mass shell that turned around at radius $R_{\rm ta}$. Momentum conservation requires that $P + \rho v^2$ be constant across the shock front (in the frame of that front), meaning that incoming momentum applies a boundary condition on the atmosphere's pressure profile. 

{: .note}
Standard **ExpCGM** models related $\dot{M}\_{\rm acc}$ to the halo's cosmological accretion rate through the cosmological baryon fraction $f_{\rm b}$. However, users can also apply a ***preventative feedback factor*** $\zeta \leq 1$, so that $\dot{M}\_{\rm halo} = \zeta f_{\rm b} \dot{M}\_{\rm halo}$, to account for phenomena that reduce the ratio of gas mass accretion to total mass accretion. 

### Asymmetric Accretion

The difficulty with applying a boundary condition representing incoming momentum is cosmological accretion's lack of spherical symmetry. Much of the gas entering a galaxy's atmosphere streams in along cosmological filaments or plunges in along with merging subhalos. The gas associated with filaments and subhalos tends to be much denser than the ambient gas of a halo's outer atmosphere and is unimpeded by it. Meanwhile, ambient atmospheric gas that is rising outward can proceed along pathways posing less resistance. Nevertheless, the accreting gas must slow down somewhere in the atmosphere by shedding its infalling momentum and exerting an inward force.

### Force Balance Modification

In the spherically symmetric **ExpCGM** framework, the effects of incoming momentum on the pressure gradient can be included by adding an accretion term depending on $\dot{M}\_{\rm acc}$ and $v_{\rm acc}$ to the force balance equation:

<p>
  $$\frac {d} {dr} \frac {P} {f_{\rm th}} = - \, \rho \frac {d \varphi} {dr} - \frac {v_{\rm acc}} {4 \pi r^2} \frac {d \dot{M}_{\rm acc}} {dr}$$
</p>
    

The gradient of the inward mass flux assists gravitational confinement of the atmosphere and can be represented in terms of the standard **ExpCGM** force balance equation
  $$\frac {d} {dr} \frac {P} {f_{\rm th}} = - \, \rho f_\varphi \frac {d \varphi} {dr}$$
using the modification factor

<p>
 $$f_\varphi  = 1 + \frac {(\rho v^2)_{\rm acc}} {\rho v_c^2} \frac {d \ln \dot{M}_{\rm acc}} {d \ln r}$$
</p>


Note that $\rho v_{\rm c}^2 = P_0 [ \alpha_{\rm eff} (r) f_\alpha (r) / f_\varphi f_{\rm th} (r)]$ in an equilibrium **ExpCGM** atmosphere.

Momentum transfer that happens within a narrow range of radii, as in a spherical accretion shock, leads to a sharp pressure jump, but momentum transfer spread over a broad range of radii has a less pronounced effect on the pressure gradient.  If the accreting gas deposits its momentum over a broad region, then the logarithmic derivative of $\dot{M}\_{\rm acc}$ in the expression for $f_\varphi$ will be of order unity. The magnitude of the accretion-dependent term then depends on how the inward mass flux $(\rho v^2)\_{\rm acc}$ at radius $r$ compares with the value of $\rho v_{\rm c}^2 = \alpha_{\rm eff} P / f_{\rm th}$ in the ambient atmosphere. 

{: .note}
A correction for incoming momentum is more important in low-density atmospheres that have significantly expanded in response to central energy input than in uninflated atmospheres. High-density accreting gas penetrates more deeply into those expanded atmospheres before decelerating, making its inward momentum flux a larger fraction of $\rho v_{\rm c}^2$.

### Deceleration Zone

Implementation of a momentum transfer correction requires guidance from numerical simulations. What's needed is an approximate power-law dependence of the gas inflow rate $\dot{M}\_{\rm acc} = 4 \pi r^2 (\rho v)\_{\rm acc}$ on radius. 

Suppose that $r_{\rm dec}$ is the outer radius of the deceleration zone, that $\dot{M}\_{\rm acc} \propto r^{\eta_{\rm dec}}$ within $r_{\rm dec}$, and that $v_{\rm acc} \approx v_{\rm c}$ is approximately constant with radius. In that case, we find

<p>
  $$f_\varphi \approx 1 + {\eta_{\rm dec}} \left[ \frac { \dot{M}_{\rm acc} (R_{\rm acc})} {4 \pi r_{\rm dec}^2 \rho v_{\rm c}} \right] \left( \frac {r} {r_{\rm dec}} \right)^{\eta_{\rm dec}}$$
</p>

in which the magnitude of the factor in square brackets is similar to the ratio of the gas mass entering $r_{\rm dec}$ during a dynamical time $r_{\rm dec} / v_{\rm c}$ to the gas mass currently within $r_{\rm dec}$. If the atmosphere's state is quasi-steady, then $(\rho v^2)\_{\rm acc} \sim \rho v_{\rm c}^2$ corresponds to a situation in which ambient gas at $r_{\rm dec}$ is flowing outward at a speed similar to the inflow speed of accreting gas. A smaller correction to purely gravitational force balance corresponds to an outflow speed that is small compared with $v_{\rm c}$. 

### Accretion-Corrected Temperature

If the effects of momentum transfer are locally significant compared to pressure support, then a choice must be made about how to represent the response of the atmosphere's pressure profile to the additional confinement. In the **ExpCGM** framework, the shape function for support energy (i.e. $\alpha$ or $\alpha_{\rm eff}$) is chosen by the user. To account for incoming momentum, **ExpCGM** users can also opt for an approximate accretion-corrected temperature profile 

<p>
    $$T(r) \approx \frac {2 f_{\rm th} (r)} {\alpha_{\rm eff}(r)} \left[ 1 + {\eta_{\rm dec}} \frac { \dot{M}_{\rm acc} (R_{\rm acc})} {4 \pi r_{\rm dec}^2 \rho (r) v_{\rm c} (r)} \left( \frac {r} {r_{\rm dec}} \right)^{\eta_{\rm dec}} \right] T_\varphi (r)$$
</p>


in which $\rho (r)$ follows from $P(r)$ and $T(r)$. 

{: .note}
Some iteration may be needed to obtain a self-consistent solution for $T(r)$, because $\rho (r)$ is part of the correction. 

### Pressure Jump

If the accreting gas decelerates over a narrow range of radii, then its confining effects on the pressure profile may better represented as a discontinuity in the pressure profile at $r_{\rm dec}$:

<p>
    $$\Delta \left( \frac {P} {f_{\rm th}} \right) \approx - \frac {\dot{M}_{\rm acc} v_{\rm acc}} { 4 \pi r_{\rm dec}^2}$$
</p>


The chosen shape function and potential well can continue to determine the product of $f_{\rm th}(r)$ and $T(r)$ outside of $r_{\rm dec}$, but the pressure normalization factor $P_0$ in the usual expressions for $P(r)$ and $\rho(r)$ drops beyond $r_{\rm dec}$ to a lower value

<p>
    $$P_0 \left[ 1 - \frac {\dot{M}_{\rm acc} v_{\rm acc}} {4 \pi r_{\rm dec}^2 P (r_{\rm acc})} \right]$$
</p>


consistent with the pressure jump at $r_{\rm dec}$.

{: .note}
In the limit of a sharp pressure jump, the equilibrium temperature profile in **ExpCGM** is continuous across the pressure discontinuity as long as $\alpha(r)$ is continuous. Only the pressure normalization changes.

## Cosmological Evolution

To summarize how accretion of both gas mass and total mass affects the cosmological evolution of a galactic atmosphere:

* **Spherical Collapse.**
Cosmological structure formation brings gas into a halo of mass $M_{\rm halo}$ and radius $R_{\rm halo}$ with a specific kinetic energy 
$$\varepsilon_{\rm kin} \approx \frac {G M_{\rm halo}} {2 R_{\rm halo}} = \frac {v_{\rm c}^2 (R_{\rm halo})} {2}$$
Users of the **ExpCGM** framework are free to choose the density contrast factor $\Delta_{\rm c}$ that defines $R_{\rm halo}$ and relates it to the turnaround radius $R_{\rm ta}$ of matter that accretes through $R_{\rm halo}$ at time $t$. Once that choice has been made, users who wish to can apply the more precise expression
$$\varepsilon_{\rm kin} (t) = \frac {G M(t)} {R_{\rm halo}} \left[ 1 - \frac {R_{\rm halo}} {R_{\rm ta}} - \frac {\Omega_\Lambda \rho_{\rm cr,0}} {\rho_{\rm halo}} \left( \frac {R_{\rm ta}^3} {R_{\rm halo}^3} - 1 \right)  \right]$$
for the specific kinetic energy of accreting gas.

{: .note}
At present, the **ExpCGM** framework does not account for any thermal energy that accreting gas might have prior to passing within $R_{\rm halo}$.

* **Adiabatic Compression.**
Changes in the halo's gravitational potential owing to cosmological accretion change the atmosphere's total energy $E_{\rm CGM}$ by the amount
  $$\Delta E_{\varphi,{\rm cos}} = \int_0^{r_{\rm CGM}} \Delta \varphi \cdot 4 \pi r^2 \rho dr$$
Simply increasing the potential's normalization factor $v_\varphi^2$ by the fraction $\Delta v_\varphi^2 / v_\varphi^2$ results in a fractional increase $\Delta E_{\varphi,{\rm cos}} / E_{\rm CGM}$ in total atmospheric energy that is smaller than $\Delta v_\varphi^2 / v_\varphi^2$, meaning that the atmosphere becomes more strongly bound.

{: .note}
The **ExpCGM** framework does not yet account for changes in the gravitational potential stemming from changes in the radial distribution of atmospheric gas.

* **Incoming Momentum.**
Accreting gas might not transfer much of its kinetic energy to the atmosphere until reaching a deceleration radius $r_{\rm dec}$ significantly smaller than $R_{\rm halo}$. Momentum transfer to the atmosphere as accreting gas decelerates in the vicinity of $r_{\rm dec}$ assists gravitational confinement. The effects of momentum transfer on the atmosphere's pressure and density structure depend on: (1) how the mean inward momentum flux $(\rho v)\_{\rm acc}$ near $r_{\rm dec}$ compares with $\rho v_{\rm c}^2 = \alpha_{\rm eff} P / f_\varphi f_{\rm th}$ in the ambient atmosphere at that radius, and (2) the radial width of the deceleration zone.

* **Net Energy Change.**
The net change in atmospheric specific energy stemming from cosmological accretion during a time interval $\Delta t$ is
  $$\Delta E_{\rm acc} = \left[ \varepsilon_{\rm kin}  + \varphi(R_{\rm halo}) \right] \Delta M_{\rm CGM} + \Delta E_{\varphi,{\rm cos}}$$
where $\Delta M_{\rm CGM} = \zeta f_{\rm b} \dot{M}\_{\rm halo} \, \Delta t$ is the gas mass added through accretion. Without radiative losses that reduce $E_{\rm CGM}$ or feedback energy that increases $E_{\rm CGM}$, the net effect of cosmological accretion on an atmosphere's mean specific energy $\varepsilon_{\rm CGM} = E_{\rm CGM} / M_{\rm CGM}$ is
  $$\Delta \varepsilon_{\rm CGM} =  \left[ \varepsilon_{\rm kin} + \varphi(R_{\rm halo}) - \varepsilon_{\rm CGM} \right] \frac {\Delta M_{\rm CGM}} {M_{\rm CGM}} + \frac {\Delta E_{\varphi,{\rm cos}}} {M_{\rm CGM}}$$.

{: .note}
A cosmological atmosphere's structure remains roughly self-similar, with nearly constant $x_{\rm CGM}$, as long as $\Delta \varepsilon_{\rm CGM} / \varepsilon_{\rm CGM}  \approx \Delta v_\varphi^2 / v_\varphi^2$. When this condition holds, cosmological addition of atmospheric support energy offsets cosmological changes in the halo's gravitational potential well.
