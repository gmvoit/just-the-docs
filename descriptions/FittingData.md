---
title: Fitting Data
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

# Fitting Data
{: .no_toc}

This page outlines the input parameters for **ExpCGM** models and the output predictions for users wishing to fit those models to either observed or simulated galactic atmospheres.

{: .warning}
This page is still under construction.

<details closed markdown="block">
  <summary>
    Contents
  </summary>
   {: .text-delta}
- TOC
{:toc}  
</details>

## Input Parameters

All **ExpCGM** models for galactic atmospheres are based on a user-specified gravitational potential function $\varphi(\mathbf{r})$ and a user-specified shape function $\alpha(\mathbf{r})$ for the atmosphere's pressure profile. Both functions can depend on a three-dimensional position vector $\mathbf{r}$. However, spherically symmetric models depending only on $r = \| \mathbf{r} \|$ are often sufficient. This introductory page focuses on just spherically symmetric models.

Two parametric models represent the mininum input for generating an **ExpCGM** model atmosphere: 

* **Gravitational Potential,** $\varphi( ~r~ \| ~v_\varphi , r_{\rm s} , ... ~)$: The gravitational potential model requires only one parameter, the maximum circular velocity $v_\varphi$ of the confining halo's gravitational potential. It may also include a scale radius $r_{\rm s}$ determining how the potential's circular velocity $v_{\rm c}$ depends on $r$. Descriptions of more detailed potential wells may require additional parameters.

* **Shape Function,** $\alpha( ~r~ \| ~\alpha_{\rm in} , \alpha_{\rm out} , r_\alpha , ... ~)$: The atmosphere's shape function can be just a constant value of $\alpha$. It can also shift from one limiting value $\alpha_{\rm in}$ at small radii to another one $\alpha_{\rm out}$ at large radii in the vicinity of a crossover radius $r_\alpha$. The [Pressure Profiles](PressureProfiles) page discusses how $\alpha (r)$ is related to the processes that shape a galaxy's atmosphere and provides parametric expressions for several physically motivated options.

Once $\alpha(r)$ has been specified, the pressure profile of an **ExpCGM** atmosphere model depends on just the normalization factor $P_0$ at a user-chosen fiducial radius $r_0$. The atmosphere's pressure profile becomes
  $$P(r) = P_0 \cdot \exp \left[ - \int_1^{r/r_0} \frac {\alpha(x)} {x} dx \right]$$
Dividing $v_{\rm c}^2 (r)$ by $\alpha(r)$ gives the temperature profile for a thermally supported atmosphere: 
  $$kT(r) = \frac {\mu m_p v_{\rm c}^2(r)} {\alpha(r)}$$
The atmosphere's density profile then depends on just $\varphi(r)$, $\alpha(r)$, and $P_0$:
  $$\rho(r) = \frac {\alpha(r) P(r)} {v_{\rm c}^2(r)}$$
However, $T(r)$ and $\rho(r)$ may also depend on two additional parametric functions representing the atmosphere's thermalization fraction $f_{\rm th}$ and force modification factor $f_\varphi$ (see the [Essentials](Essentials) page for definitions).

### Isothermal Atmosphere

Users seeking to minimize degrees of freedom can opt for an isothermal potential well with constant circular velocity ($v_{\rm c} = v_\varphi$) confining an atmosphere with constant $\alpha$. Setting $f_{\rm th}$ and $f_\varphi$ to unity then gives an atmospheric temperature $kT = \mu m_p v_\varphi^2 / \alpha$ that is independent of radius and power-law profiles for pressure and density: 
  $$P(r) = P_0 \left( \frac {r} {r_0} \right)^{-\alpha} ~~~,~~~ \rho(r) = \frac {\alpha P_0} {v_\varphi^2} \left( \frac {r} {r_0} \right)^{-\alpha}$$
This *isothermal power-law atmosphere* model has three degrees of freedom $(P_0,v_\varphi,\alpha)$, because $r_0$ is degenerate with $P_0$. 

### Double Power-Law Atmospheres

Cosmological structure formation produces gaseous atmospheres in which $\alpha(r)$ increases with radius. An **ExpCGM** model can describe that increase with the four-parameter fitting formula
  $$\alpha(r) = \alpha_{\rm in} + ( \alpha_{\rm out} - \alpha_{\rm in} ) \left[ \frac {(r / r_\alpha)^{\alpha_{\rm tr}}} {1 + (r / r_\alpha)^{\alpha_{\rm tr}}} \right]$$
If all four parameters are left free, the resulting model for an atmosphere in an isothermal potential well has six degrees of freedom instead of three. Its pressure profile is 
  $$P(r) \propto \left( \frac {r} {r_\alpha} \right)^{-\alpha_{\rm in}} \left[ 1 + \left( \frac {r} {r_\alpha} \right)^{\alpha_{\rm tr}} \right]^{-(\alpha_{\rm out} - \alpha_{\rm in}) / {\alpha_{\rm tr}}}$$
and its gas temperature declines from $kT \approx \mu m_p v_\varphi^2 / \alpha_{\rm in}$ at small radii toward $kT \approx \mu m_p v_\varphi^2 / \alpha_{\rm out}$ at large radii as $\alpha (r)$ rises.

{: .note}
Restricting the double power-law atmosphere model by choosing $\alpha_{\rm in} = 0$ and $\alpha_{\rm tr} = 2$ results in 
  $$P(r) \propto \left[ 1 + \left( \frac {r} {r_\alpha} \right)^2 \right]^{-\alpha_{\rm out} / 2}$$ 
This relation is nearly equivalent to the classic "beta model" for galaxy-cluster atmospheres, but with thermal pressure replacing gas density. However, the model has a drawback: It results in a gas-temperature profile that diverges at small $r$ in an isothermal potential well with constant $v_\varphi$. This undesirable feature is less problematic in gravitational potentials with small values of $v_{\rm c}$ at small radii.

### NFW-like Models

Cosmological structure formation also produces halo potential wells that are not quite isothermal. Typically, the circular velocity profile of a cosmological halo rises with radius at small $r$ and declines with radius at large $r$. The most common fitting formula accounting for that feature is the Navarro-Frenk-White (NFW) model, which has a circular velocity profile
  $$v_{\rm NFW}^2(x) = A_{\rm NFW} v_\varphi^2 \left[ \frac {\ln (1 + x)} {x} - \frac {1} {1 + x} \right]$$
with $x =  r / r_{\rm s}$ and $A_{\rm NFW} = 4.625$. 

The [Essentials](Essentials) page presents a simple example with four degrees of freedom $(P_0,v_\varphi,r_{\rm s}, \alpha)$ describing a power-law atmosphere in an NFW potential well. Expanding that example using all four parameters of a double power-law atmosphere yields a model with seven degrees of freedom $(P_0,v_\varphi,r_{\rm s},\alpha_{\rm in},\alpha_{\rm out},\alpha_{\rm tr},r_\alpha)$. Users can reduce the dimensionality of the input parameter space by keeping $\alpha_{\rm tr}$ fixed and by choosing to make $r_\alpha$ a constant multiple of $r_{\rm s}$.
 
### NFW Halo + Central Galaxy

An NFW halo model coupled with a shape function model that has $\alpha_{\rm in} > 0$ results in an atmospheric temperature that formally approaches zero at small radii. Adding a central galaxy to the gravitational potential model helps to mitigate that potentially problematic issue.

One option is to use a ***Hernquist model*** to represent the galaxy's contribution to the potential's circular velocity profile:
  $$v_{\rm H}^2(r) =  \frac {G M_* r} { ( r + r_{\rm H})^2 }$$
In this expression, $M_\*$ represents the galaxy's total stellar mass and the ***Hernquist radius*** $r_{\rm H}$ is a scale radius determining how the galaxy's mass profile converges toward $M_{\rm H}$. 

The model parameters $M_\*$ and $r_{\rm H}$ can be free, or they can be fixed at values consistent with the observed stellar mass and effective radius of the halo's central galaxy. If both $M_\*$ and $r_{\rm H}$ are allowed to be free, then adding a central galaxy to the gravitational potential model adds two degrees of freedom to the input parameter space.

{: .note}
Most central galaxies have a maximum circular velocity $(G M_* / 4 r_{\rm H})^{1/2}$ similar to the maximum circular velocity $v_\varphi$ of the surrounding halo. It is therefore reasonable to reduce the dimensionality of the parameter space by applying the restriction $r_{\rm H} = G M_* / 4 v_\varphi^2$, so that $\max (v_{\rm H}) = v_\varphi$. However, applying that restriction is unwise for galaxy-cluster models, because the maximum circular velocity of a central cluster galaxy is significantly smaller than the maximum circular velocity of its halo. 

### NFW Halo + Central Galaxy + BH

A supermassive black hole may dominate the galaxy's gravitational potential at the smallest radii. Its contribution can be included in an **ExpCGM** model using the Newtonian formula
  $$v_{\rm BH}^2 (r) = \frac {G M_{\rm BH}} {r}$$
where $M_{\rm BH}$ is the central black hole's mass. Adding a black hole to the potential model so that
  $$v_{\rm c}^2 (r) = v_{\rm NFW}^2 (r) + v_{\rm H}^2 (r) + v_{\rm BH}^2 (r)$$
then ensures that the model atmosphere's temperature does not formally go to zero at the center.

### Thermalization Fraction

Observational constraints can be placed on an atmosphere's thermalization fraction $f_{\rm th}$ if the overall data set contains information complementary to the atmospheric temperature profile. 

An **ExpCGM** user interested in thermalization may choose to add a parametric model for $f_{\rm th}$ that depends on radius. Combining that model with $\alpha (r)$ gives
  $$\alpha_{\rm eff} (r) = \alpha (r) + \frac {d \ln f_{\rm th}} {d \ln r}$$
The predicted temperature profile then becomes
  $$kT(r) = \frac {\mu m_p v_{\rm c}^2 (r)} {\alpha_{\rm eff} (r)} f_{\rm th} (r)$$
Furthermore, assuming that isotropic turbulence provides the rest of the support needed for force balance leads to the prediction
  $$\sigma_{\rm 1D}^2 (r) = \frac {2} {3} \frac {v_{\rm c}^2 (r)} {\alpha_{\rm eff} (r)} \left[ 1 - f_{\rm th} (r) \right]$$
in which $\sigma_{\rm 1D}$ is the one-dimensional velocity dispersion of turbulent support.

Fitting such an **ExpCGM** atmosphere model to a data set containing information about both $T(r)$ and $v_{\rm c} (r)$ then constrains $f_{\rm th}(r)$ and $\sigma_{\rm 1D}$. Likewise, having information about both $T(r)$ and $\sigma_{\rm 1D}$ constrains $v_{\rm c} (r)$ and $f_{\rm th}(r)$.

### Summary of Input Parameters

#### Essential Parameters

| Parameter | Description |
| :-------: | ----------- |
|   $r_0$   | Fiducial radius (chosen by user and remains fixed) |
|   $P_0$   | Thermal pressure normalization at $r_0$ |
| $v_\varphi$ | Halo circular velocity (constant for isothermal potential) | 
| $\alpha$ | Shape function (constant for power-law atmosphere model) |

#### Shape Function Parameters

| Parameter | Description |
| :-------: | ----------- |
|  $\alpha_{\rm in}$   | Asymptotic value of $\alpha$ at small radii |
|  $\alpha_{\rm out}$  | Asymptotic value of $\alpha$ at large radii |
|  $r_\alpha$ | Transitional radius from $\alpha_{\rm in}$ to $\alpha_{\rm out}$ | 
|  $\alpha_{\rm tr}$ | Parameter determining sharpness of transition |

#### Potential Well Parameters

| Parameter | Description |
| :-------: | ----------- |
|  $v_\varphi$   | Maximum circular velocity of halo potential |
|  $r_{\rm s}$  | Scale radius of NFW halo potential well |
|  $M_*$        | Stellar mass of central galaxy | 
|  $r_{\rm H}$  | Hernquist scale radius of stellar potential well |
| $M_{\rm BH}$  | Mass of central supermassive black hole |

#### Force Balance Parameters

| Parameter | Description |
| :-------: | ----------- |
|  $f_{\rm th}$ | Thermalization fraction (may be constant or a parametric function of $r$) |
|  $f_\varphi$  | Force modification factor (may be constant or a parametric function of $r$) |


## Model Output

A variety of potentially observable atmospheric characteristics can be used to test **ExpCGM** models for galactic atmospheres and constrain their input parameters.

### Radial Profiles

An **ExpCGM** model's radial profiles of thermal pressure, temperature, and gas density depend on the input parameter set as described in the previous section. Those profiles can be directly compared with deprojected versions of $P(r)$, $T(r)$, and $\rho (r)$ derived from observational data. Fits of an **ExpCGM** model to deprojected observations therefore constrain the model's input parameters. However, reliable deprojections require high-quality data and approximate spherical symmetry.

### Projected Profiles

Projected **ExpCGM** models provide many observable predictions that can be compared more directly with observational data and combined to obtain joint constraints on a model's input parameters.

#### Surface Mass Density

A spherical atmosphere's surface mass density along a line of sight at a projected radius $r_\perp$ is 
  $$\Sigma_{\rm CGM} (r_\perp) = \int_{-\infty}^{\infty} \rho(r) ~dr_\parallel$$ 
where $r_\parallel = \pm ( r^2 - r_\perp^2 )^{1/2}$ is the component of $\mathbf{r}$ parallel to the line of sight. Bringing dimensional factors outside of the integral gives
  $$\Sigma_{\rm CGM} (r_\perp) = r_\perp \rho(r_\perp) \int_{-\infty}^{\infty} \frac {\rho(r)} {\rho(r_\perp)} ~d \left( \frac {r_\parallel} {r_\perp} \right)$$
The integral to be performed is then a structure factor of order unity usually calculated via numerical integration. 

For an isothermal power-law atmosphere, the integral can be expressed in terms of gamma functions, giving
  $$\Sigma_{\rm CGM} (r_\perp) = \left[ \frac {\pi^{1/2} \Gamma \left( \frac {\alpha - 1} {2} \right)} {\Gamma \left( \frac {\alpha} {2} \right)} \right] r_\perp \rho(r_\perp) $$
For example, the result for $\alpha = 2$ and constant $v_{\rm c}$ is
  $$\Sigma_{\rm CGM} (r_\perp) ~=~ \pi r_\perp \rho (r_\perp) ~=~ \frac {2 \pi r_0 P_0} {v_\varphi^2} \left( \frac {r_\perp} {r_0} \right)^{-1}$$

{: .note}
The approximation 
  $$\Sigma_{\rm CGM} \approx \frac {2 \pi r_0 P_0} {v_\varphi^2} \left( \frac {r_\perp} {r_0} \right)^{1-\alpha}$$ 
for isothermal power-law atmospheres is good to within 10% for $\alpha$ in the range $1.7 \leq \alpha \leq 5.5$.


#### Hydrogen Column Density

Dividing the atmosphere's surface mass density by its mean mass per hydrogen nucleus $\mu_{\rm H} m_p$ gives the total hydrogen column density along a line of sight at $r_\perp$:
  $$N_{\rm H} (r_\perp) = \frac {\Sigma_{\rm CGM} (r_\perp)} {\mu_{\rm H} m_p}$$
A primordial atmosphere has $\mu_{\rm H} = 1.33$. An atmosphere with solar abundance ratios has $\mu_{\rm H} = 1.42$.

{: .note}
Some of the observational constraints on $N_{\rm H}$ and its dependence on $r_\perp$ around galaxies like the Milky Way come from circumgalactic clouds that absorb UV light from background quasars. Photoionization modeling is necessary for determining $N_{\rm H}$ and its uncertainty range from those observations. Also, the fraction of gas without a UV absorption signature is often unknown, meaning that column density contraints derived from those observations are usually lower limits on $N_{\rm H} (r_\perp)$.


#### Electron Column Density

Dividing the atmosphere's surface mass density by its mean mass per electron $\mu_e m_p$ gives the electron column density along a line of sight at $r_\perp$:
  $$N_e (r_\perp) = \frac {\Sigma_{\rm CGM} (r_\perp)} {\mu_e m_p}$$
A fully ionized primordial atmosphere has $\mu_e = 1.14$. A fully ionized atmosphere with solar abundances has $\mu_e = 1.18$.

Dispersion-measure observations of fast radio bursts that pass through galactic atmospheres place constraints on $N_e (r_\perp)$.


#### Compton Parameter

Compton scattering distorts the microwave background spectrum along lines of sight through hot galactic atmospheres. This distortion is known as the *Thermal Sunyaev-Zeldovich Effect* or *tSZ* for short. It depends on frequency and is proportional to the ***Compton parameter***
  $$y = \int \left( \frac {kT} {m_e c^2} \right) n_e \sigma_{\rm T} ~d r_\parallel$$
in which $\sigma_{\rm T}$ is the Thomson cross section for electron scattering.

A spherical **ExpCGM** atmosphere model gives the predicted tSZ distortion profile
  $$y (r_\perp) = \left( \frac {\mu \sigma_{\rm T}} {\mu_e m_e c^2} \right) r_\perp P (r_\perp) \int_{-\infty}^{\infty} \frac {f_P (r)} {f_P (r_\perp)} ~d \left( \frac {r_\parallel} {r_\perp} \right)$$ 
As with the surface density, the integral is a structure factor of order unity usually computed through numerical integration. 

For an isothermal power-law atmosphere, the integral results in
  $$y (r_\perp) = \left( \frac {\mu \sigma_{\rm T}} {\mu_e m_e c^2} \right) \left[ \frac {\pi^{1/2} \Gamma \left( \frac {\alpha - 1} {2} \right)} {\Gamma \left( \frac {\alpha} {2} \right)} \right] r_0 P_0 \left( \frac {r_\perp} {r_0} \right)^{1-\alpha}$$
When $\alpha = 2$ it further reduces to
  $$y (r_\perp) = \left( \frac {\mu \sigma_{\rm T}} {\mu_e m_e c^2} \right) \pi r_0 P_0 \left( \frac {r_\perp} {r_0} \right)^{-1}$$
 

{: .note}
Inserting physical quantities characteristic of galaxy groups gives a reference value
  $$y \approx 5 \times 10^{-7} \left( \frac {r_\perp} {100~ {\rm kpc}} \right) \left( \frac {n} {10^{-3}~ {\rm cm}^{-3}} \right) \left( \frac {T} {10^7~ {\rm K}} \right)$$
for atmospheres with $\alpha \approx 2$.


#### Emission Measure

Collisional excitation of emission lines produces a signal proportional to the integral of $\rho^2$ along a line of sight through a galactic atmosphere. The literature on nebular emission excited by electron collisions defines the ***emission measure*** along a line of sight to be
  $${\rm EM} \equiv \int n_e n_{\rm H} ~d r_\parallel$$
The emission measure profile of an **ExpCGM** galactic atmosphere model is therefore
  $${\rm EM} (r_\perp) = \frac {r_\perp \rho^2 (r_\perp)} {\mu_e \mu_{\rm H} m_p^2} \int_{-\infty}^{\infty} \frac {\rho^2 (r)} {\rho^2 (r_\perp)} ~d \left( \frac {r_\parallel} {r_\perp} \right)$$
Once again, the integral is a structure factor of order unity, to be determined numerically. 

In the special case of an isothermal power-law atmosphere, the structure-factor integral simplifies to
  $$\int_{-\infty}^{\infty} \left( \frac {r} {r_\perp} \right)^{-2\alpha} ~d \left( \frac {r_\parallel} {r_\perp} \right) =  \frac {\pi^{1/2} \Gamma \left( \alpha - \frac {1} {2} \right)} {\Gamma \left( \alpha \right)}$$
and reduces to $\pi/2$ for $\alpha = 2$. 

{: .note}
Be aware that emission measure is sometimes defined in the literature as an integral of $n_e n_{\rm H}$ over volume rather than an integral along a line of sight. It is then proportional to a quantity called the "normalization" in models of X-ray emission.


#### Line Intensity

A particular emission line has a temperature-dependent emissivity $\epsilon_{\rm line} (T)$, defined so that $4 \pi n_e n_{\rm H} \epsilon_{\rm line}(T)$ is the emission rate of line energy per unit volume. Isotropic emission in an isothermal atmosphere therefore produces a line of intensity
  $$I_{\rm line} (r_\perp) = \epsilon_{\rm line}(T) \cdot {\rm EM} (r_\perp)$$
at projected radius $r_\perp$. The line's intensity then has units of energy/time/area/solid angle. 

Line emission from an atmosphere that is not isothermal requires numerical integration to obtain
  $$I_{\rm line} (r_\perp) = \int_{-\infty}^{\infty} n_e n_{\rm H} \epsilon_{\rm line} (T) ~d r_\parallel$$
Currently, **ExpCGM** models do not automatically provide $I_{\rm line}$ as standard output. However, a user interested in the line-intensity profile of a non-isothermal atmopshere can perform the necessary integration using the radial-profile output.

{: .note}
Sometimes the emission line of interest will be from a region of the atmosphere with a temperature distinctly different from most of the atmosphere, such as a low-temperature cloud in pressure equilibrium with the rest of the gas at its location. In that case, the integral should be limited to that region. See the [Multiphase Gas](MultiphaseGas) page for more on how gas components with $T \ll T_\varphi$ are treated in the **ExpCGM** framework.


#### Spectral Intensity

A galactic atmosphere's overall spectrum comes from both line emission and continuum emission processes collectively represented by the quantity $\epsilon_\nu (T)$. This version of emissivity is defined so that $4 \pi n_e n_{\rm H} \epsilon_\nu ~ \Delta \nu$ is the rate of energy emission per unit volume within a narrow frequency interval $\Delta \nu$ containing the frequency $\nu$. An **ExpCGM** atmosphere model therefore predicts that emission at frequency $\nu$ has an intensity 
  $$I_\nu (r_\perp | Z) = \int_{-\infty}^{\infty} n_e n_{\rm H} \epsilon_\nu (T,Z) ~d r_\parallel$$
at projected radius $r_\perp$. This ***spectral intensity*** has units of energy/time/area/solid angle/frequency and usually depends on the mass fraction $Z$ of elements other than H and He. An **ExpCGM** user interested in $I_\nu$ may specify as an auxilliary input parameter the mass fraction $Z$ of heavy elements in units of the solar value $Z_\odot$.

{: .note}
Forward modeling that convolves $I_\nu (r_\perp)$ with an instrumental response function and fits the result to an instrument's signal is an alternative to deprojection that makes the uncertainties in input model parameters easier to assess.


#### Surface Brightness

Integrating $I_\nu$ over all frequencies gives the atmosphere's ***bolometric surface brightness*** 
  $$I_{\rm bol} (r_\perp | Z) = \int_0^\infty I_\nu (r_\perp | Z) ~d\nu$$
Observations of surface brightness generally remain within a specific frequency band going from $\nu_{\rm min}$ to $\nu_{\rm max}$, in which the ***band surface brightness*** is 
  $$I_{\rm band | Z } (~ r_\perp ~|~ Z , \nu_{\rm min} , \nu_{\rm max} ~) = \int_{\nu_{\rm min}}^{\nu_{\rm max}} I_\nu (r_\perp | Z) ~d\nu$$
**ExpCGM** users can specify the desired band range $[\nu_{\rm min} , \nu_{\rm max}]$ for model output.

{: .note}
More precise comparisons with observations may require multiplying the **ExpCGM** model prediction for $I_\nu$ by a more complex frequency-dependent kernel function before integrating it over frequency. For example, soft X-ray absorption by the Milky Way's interstellar medium can attenuate the X-ray signal coming from an extragalactic source. If the interstellar X-ray optical depth along that direction is $\tau_\nu$, then the kernel function accounting for it is $\exp ( - \tau_\nu )$.

#### Minimum Pressure

Another constraint on projected **ExpCGM** models comes from UV absorption-line observations. Photoionization models of such an absorbing cloud at projected radius $r_\perp$ provide a ***minimum pressure*** estimate $P_{\rm min} (r)$ at $r \approx r_\perp$. That estimate is likely to be a lower limit on the atmospheric pressure at $r$ for two reasons:

1. The cloud's actual distance from the atmosphere's center may be greater than $r_\perp$, and thermal pressure usually declines with radius in a galactic atmosphere.
2. Thermal pressure may not be the only source of pressure keeping the cloud from being compressed by its surroundings. 


### Cumulative Profiles

Sometimes the only information we can gather about a galactic atmosphere is spatially unresolved and corresponds to a projected model quantity integrated over solid angle. The output of an **ExpCGM** model can provide predictions for some of those integrated properties. 

#### Integrated Compton Parameter Profile

Integrating a spherical atmosphere's Compton parameter over projected radius yields the ***integrated Compton parameter***
  $$Y_{\rm SZ}(r_\perp) = 2 \pi \int_0^{r_\perp} y(r_\perp) ~r_\perp d r_\perp$$
The integral has units of area and diverges at small radii unless $\alpha_{\rm in} < 3$.

Dividing $Y$ by the effective area of the full sky at a halo's redshift $z$ gives the dimensionless quantity
  $$\tilde{Y}\_{\rm SZ} = \frac {Y_{\rm SZ}} {4 \pi d_{\rm A}^2 (z)}$$
Here, $d_{\rm A} (z)$ is the cosmological angular size distance at the atmosphere's redshift $z$. The detectability of an unresolved tSZ source depends on the magnitude of $\tilde{Y}\_{\rm SZ}$.

{: .note}
The integral for $Y_{\rm SZ}$ diverges at small radii for $\alpha_{\rm in} > 3$ because the atmosphere's thermal energy unphysically diverges there. It diverges at large radii for $\alpha_{\rm out} < 3$ because the atmosphere's total thermal energy then does not converge.  


#### Bolometric Luminosity Profile

Integrating the bolometric surface brightness over projected area gives the atmosphere's bolometric luminosity profile:
  $$L_{\rm bol} (r_\perp) = 8 \pi^2 \int_0^{r_\perp} I_{\rm bol} (r_\perp \| Z) ~r_\perp d r_\perp$$
This quantity may depend on the atmosphere's heavy-element abundance $Z$, which a user can specify in units of $Z_\odot$ along with the input parameter set.

{: .note}
This integral for $L_{\rm bol}$ and the luminosity integrals that follow all diverge at small radii for $\alpha_{\rm in} > 3/2$ and at large radii for $\alpha_{\rm out} < 3/2$. To obtain converged values of $Y_{\rm SZ}$ and $L_{\rm bol}$ from **ExpCGM** models, users need to specify a shape function that has $\alpha_{\rm in} < 3/2$ and $\alpha_{\rm out} > 3$.

#### Band Luminosity Profile

Integrating the band surface brightness over projected area gives the atmosphere's luminosity in the band $[\nu_{\rm min} , \nu_{\rm max}]$:
  $$L_{\rm band} (Z) = 8 \pi^2 \int I_{\rm band}(r_\perp | Z) ~r_\perp d r_\perp$$
This quantity also may depend on the heavy-element abundance $Z$.

#### Volumetric Emission Measure Profile

Integrating ${\rm EM}(r_\perp)$ over projected area gives the ***emission normalization*** corresponding to an integral of $n_e n_{\rm H}$ over the cylindrical volume within $r_\perp$:
  $${\rm EN} (r_\perp) = 2 \pi \int_0^{r_\perp} {\rm EN}(r_\perp) ~r_\perp d r_\perp$$
Mulitplying ${\rm EN} (r_\perp)$ by $4 \pi \epsilon_{\rm line} (T)$ gives the line luminosity profile $L_{\rm line} (r_\perp)$ for an isothermal atmosphere. Users interested in line-luminosity predictions for non-isothermal atmospheres should use a model's radial-profile output to perform the necessary integrals.

{: .note}
The **ExpCGM** framework calls ${\rm EN} (r_\perp)$ an "emission normalization" because the quantity
  $$\frac {10^{-14}} {4 \pi d_{\rm A}^2 (1 + z)^2} \int n_e n_{\rm H}$$
gives a quantity often called a "normalization" in the X-ray astronomy literature. Elsewhere, ${\rm EN}$ is sometimes called an "emission measure."

### Summary of Model Output

#### Radial Profiles

| Output | Description |
| :-------: | ----------- |
|  $P(r)$  | Thermal pressure profile |
|  $T(r)$  | Temperature profile |
| $\rho (r)$ | Gas density profile | 
| $f_{\rm th} (r)$ | Thermalization profile (if $f_{\rm th}$ is a parametric function of $r$) |

#### Projected Profiles

| Output | Description |
| :-------: | ----------- |
|  $\Sigma_{\rm CGM}(r_\perp)$  | Surface mass density profile |
|  $N_{\rm H}(r_\perp)$  | Hydrogen column density profile |
|  $N_e(r_\perp)$  | Electron column density profile |
|  $y(r_\perp)$  | Compton parameter profile |
|  ${\rm EM}(r_\perp)$  | Emission measure profile |
|  $I_\nu(r_\perp \| Z)$  | Spectral intensity profile |
|  $I_{\rm bol}(r_\perp \| Z)$  | Bolometric intensity profile |
|  $I_{\rm band}(r_\perp \| Z , \nu_{\rm min} , \nu_{\rm max})$  | Band intensity profile |

#### Cumulative Profiles

| Output | Description |
| :-------: | ----------- |
|  $Y_{\rm SZ}(r_\perp)$  | Integrated Compton parameter profile |
|  $L_{\rm bol}(r_\perp \| Z)$  | Bolometric luminosity profile |
|  $L_{\rm band}(r_\perp \| Z , \nu_{\rm min} , \nu_{\rm max})$  | Band luminosity profile |
|  ${\rm VEM}(r_\perp)$ | Volumetric emission measure profile |

#### Auxilliary Input Parameters

| Output | Description |
| :-------: | ----------- |
|  $Z$  | Heavy-element abundance in units of $Z_\odot$ (default: 1) |
|  $\nu_{\rm min}$  | Minimum frequency of band (default: 0) |
|  $\nu_{\rm max}$  | Maximum frequency of band (default: $\infty$) |


## Population Scaling Laws

One of the **ExpCGM** framework's primary purposes is to constrain galactic feedback models using multiple data sets providing complementary information about how atmospheric properties depend on halo mass and redshift.

### Dependence on Halo Mass

### Dependence on Redshift



