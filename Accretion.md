---
title: Accretion
layout: default
nav_order: 2
parent: Documents
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

## Spherical Collapse

One of the energy sources responsible for both atmospheric heating and turbulence is cosmological accretion. Gas that enters a galaxy's atmosphere through cosmological accretion has a specific energy that is often estimated using a spherical collapse approximation. 

### Equation of Motion

The equation of motion for a spherical shell of radius $R$ containing a constant mass $M$ is
$$\ddot{R} = - \frac {G M} {R^2} + H_0^2 \Omega_\Lambda R$$
in an expanding universe with a Hubble constant $H_0$ at the present time and in which dark energy has a constant energy density ($3 H_0^2 / 8 \pi G) \Omega_\Lambda c^2$. Integrating the equation of motion gives the shell's specific kinetic energy at radius $R$,
$$\frac {\dot{R}^2} {2} = \frac {G M} {R} \left( 1 - \frac {R} {R_{\rm ta}} \right) + \frac {H_0^2 \Omega_\Lambda} {2} R^2 \left( 1 - \frac {R_{\rm ta}^2} {R^2}  \right)$$
in which the shell's turnaround radius $R_{\rm ta}$ depends on the mass it contains and the time when it accretes onto the galaxy's cosmological halo. According to this approximation, a shell of matter accreting onto a cosmological halo of mass $M_{\rm halo}(t)$ and radius $R_{\rm halo}$ at time $t$ has a specific kinetic energy
$$\varepsilon_{\rm kin} (t) = \frac {G M_{\rm halo} (t)} {R_{\rm halo}} \left[ 1 - \frac {R_{\rm halo}} {R_{\rm ta}} - \left( \frac {R_{\rm ta}^3} {R_{\rm halo}^3} - 1 \right) \left( \frac {3 H_0^2 \Omega_\Lambda} {8 \pi G \rho_{\rm halo}} \right) \right]$$
in which $\rho_{\rm halo} (t) \equiv 3 M(t) / 4 \pi R_{\rm halo}^3$ is the halo's mean matter density. 

### Halo Radius

A real cosmological halo does not have a distinct radius. Cosmological simulations show that gravitational scattering converts radial infall into randomly oriented orbital motion over a range of radii in the vicinity of $R_{\rm ta} / 2$. Consequently, there is some freedom to define $R_{\rm halo}$ so that it suits the situation of interest. It is often defined in terms of a contrast factor
$$\Delta_{\rm c} = \frac {\rho_{\rm halo}} {\rho_{\rm cr}}$$
relative to the universe's critical density $\rho_{\rm cr} = 3 H^2 (t) / 8 \pi G$, in which $H(t)$ is the Hubble expansion parameter at time $t$. For all suitable definitions of $\Delta_{\rm c}$, the factor
$$\frac {3 H_0^2 \Omega_\Lambda} {8 \pi G \rho_{\rm halo}} = \frac {\Omega_\Lambda} {\Delta_{\rm c}} \frac {H_0^2} {H^2 (t)}$$
is less than one percent. Neglecting that factor and assuming $R_{\rm halo} \approx R_{\rm ta} / 2$ then gives the approximation
$$\varepsilon_{\rm kin} (t) \approx \frac {G M_{\rm halo}(t)} {2 R_{\rm halo}}$$
for the specific kinetic energy of infalling matter at time $t$.

## Adiabatic Compression

Another cosmological source of atmospheric heating is adiabatic gravitational compression. Mass that falls through a radius $r$ and remains at smaller radii increases the halo's circular velocity at $r$. None of the infalling matter needs to be baryonic but may consist of baryonic mass in stars. The increase in gravitational force at $r$ compresses the atmospheric gas within $r$, thereby increasing both its temperature and turbulent velocity dispersion, if no support energy is added to offset compression.

To assess the contribution that adiabatic compression makes to the total atmospheric energy ($E_{\rm CGM}$), consider the result of a change $\Delta \varphi$ in the gravitational potential without a compensating change in the atmosphere's pressure, temperature, or density profiles. The total gravitational energy rises by
$$\Delta E_\varphi = \int_0^{r_{\rm CGM}} \Delta \varphi \cdot 4 \pi r^2 \rho dr$$
while $E_{\rm th}$, $E_{\rm nt}$, and $M_{\rm CGM}$ stay the same (see [Essentials](Essentials) for definitions of symbols). If the change in $\varphi$ comes from an increase of the total mass within $r_{\rm CGM}$, then it will raise the circular velocity there and at smaller radii. Therefore, the change in $E_{\rm CGM} / M_{\rm CGM} v_\varphi^2$ is 
$$\Delta \left( \frac {E_{\rm CGM}} {M_{\rm CGM} v_\varphi^2} \right) = \left[ \frac {\Delta E_\varphi} {E_\varphi} \frac {E_\varphi} {E_{\rm CGM}} - \frac {\Delta v_\varphi^2} {v_\varphi^2} \right] \frac {E_{\rm CGM}} {M_{\rm CGM} v_\varphi^2}$$
in which $v_\varphi$ is the normalization of the circular velocity profile.

A simple renormalization of the gravitational potential, proportional to $v_\varphi^2$, causes a decrease in $E_{\rm CGM} / M_{\rm CGM} v_\varphi^2$. That happens because $\Delta E_\varphi / E_\varphi = \Delta v_\varphi^2 / v_\varphi^2$ and $E_\varphi / E_{\rm CGM} < 1$. A change in both the normalization $v_\varphi$ and scale radius $r_{\rm s}$ of the potential well is a slightly more complicated case, giving
$$\Delta \varphi (x) = \varphi (x) \, \frac {\Delta v_\varphi^2} {v_\varphi^2} - v_c^2(x) \frac {\Delta r_{\rm s}} {r_{\rm s}}$$
for the change in $\varphi(x)$ at $x = r / r_{\rm s}$. In that case one finds 
$$\frac {\Delta E_\varphi} {E_\varphi}  = \frac {\Delta v_\varphi^2} {v_\varphi^2} - \frac {\Delta r_{\rm s}} {r_{\rm s}} \int_0^{x_{\rm CGM}} \frac {\alpha (x) f_{\alpha} (x)} {J_\varphi(x_{\rm CGM}) f_{\rm th}(x)} x^2 dx$$ when $\Delta \varphi$ is inserted into the integral for the change in $E_\varphi$. Therefore, increasing both $v_\varphi$ and $r_{\rm s}$ causes an even greater decrease in $E_{\rm CGM} / M_{\rm CGM} v_\varphi^2$ than just an increase of $v_\varphi$. 

In the **ExpCGM** framework, a decrease in $E_{\rm CGM} / M_{\rm CGM} v_\varphi^2$ leads to a decrease in the atmosphere's equilibrium radius $x_{\rm CGM} = r_{\rm CGM} / r_{\rm s}$. The increase in the gravitational potential temporarily causes gravitational forces to exceed pressure forces. Adiabatic compression of the atmosphere then raises its temperature by
$$\Delta T (r) \approx \frac {\Delta v_\varphi^2} {v_\varphi^2} T (r)$$
so that support energy and gravity come back into force balance. 

Thermal and turbulent energy input associated with the mass accretion that deepens the potential well has little effect on this rise in equilibrium temperature. Instead, the energy that accreting gas adds directly to the atmosphere acts to offset adiabatic contraction of the atmosphere by increasing $E_{\rm CGM} / M_{\rm CGM} v_\varphi^2$ and $x_{\rm CGM}$ while preserving the temperature increase linked to $\Delta v_\varphi^2$.

## Incoming Momentum

Baryonic gas accreting onto a galactic atmosphere does more than just add energy. It also applies some pressure. Incoming gas that is initially cold passes through an accretion shock at the atmosphere's outer edge. In an idealized, spherically symmetric accretion flow, the inward momentum flux of accreting gas at the shock's radius $R_{\rm acc}$ is

<p>
$$( \rho v^2 )_{\rm acc} = \frac {\dot{M}_{\rm acc} (R_{\rm acc})} { 4 \pi R_{\rm acc}^2 v_{\rm acc} ( R_{\rm acc} )}$$
</p>


where $\dot{M}\_{\rm acc} = \zeta f_{\rm b} \dot{M}\_{\rm halo}$ is the gas mass accretion rate, $\dot{M}\_{\rm halo}$ is the total mass accretion rate, $f_{\rm b}$ is the cosmic baryonic mass fraction, $\zeta \leq 1$ is a ``preventative feedback" factor accounting for phenomena that reduce the ratio of gas mass accretion to total mass accretion, and $v_{\rm acc} =  v_c (R_{\rm acc}) \cdot ( 1 - R_{\rm acc} / R_{\rm ta} )^{1/2}$ is the accretion speed of a mass shell that turned around at radius $R_{\rm ta}$. Momentum conservation requires that $P + \rho v^2$ be constant across the shock front (in the frame of that front), meaning that there is a boundary condition to be applied to the atmosphere's pressure profile. 

The difficulty with applying that boundary condition is cosmological accretion's lack of spherical symmetry. Much of the gas entering a galaxy's atmosphere streams in along cosmological filaments or plunges in along with merging subhalos. The gas associated with filaments and subhalos tends to be much denser than the ambient gas of a halo's outer atmosphere and is unimpeded by it. Meanwhile, ambient atmospheric gas that is rising outward can proceed along pathways posing less resistance. Nevertheless, the accreting gas must slow down somewhere in the atmosphere by shedding its infalling momentum and exerting an inward force.

In the spherically symmetric **ExpCGM** framework, the effects of incoming momentum on the pressure gradient can be included by adding an accretion term to the force balance equation, giving

<p>
$$\frac {d} {dr} \frac {P} {f_{\rm th}} = - \, \rho \frac {d \varphi} {dr} - \frac {v_{\rm acc}} {4 \pi r^2} \frac {d \dot{M}_{\rm acc}} {dr}$$
</p>
    

in which $\dot{M}\_{\rm acc}$ is an inward gas accretion rate that depends on radius and $v_{\rm acc}$ is the infall speed of that gas. The gradient of the inward mass flux assists gravitational confinement of the atmosphere and can be represented in terms of the standard **ExpCGM** force balance equation
$$\frac {d} {dr} \frac {P} {f_{\rm th}} = - \, \rho f_\varphi \frac {d \varphi} {dr}$$
using the modification factor

<p>
 $$f_\varphi  = 1 + \frac {(\rho v^2)_{\rm acc}} {\rho v_c^2} \frac {d \ln \dot{M}_{\rm acc}} {d \ln r}$$
</p>


Note that $\rho v_{\rm c}^2 = P_0 [ \alpha_{\rm eff} (r) f_\alpha (r) / f_\varphi f_{\rm th} (r)]$ in an equilibrium **ExpCGM** atmosphere.

Momentum transfer that happens within a narrow range of radii, as in a spherical accretion shock, leads to a sharp pressure jump, but momentum transfer spread over a broad range of radii has a less pronounced effect on the pressure gradient.  If the accreting gas deposits its momentum over a broad region, then the logarithmic derivative of $\dot{M}\_{\rm acc}$ in the expression for $f_\varphi$ will be of order unity. The magnitude of the accretion-dependent term then depends on how the inward mass flux $(\rho v^2)\_{\rm acc}$ at radius $r$ compares with the value of $\rho v_{\rm c}^2 = \alpha_{\rm eff} P / f_{\rm th}$ in the ambient atmosphere. The correction for incoming momentum is therefore more important in low-density atmospheres that have significantly expanded in response to central energy input than in uninflated atmospheres: High-density accreting gas penetrates more deeply into expanded atmospheres before slowing down, and its inward momentum flux is a larger fraction of $\rho v_{\rm c}^2$.

Implementation of this correction requires guidance from numerical simulations. What's needed is an approximate power-law dependence of the gas inflow rate $\dot{M}\_{\rm acc} = 4 \pi r^2 (\rho v)\_{\rm acc}$ on radius. Suppose that $r_{\rm dec}$ is the outer radius of the deceleration zone, that $\dot{M}\_{\rm acc} \propto r^\eta$ within $r_{\rm dec}$, and that $v_{\rm acc} \approx v_{\rm c}$ is approximately constant with radius. In that case, we find

<p>
$$f_\varphi \approx 1 + \eta \left[ \frac { \dot{M}_{\rm acc} (R_{\rm acc})} {4 \pi r_{\rm dec}^2 \rho v_{\rm c}} \right] \left( \frac {r} {r_{\rm dec}} \right)^\eta$$
</p>

in which the magnitude of the factor in square brackets is similar to the ratio of the gas mass entering $r_{\rm dec}$ during a dynamical time $r_{\rm dec} / v_{\rm c}$ to the gas mass currently within $r_{\rm dec}$. If the atmosphere's state is quasi-steady, then $(\rho v^2)\_{\rm acc} \sim \rho v_{\rm c}^2$ corresponds to a situation in which ambient gas at $r_{\rm dec}$ is flowing outward at a speed similar to the inflow speed of accreting gas. A smaller correction to purely gravitational force balance corresponds to an outflow speed that is small compared with $v_{\rm c}$. 

If the effects of momentum transfer are locally significant compared to pressure support, then a choice must be made about how to represent the response of the atmosphere's pressure profile to the additional confinement. According to the default \textsc{ExpCGM} prescription, the shape function for support energy (i.e. $\alpha$ or $\alpha_{\rm eff}$) is chosen by the user. An approximate accretion-corrected temperature profile 

<p>
    $$T(r) \approx \frac {2 f_{\rm th} (r)} {\alpha_{\rm eff}(r)} \left[ 1 + \eta \frac { \dot{M}_{\rm acc} (R_{\rm acc})} {4 \pi r_{\rm dec}^2 \rho (r) v_{\rm c} (r)} \left( \frac {r} {r_{\rm dec}} \right)^\eta \right] T_\varphi (r)$$
</p>


follows from that choice, and $\rho (r)$ follows from $P(r)$ and $T(r)$. Some iteration may be needed to obtain a self-consistent solution for $T(r)$, because $\rho (r)$ is part of the correction. However, if the accreting gas decelerates over a narrow range of radii, then its confining effects on the pressure profile may better represented as a discontinuity in the pressure profile at $r_{\rm dec}$:

<p>
    $$\Delta \left( \frac {P} {f_{\rm th}} \right) \approx - \frac {\dot{M}_{\rm acc} v_{\rm acc}} { 4 \pi r_{\rm dec}^2}$$
</p>


The chosen shape function and potential well can continue to determine the product of $f_{\rm th}(r)$ and $T(r)$ outside of $r_{\rm dec}$, but the pressure normalization factor $P_0$ in the usual expressions for $P(r)$ and $\rho(r)$ drops at $r_{\rm dec}$ to a lower value

<p>
    $$P_0 \left[ 1 - \frac {\dot{M}_{\rm acc} v_{\rm acc}} {4 \pi r_{\rm dec}^2 P (r_{\rm acc})} \right]$$
</p>


consistent with the pressure jump at $r_{\rm dec}$.

## Cosmological Evolution

To summarize how accretion of both gas mass and total mass affects the cosmological evolution of a galactic atmosphere:

* **Spherical Collapse.**
Cosmological structure formation brings gas into a halo of mass $M_{\rm halo}$ and radius $R_{\rm halo}$ with a specific kinetic energy 
$$\varepsilon_{\rm kin} \approx \frac {G M_{\rm halo}} {2 R_{\rm halo}} = \frac {v_{\rm c}^2 (R_{\rm halo})} {2}$$
Users of the \textsc{ExpCGM} framework are free to choose the density contrast factor $\Delta_{\rm c}$ that defines $R_{\rm halo}$ and relates it to the turnaround radius $R_{\rm ta}$ of matter that accretes through $R_{\rm halo}$ at time $t$. Once that choice has been made, users can apply the more precise expression
$$\varepsilon_{\rm kin} (t) = \frac {G M(t)} {R_{\rm halo}} \left[ 1 - \frac {R_{\rm halo}} {R_{\rm ta}} - \left( \frac {R_{\rm ta}^3} {R_{\rm halo}^3} - 1 \right) \left( \frac {3 H_0^2 \Omega_\Lambda} {8 \pi G \rho_{\rm halo}} \right) \right]$$
for the specific kinetic energy of accreting gas. The framework does not account for any thermal energy the accreting gas might have prior to passing within $R_{\rm halo}$.

* **Adiabatic Compression.**
Changes in the gravitational potential of a cosmological halo change the atmosphere's total energy $E_{\rm CGM}$ by the amount
$$\Delta E_\varphi = \int_0^{r_{\rm CGM}} \Delta \varphi \cdot 4 \pi r^2 \rho dr$$
Simply increasing the potential's normalization factor $v_\varphi^2$ by the fraction $\Delta v_\varphi^2 / v_\varphi^2$ results in a fractional increase $\Delta E_\varphi / E_{\rm CGM}$ in total atmospheric energy that is smaller than $\Delta v_\varphi^2 / v_\varphi^2$, meaning that the atmosphere becomes more strongly bound. If no atmospheric support energy is added along with the accreting mass, the increase in gravitational binding adiabatically compresses the atmosphere, causing a temperature rise proportional to $\Delta v_\varphi^2$. Adding some support energy does not offset this temperature increase. Instead, it causes atmospheric expansion that is approximately isothermal, because the product of $f_{\rm th}$ and $T$ depends far more directly on $v_\varphi^2$ than on $E_{\rm CGM}/M_{\rm CGM}$.

* **Incoming Momentum.**
Accreting gas might not transfer much of its kinetic energy to the atmosphere until reaching a deceleration radius $r_{\rm dec}$ significantly smaller than $R_{\rm halo}$. Within $r_{\rm dec}$, momentum transfer to the atmosphere as accreting gas decelerates assists gravitational confinement. The effects of momentum transfer on the atmosphere's pressure and density structure depend on: (1) how the mean inward momentum flux $(\rho v)_{\rm acc}$ near $r_{\rm dec}$ compares with $\rho v_{\rm c}^2 = \alpha P / f_\varphi f_{\rm th}$ in the ambient atmosphere at that radius, and (2) the radial width of the deceleration zone.

* **Net Energy Change.**
The net change in atmospheric specific energy stemming from cosmological accretion during a time interval $\Delta t$ is
$$\Delta E_{\rm acc} = \left[ \varepsilon_{\rm kin}  + \varphi(R_{\rm halo}) \right] \Delta M_{\rm CGM} + \Delta E_\varphi$$
where $\Delta M_{\rm CGM} = \zeta f_{\rm b} \dot{M}\_{\rm halo} \, \Delta t$ is the gas mass added through accretion. Without radiative losses that reduce $E_{\rm CGM}$ or feedback energy that increases $E_{\rm CGM}$, the net effect of cosmological accretion on an atmosphere's mean specific energy $\varepsilon_{\rm CGM} = E_{\rm CGM} / M_{\rm CGM}$ is
$$\Delta \varepsilon_{\rm CGM} =  \left[ \varepsilon_{\rm kin} + \varphi(R_{\rm halo}) - \varepsilon_{\rm CGM} \right] \frac {\Delta M_{\rm CGM}} {M_{\rm CGM}} + \frac {\Delta E_\varphi} {M_{\rm CGM}}$$
A cosmological atmosphere's structure will remain roughly self-similar (i.e. constant $x_{\rm CGM}$) as long as $\Delta \varepsilon_{\rm CGM} / \varepsilon_{\rm CGM}  \approx \Delta v_\varphi^2 / v_\varphi^2$.}
