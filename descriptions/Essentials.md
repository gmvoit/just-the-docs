---
title: Essentials
layout: default
nav_order: 1
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

# Essentials
{: .no_toc}

This page outlines the essential elements of the **ExpCGM** framework, starting with force balance and atmospheric specific energy. It then provides a simple example illustrating how to produce an **ExpCGM** model atmosphere. Two sections following the example describe how **ExpCGM** accounts for turbulent support and thermalization of atmospheric energy. The page concludes with a section outlining how **ExpCGM** can be used to model atmospheric evolution.

<details closed markdown="block">
  <summary>
    Table of contents
  </summary>
   {: .text-delta}
- TOC
{:toc}  
</details>

## Force Balance 

The core of **ExpCGM** is an efficient method for determining the steady-state configuration of a galactic atmosphere having a total gas mass $M_{\rm CGM}$ and a total energy $E_{\rm CGM}$. In this framework, the equilbrium radius $r_{\rm CGM}$ containing the atmospheric gas mass $M_{\rm CGM}$ is a function of $E_{\rm CGM}$. 

### Hydrostatic Equilibrium

The most basic example of a such a steady-state atmospheric configuration is hydrostatic equilibrium in a spherically symmetric potential well, as described by 
$$\frac {dP} {dr} = -  \rho \frac {d \varphi} {dr}$$ 
where $P$ is thermal pressure, $\rho$ is gas density, and $\varphi$ is the gravitational potential.

### Pressure Shape Function

Direct integration of the hydrostatic equilbrium equation is possible if both the gravitational potential $\varphi (r)$ and the pressure profile's ***shape function***
$$\alpha (r) \equiv - \frac {d \ln P} {d \ln r}$$
are known functions of radius. Integrating the shape function leads to
$$f_P(r) \equiv \exp \left[ - \int_1^{r/r_0} \alpha (x)  dx \right]$$ 
which is a dimensionless pressure profile normalized to unity at a reference radius $r_0$. Providing a pressure normalization $P_0$ at $r_0$ then specifies the atmosphere's equilibrium pressure profile:
$$P(r) = P_0 f_P(r)$$

{: .note}
Many details of an **ExpCGM** atmosphere model hinge on a user's choice for the pressure shape function $\alpha(r)$. That choice represents an assumption about the physical processes responsible for maintaining the pressure profile. See the [Pressure Profiles](PressureProfiles) page for more detail.

### Temperature Profile

Solving the hydrostatic equilibrium equation for $T(r)$ yields the atmosphere's temperature profile:
$$T(r) = \frac {2 T_\varphi (r) } {\alpha (r)}$$
Here, the function $T_\varphi (r) \equiv \mu m_p v_{\rm c}^2(r) / 2k$ represents the ***gravitational temperature profile*** for an atmosphere with a mean mass per particle $\mu m_p$ confined by a gravitational potential with a circular velocity profile $v_{\rm c} (r) = ( r d \varphi / dr)^{1/2}$. It is normalized so that $T(r) = T_\varphi (r)$ for $\alpha = 2$ because $P \propto r^{-2}$ is the equilibrium pressure profile of an isothermal atmosphere in a potential well with constant $v_{\rm c}$.

{: .note}
The astronomical literature often calls something like $T_\varphi$ a *virial temperature*, but it's not really an outcome of the virial theorem. Also, a galactic atmosphere can satisfy the virial theorem without having converted all of its kinetic energy into thermal energy. That is why **ExpCGM** calls $T_\varphi$ a *gravitational temperature*.

### Density Profile

Given those pressure and temperature profiles, an equilibrium atmosphere's density profile becomes
$$\rho (r) = P_0 \frac {\alpha (r) f_P (r)} {v_{\rm c}^2(r)}$$
For example, the gas mass density at the reference radius $r_0$ is simply $\rho_0 = P_0 / (v_c^2 / 2)$ in an equilibrium atmosphere with $\alpha = 2$.

### Generalized Force Balance

To represent equilibrium atmospheres partially supported by non-thermal energy, the **ExpCGM** framework generalizes hydrostatic equilibrium using the force balance equation
$$\frac {d} {dr} \frac {P} {f_{\rm th}} = - \rho f_\varphi  \frac {d \varphi} {dr}$$
The ***thermalization fraction*** $f_{\rm th}$ represents the proportion that thermal pressure contributes to the total support opposing gravity. The ***force modification factor*** $f_\varphi$ accounts for phenomena (such as rotation) that alter the effective gravitational force pulling the atmosphere inward. 

With those generalizations, an atmosphere's equilibrium temperature and density profiles become 
$$T(r) = \frac {2 f_{\rm th} f_\varphi} {\alpha_{\rm eff} (r)} ~T_\varphi (r)~~~~~~,~~~~~~\rho (r) = P_0 \frac {\alpha_{\rm eff} (r) f_P (r)}{f_{\rm th} f_\varphi v_{\rm c}^2(r)}$$ 
in which the function 
$$\alpha_{\rm eff} (r) \equiv \alpha(r) + \frac {d \ln f_{\rm th}} {d \ln r}$$ 
is a ***generalized shape function*** for atmospheric support, accounting for possible dependences of $f_{\rm th}$ on radius. It is also possible for $f_\varphi$ to depend on radius.

{: .note}
Some other formulations of atmospheric force balance express resistance to gravity in terms of a total pressure $P_{\rm tot} = P / f_{\rm th}$. However, **ExpCGM** explicitly accounts for the thermal pressure contribution using the $f_{\rm th}$ factor so that an equilibrium atmosphere's temperature profile can be directly inferred from just force balance considerations and the thermalization fraction $f_{\rm th}$.

## Specific Energy <a name="specific-energy-"></a>

A galactic atmosphere of total mass $M_{\rm CGM}$ expands if its total energy $E_{\rm CGM}$ increases, and it contracts if its total energy declines. The **ExpCGM** framework therefore links a galactic atmosphere's equilibrium radius $r_{\rm CGM}$ to its ***mean specific energy***
$$\varepsilon_{\rm CGM} = \frac {E_{\rm CGM}} {M_{\rm CGM}}$$
The dependence of specific energy on radius can be inverted to obtain the dependence of radius on specific energy. However, several integrals are needed to derive $r_{\rm CGM} ( \varepsilon_{\rm CGM} )$ from the equilibrium profiles of pressure, temperature, and density. 

### Gas Mass

Integrating $\rho(r)$ in a potential well with maximum circular velocity $v_\varphi \equiv \max (v_{\rm c})$ gives the atmospheric gas mass profile
$$M_{\rm CGM} (r) = \frac {4 \pi r_0^3 P_0} {v_\varphi^2} I \left( \frac {r} {r_0} \right) ~~~~~~,~~~~~~I (x) \equiv v_\varphi^2 \int_0^{r/r_0} \frac {\alpha_{\rm eff} (x) f_P (x)} { f_{\rm th} f_\varphi v_{\rm c}^2 (x) } x^2 dx$$

### Gravitational Energy

Integrating $\varphi (r) \rho (r)$ gives the cumulative gravitational energy profile
$$E_\varphi (r) = 4 \pi r_0^3 P_0 J_\varphi \left( \frac {r} {r_0} \right) ~~~~~~,~~~~~~J_\varphi (x) \equiv \int_0^{r/r_0} \frac {\alpha_{\rm eff} (x) f_P (x) \varphi(x)} { f_{\rm th} f_\varphi v_{\rm c}^2 (x) }  x^2 dx $$

### Thermal Energy 

Integrating $3 P(r)/2$ gives the cumulative thermal energy profile
$$E_{\rm th} (r) = 4 \pi r_0^3 P_0  J_{\rm th} \left( \frac {r} {r_0} \right) ~~~~~~,~~~~~~J_{\rm th} (x) \equiv \frac {3} {2} \int_0^x f_P (x) x^2 dx$$

### Non-Thermal Energy

If there are non-thermal pressure components, the cumulative non-thermal support energy profile is
$$E_{\rm nt} (r) = 4 \pi r_0^3 P_0 J_{\rm nt} \left( \frac {r} {r_0} \right) ~~~~~~,~~~~~~J_{\rm nt} (x) \equiv \int_0^x \frac {(1 - f_{\rm th}) f_P (x)} {f_{\rm th} (\gamma_{\rm nt} - 1)} x^2  dx$$
The factor $\gamma_{\rm nt} - 1$ in the integral's denominator is the local ratio of non-thermal pressure support to non-thermal energy density, which can be different from 2/3. 

### Total Specific Energy

To obtain a galactic atmosphere's total specific energy, the **ExpCGM** framework adds up the energy integrals to make the function 
$$F(x) = \frac { J_\varphi (x) + J_{\rm th}(x) + J_{\rm nt}(x)} {I(x)}$$
It is defined so that
$$\varepsilon_{\rm CGM} = v_\varphi^2 F \left( \frac {r_{\rm CGM}} {r_0} \right)$$
relates an atmosphere's mean specific energy $\varepsilon_{\rm CGM}$ to its equilibrium radius $r_{\rm CGM}$. 

### Equilibrium Radius

Inverting $F(x)$ then gives the atmosphere's equilibrium radius $r_{\rm CGM} = x_{\rm CGM} r_0$ as a function of $\varepsilon_{\rm CGM} / v_\varphi^2$:
$$r_{\rm CGM} = r_0 F^{-1} \left( \frac {\varepsilon_{\rm CGM}} {v_\varphi^2} \right)$$

### Pressure Normalization

Once an equilibrium radius has been determined, the relation
$$P_0 = \frac {M_{\rm CGM} v_\varphi^2} {4 \pi r_0^3} \frac {1} {I(r_{\rm CGM}/r_0)}$$
gives the pressure profile's normalization at $r_0$.

{:.note}
These relationships between $\varepsilon_{\rm CGM}$, $r_{\rm CGM}$, and $P_0$ are the cornerstones of all **ExpCGM** atmosphere models. They depend only on a user's choices for $\varphi(r)$ and $\alpha(r)$ and determine how the resulting atmosphere expands or contracts as its total mass and energy change.

## A Simple Example <a name="ASimpleExample"></a>

To illustrate how solutions for steady-state atmospheric structure emerge from the **ExpCGM** framework, the following example simplifies some things. 

### Power-Law Pressure Profile

First, assume that the atmosphere has a power-law pressure profile (constant $\alpha$), that the atmosphere's support energy is purely thermal ($f_{\rm th} =1$), and that there are no radial forces other than gravity ($f_\varphi =1$). In that case, its equilibrium pressure, temperature, and density profiles become
$$P(r) = P_0 \left( \frac {r} {r_0} \right)^{-\alpha}~~~~,~~~~T(r) = \frac {2 T_\varphi (r)} {\alpha} ~~~~,~~~~\rho (r) = \frac {\alpha P_0} {v_{\rm c}^2(r)} \left( \frac {r} {r_0} \right)^{-\alpha}$$

### NFW Potential Well

Next, consider confinement by an Navarro-Frenk-White (NFW) gravitational potential with a zero point at $r=0$. We can express that potential as
$$\varphi_{\rm NFW} (x) = A_{\rm NFW} v_\varphi^2 \left[ 1 - \frac {\ln (1 + x)} {x} \right]$$
where $r_{\rm s}$ is the potential's ***scale radius*** and $x = r / r_{\rm s}$.
The parameter $v_\varphi^2$ is equal to the maximum value of the NFW circular velocity profile
$$v_{\rm c}^2 (x) = A_{\rm NFW} v_\varphi^2 \left[ \frac {\ln (1 + x)} {x}  - \frac {1} {1 + x} \right]$$
for $A_{\rm NFW} = 4.625$. For convenience, we will choose $r_0$ to be identical to $r_{\rm s}$, so that $P_0$ is the thermal pressure at $r_{\rm s}$.

### Energy and Mass Integrals

With those choices, the integrals needed for determining the atmosphere's equilibrium radius become much simpler. The integral giving the dimensionless gas mass profile simplifies to 
$$I(x) = \frac {\alpha} {A_{\rm NFW}} \int_0^x \left[ \frac {x} {\ln (1+x) - x / (1 + x) } \right] x^{2 - \alpha} dx$$
The integral giving the cumulative gravitational energy profile simplifies to         
$$J_\varphi(x) = \alpha \int_0^x \left[ \frac {x - \ln (1 + x)} {\ln (1+x) - x / (1 + x)} \right] x^{2 - \alpha} dx$$
The cumulative thermal energy profile becomes an algebraic function: 
$$J_{\rm th}(x) = \frac {3 x^{3-\alpha}} {6 - 2\alpha}$$
All of the support energy is thermal, and so there is no need to compute $J_{\rm nt} (x)$.

### Energy and Expansion

The figure below shows the relationship between atmospheric radius and specific energy that follows from these integrals. A solid red line represents how an atmosphere's equilibrium radius $r_{\rm CGM} = x_{\rm CGM} r_{\rm s}$ depends on its mean specific energy $\varepsilon_{\rm CGM} = E_{\rm CGM} / M_{\rm CGM}$ for a case with $\alpha = 3/2$. The equilibrium radius starts at zero for $\varepsilon_{\rm CGM} = 0$ and rises through $r_{\rm CGM} \approx 6 r_{\rm s}$ at $\varepsilon_{\rm CGM} \approx 3 v_\varphi^2$. The atmosphere's radius then formally approaches infinity as $\varepsilon_{\rm CGM}$ approaches $A_{\rm NFW} v_\varphi^2$ because atmospheric gas with specific energy exceeding $A_{\rm NFW} v_\varphi^2$ is unbound. A dashed blue line shows how the normalization $P_0 \propto 1/ I(x)_{\rm CGM}$ of the atmosphere's pressure profile declines as the atmosphere expands.

<figure>
    <img src="../epsCGM_xCGM_plot.jpg"
         alt="epsCGM_xCGM_plot">
</figure>

Dotted lines in the figure illustrate relationships that are exponentially sensitive to the scaled specific energy $\varepsilon_{\rm CGM} / v_\varphi^2$. They are similar to the solid and dashed lines within the range $v_\varphi^2 \lesssim \varepsilon_{\rm CGM} \lesssim 4 v_\varphi^2$. However, the functions describing the atmosphere's equilibrium radius and pressure normalization become even more sensitive than the exponential relationships as $\varepsilon_{\rm CGM}$ surpasses $4 v_\varphi^2$. Consequently, feedback mechanisms capable of further raising the atmosphere's specific energy drastically lower its density and radiative cooling rate.

{:.note}
The relationship between $r_{\rm CGM}$ and $\varepsilon_{\rm CGM}$ would be purely exponential for an atmosphere with constant $\alpha$ in a gravitational potential with constant $v_{\rm c}$. The relationships in the figure are consequently *nearly* exponential in the portion of an NFW potential well in which $v_{\rm c}$ is *nearly* constant. 

## Turbulent Support <a name="TurbulentSupport"></a>

The **ExpCGM** framework is designed to model galactic atmospheres supported by a combination of thermal energy and non-thermal gas motions that are usually called "turbulence" even though they do not necessarily arise from a classic Kolmogorov cascade of eddies. For simplicity, **ExpCGM** treats those gas motions as isotropic, with a one-dimensional velocity dispersion $\sigma_{\rm 1D}$, so that the thermalization fraction is
$$f_{\rm th} = \frac {P} {P + \rho \sigma_{\rm 1D}^2}$$
Conveniently, the ratio of turbulent energy density to turbulent pressure support is the same as the thermal ratio, corresponding to $\gamma_{\rm nt} = 5/3$. Summing the cumulative thermal and turbulent energy profiles gives
$$4 \pi r_0^3 P_0 \left[ J_{\rm th} (x) + J_{\rm nt} (x) \right] =  4 \pi r_0^3 \int_0^x \frac {3} {2} \frac {P (x)} {f_{\rm th}(x)} x^2 dx$$
The equilibrium radius of an atmosphere jointly supported by thermal and turbulent energy therefore depends only on the total density of support energy, $3P/2 f_{\rm th}$, not the separate proportions of thermal and turbulent energy. That is because the equilibrium density profile
$$\rho (r) = \frac {P_0 f_P (r)} {f_{\rm th}(r)} \frac {\alpha_{\rm eff}(r)} {v_{\rm c}^2 (r)}$$
is identical to density profile of a purely hydrostatic atmosphere with $\alpha (r) = \alpha_{\rm eff} (r)$ and $f_{\rm th} = 1$.

## Thermalization <a name="Thermalization"></a>

Turbulence dissipates into heat on a timescale $t_{\rm diss} \sim \lambda_{\rm d} / \sigma_{\rm 1D}$, in which $\lambda_{\rm d}$ is a length scale characterizing the driving of turbulence. Meanwhile, collisional excitation of photons converts thermal energy into radiative energy on a timescale $t_{\rm cool}$. An atmosphere's thermalization fraction $f_{\rm th}$ therefore depends on how $t_{\rm diss}$ compares with $t_{\rm cool}$. 

The previous section's results show that dissipation of turbulent support energy does not change the atmosphere's equilibrium radius or density profile. Turbulent dissipation simply changes $f_{\rm th}$ without altering an equilibrium atmosphere's overall structure. 

### Energy Injection

To track how thermalization of turbulence determines the value of $f_{\rm th}$ in an atmosphere jointly supported by turbulence and thermal energy, the **ExpCGM** framework defines $\dot{E}\_{\rm inj}$ to be the rate at which energy sources inject energy into a galaxy's atmosphere and accounts for the proportions going into thermal and turbulent energy. 

The timescale $t_{\rm inj} \equiv E_{\rm th} / f_{\rm th} \dot{E}\_{\rm inj}$ for energy input is the time it takes for those sources to inject an amount of energy $\dot{E}\_{\rm inj} t_{\rm inj}$ comparable to the total support energy opposing gravitational compression, which is equal to the thermal energy $E_{\rm th}$ divided by $f_{\rm th}$. That energy input branches into a fraction $f_{\rm inj,th}$ going directly into heat and a complementary fraction $1 - f_{\rm inj,th}$ that initially goes into turbulence. Eventually the turbulence dissipates into heat on the timescale $t_{\rm diss} \equiv (1 - f_{\rm th}) E_{\rm th} / f_{\rm th} \dot{E}\_{\rm diss}$, where $\dot{E}\_{\rm diss}$ is the dissipation rate. Radiative cooling then converts that thermal energy into escaping photons on a timescale $t_{\rm cool} \equiv E_{\rm th} / \dot{E}\_{\rm rad}$, where $\dot{E}_{\rm rad}$ is the radiative loss rate.

Energy input, dissipation, and radiative cooling can all change the fraction $f_{\rm th}$ of support energy in thermal form. Its rate of change can be derived from the overall rate of change in total support energy

<p>$$\frac {d} {dt} \frac {E_{\rm th}} {f_{\rm th}} = \dot{E}_{\rm inj} - \dot{E}_{\rm rad} - \dot{E}_{\rm exp}$$</p>

in which $\dot{E}\_{\rm exp}$ is the rate at which atmospheric expansion or contraction converts support energy into gravitational energy or vice versa. The rate of change of just the thermal component is

<p>$$\dot{E}_{\rm th} = \dot{E}_{\rm diss} - \dot{E}_{\rm rad} - f_{\rm th} \dot{E}_{\rm exp} + f_{\rm inj,th} \dot{E}_{\rm inj}$$</p>

The factor $f_{\rm th}$ in front of $\dot{E}\_{\rm exp}$ follows from turbulent energy having the same value of $\gamma - 1$ as thermal energy. 

### Evolution of Thermalization

Combining the equations for the rate of change in $E_{\rm th}$ and the rate of change in $E_{\rm th} / f_{\rm th}$ leads to

<p>$$\frac {E_{\rm th}} {f_{\rm th}} \frac {d f_{\rm th}} {dt} = \dot{E}_{\rm diss} - \left( 1 - f_{\rm th} \right) \dot{E}_{\rm rad} + \left( f_{\rm inj,th} - f_{\rm th} \right) \dot{E}_{\rm inj}$$</p>

The time derivative of $f_{\rm th}$ can therefore be expressed as

<p>$$\frac {d f_{\rm th}} {dt} = \frac {1 - f_{\rm th}} {t_{\rm diss}} - \frac {(1 - f_{\rm th}) f_{\rm th}} {t_{\rm cool}} + \frac {(f_{\rm inj,th} - f_{\rm th})} {t_{\rm inj}}$$</p>

which explicitly connects changes in $f_{\rm th}$ to the three timescales for energy exchange.

{: .important}
This equation for how thermalization changes with time does not depend on the volume of gas being considered. It can be applied to a global value of $f_{\rm th}$ characterizing the entire atmosphere. It can also be applied to a thin atmospheric shell of radius $r$. 

## Atmospheric Evolution <a name="AtmosphericEvolution"></a>

Evolution of a galactic atmosphere in the **ExpCGM** framework proceeds through a series of equilibrium states. They are the states the atmosphere would settle into on a dynamical timescale ($\sim r / v_c$) in the absence of heating or cooling. A series of those states is therefore a valid approximation for atmospheric evolution as long as the timescales for energy injection ($t_{\rm inj}$) and radiative cooling ($t_{\rm cool}$) are not short compared to the atmosphere's dynamical time at the radii of interest.

An **ExpCGM** atmosphere model provides the following information useful for computing $t_{\rm inj}$ and $t_{\rm cool}$.

### Radiative Cooling 

A galactic atmosphere loses thermal energy through two-body collisions that produce photons. The cooling rate per unit volume is therefore proportional to $\rho^2$ and a temperature-dependent cooling function $\Lambda_\rho (T)$ that accounts for the atmosphere's ionization state, the speeds of colliding particles, and the cross-sections for excitation of photon emission. At radius $r$, the atmosphere loses thermal energy on a timescale
$$t_{\rm cool} = \frac {3 k T} {2 \rho \Lambda_\rho (T)}$$ 
computable using an **ExpCGM** model that gives $T(r)$ and $\rho(r)$ as functions of $\varepsilon_{\rm CGM}$ and $M_{\rm CGM}$. (See the [Cooling](Cooling) page for more detail.)

### Galactic Feedback

A galactic atmosphere gains support energy when feedback energy released from the central galaxy propagates outward. The time-averaged rate of energy ejection depends on how rapidly a galaxy's atmosphere can supply the galaxy with fuel for star formation. It may also depend on how much of the galaxy's gas accretes onto its central black hole. According to the **ExpCGM** framework, the atmosphere supplies its central galaxy with gas at a rate no greater than the pure cooling-flow rate

<p> 
  $$\dot{M}_{\rm cool}(r_{\rm gal}) \approx \frac {4 \pi r_{\rm gal}^3 \, \rho (r_{\rm gal})} {t_{\rm cool} (r_{\rm gal})}$$ 
</p> 

evaluated at the galaxy's outer radius $r_{\rm gal}$. The inflowing mass is subtracted from $M_{\rm CGM}$, and its energy content is subtracted from $E_{\rm CGM}$. A supplementary galaxy model then determines the resulting supply of feedback energy $\dot{E}\_{\rm fb}$ and outflowing gas mass $\dot{M}\_{\rm fb}$. 

{: .note}
In the **ExpCGM** framework, radiative cooling can continue to supply gas to the galaxy even if $\dot{E}\_{\rm fb} > \dot{E}\_{\rm rad}$. Instead of shutting off the gas supply by offsetting radiative cooling, feedback energy limits $\dot{M}\_{\rm cool}$ by reducing $\rho (r_{\rm gal})/t_{\rm cool} (r_{\rm gal})$.

### Cosmic Accretion

Cosmological structure formation also supplies both mass and energy to the galaxy's atmosphere. The rate at which it adds energy to the atmosphere is

<p> 
  $$\dot{E}_{\rm acc} \approx \left[ \varphi(R_{\rm halo}) + \frac {v_{\rm c}^2} {2} (R_{\rm halo}) \right] \dot{M}_{\rm acc}$$ 
</p>

where $R_{\rm halo}$ is a user-defined radius for the galaxy's dark matter halo and $\dot{M}\_{\rm acc}$ is a cosmologically determined mass accretion rate. Also, cosmological structure formation can change both the maximum circular velocity ($v_\varphi$) and scale radius ($r_{\rm s}$) of the gravitational potential, thereby changing the gravitational potential energy of gas that has already accreted. That change is

<p> 
  $$\dot{E}_{\varphi,{\rm cos}} = 4 \pi \int_0^{r_{\rm CGM}} \dot{\varphi} (r) \rho(r) r^2 dr$$ 
</p>

and depends on both the rate of change $\dot{\varphi} (r)$ in the gravitational potential and the density profile supplied by an equilibrium model for the atmosphere.

### Total Energy Input

The total input rate for atmospheric energy is therefore

<p> 
  $$\dot{E}_{\rm inj} = \dot{E}_{\rm fb} + \dot{E}_{\rm acc} + \dot{E}_{\varphi,{\rm cos}}$$ 
</p>

Supplementary models can be implemented to determine how energy input changes $f_{\rm th}(r)$ and perhaps also $\alpha (r)$. To learn more about them, as well as options for the pressure profile's shape function $\alpha (r)$ and the gravitational potential correction factor $f_\varphi (r)$, interested users will need to consult other pages providing more detailed descriptions of the **ExpCGM** framework.
