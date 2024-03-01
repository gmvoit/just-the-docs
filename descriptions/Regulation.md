---
title: Regulation
layout: default
nav_order: 3
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

# Regulation
{: .no_toc}

A galaxy's circumgalactic medium (CGM) supplies the gas that sustains star formation, and galaxies appears to regulate that supply by interacting with the CGM. Supernovae drive galactic winds that can expand the atmospheres of galaxies less massive than the Milky Way. In higher-mass galaxies, black hole accretion appears to provide enough energy to reduce the galaxy's gas supply and quench star formation. This page explains how **ExpCGM** represents those interactions and models their effects on the CGM.

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

## Comprehensive Accounting

**ExpCGM** tracks ***all*** of the baryons originally cospatial with a halo's dark matter. If a mass $M_{\rm dm}$ of dark matter has accreted onto a halo, then the total baryon mass to be tracked is
 $$M_{\rm acc} = \frac {f_{\rm b}} {1 - f_{\rm b}} M_{\rm dm}$$
in which $f_{\rm b}$ is the cosmic mean baryon mass fraction. 

All baryons not within galaxies are assigned to the CGM, so that
  $$M_{\rm CGM} = M_{\rm acc} - M_* - M_{\rm ISM}$$
where $M_*$ and $M_{\rm ISM}$ are the stellar mass and ISM mass of the halo's galaxies. For simplicity, this page treats the halo's central galaxy as the halo's only galaxy. A future extension of **ExpCGM** will include satellite galaxies and their contribution to the central galaxy's atmosphere.

In **ExpCGM**, baryons that galactic feedback has pushed beyond a halo's virial radius are considered to remain part of a galaxy's atmosphere so that the CGM can be treated as a closed system that retains information about the cumulative input of feedback energy over cosmic time. The total energy associated with $M_{\rm CGM}$ is then the sum of gravitational, thermal, and non-thermal energy:
  $$E_{\rm CGM} = E_\varphi + E_{\rm th} + E_{\rm nt}$$
See the [Essentials](Essentials) page for explanations of how **ExpCGM** uses a force-balanced atmosphere model to calculate the amount of energy in each of those categories.  

## Minimalist Regulator Model

Evolution of an **ExpCGM** galactic atmosphere depends on how $E_{\rm CGM}$ and $M_{\rm CGM}$ change with time, and possibly also on time-dependent changes in the halo's gravitational potential $\varphi(r)$, the atmosphere's thermalization fraction $f_{\rm th}(r)$, and the pressure profile's shape function $\alpha(r)$. To evolve the atmosphere model, a set of equations coupling atmospheric evolution to galaxy evolution is needed. The simplest approach within **ExpCGM** is the ***minimalist regulator model*** outlined here.

### Mass Conservation

In the minimalist regulator model, cosmological accretion supplies atmospheric baryons at the rate $\dot{M}\_{\rm acc}$, and the atmosphere supplies the central galaxy's ISM with baryons at the rate $\dot{M}\_{\rm in}$. Star formation proceeds at the rate $\dot{M}\_* = M_{\rm ISM} / t_{\rm SF}$, in which the ***star-formation timescale*** $t_{\rm SF}$ is a model parameter, and recycles a fraction $f_{\rm rec}$ of that gas back into the ISM. The energy released by that stellar population drives a baryonic outflow into the CGM at the rate $\eta_M \dot{M}_\*$, where $\eta_M$ is a ***mass loading parameter***. Mass conservation therefore implies

<p>
  $$\dot{M}_{\rm CGM} = \dot{M}_{\rm acc} - \dot{M}_{\rm in} + \eta_M \frac {M_{\rm ISM}} {t_{\rm SF}}$$
</p>

<p>
  $$\dot{M}_{\rm ISM} = \dot{M}_{\rm in} - ( 1 + \eta_M - f_{\rm rec}) \frac {M_{\rm ISM}} {t_{\rm SF}}$$
</p>

Because the return of stellar gas to the ISM is effectively instantaneous, the galaxy's stellar mass grows according to $\dot{M}\_\* = (1 - f_{\rm rec}) M_{\rm ISM} / t_{\rm SF}$. 

### Energy Conservation

Closure of this set of equations requires an expression for $\dot{M}\_{\rm in}$, which according to **ExpCGM** is a function of both $M_{\rm CGM}$ and $E_{\rm CGM}$. A third equation tracking the evolution of $E_{\rm CGM}$ is therefore needed:

<p>
  $$\dot{E}_{\rm CGM} = \dot{E}_{\rm acc} - \dot{E}_{\rm rad} - \dot{E}_{\rm in} + \dot{E}_{\rm fb} + \dot{E}_{\varphi,{\rm cos}}$$
</p>

Cosmological accretion adds atmospheric energy at a rate $\dot{E}\_{\rm acc}$ given by a user-supplied model for halo growth. The radiative loss rate $\dot{E}\_{\rm rad}$ comes from integration over the atmosphere model, as described on the [Essentials](Essentials) page. Gas flowing from the CGM into the ISM removes energy from the CGM at a rate $\dot{E}\_{\rm in}$ equal to the product of $\dot{M}\_{\rm in}$ and $\varepsilon_{\rm in} = \varepsilon (r_{\rm gal})$ of atmospheric gas at the transitional radius $r_{\rm gal}$. If the feedback comes only from stellar sources, the feedback energy supply is 

<p>
  $$\dot{E}_{\rm fb} = \eta_E \varepsilon_{\rm SN} \dot{M}_*$$
</p>

in which $\varepsilon_{\rm SN} \dot{M}_\*$ is the rate at which supernovae produce kinetic energy and the ***energy loading factor*** $\eta_E$ is the fraction that couples with the CGM. Finally, $\dot{E}\_{\varphi,{\rm cos}}$ is the change in atmospheric energy stemming from changes in the gravitational potential (see the [Essentials](Essentials) page).

The minimalist regulator model comprises these three differential equations for $\dot{E}\_{\rm CGM}$, $\dot{M}\_{\rm CGM}$, and $\dot{M}\_{\rm ISM}$. An **ExpCGM** atmosphere model provides $\dot{M}\_{\rm in}$ and $\dot{E}\_{\rm rad}$ as functions of $E_{\rm CGM}$, $M_{\rm CGM}$, and model parameters. A cosmological halo model provides $\dot{M}\_{\rm acc}$ and $\dot{E}\_{\rm acc}$. 

### Steady Star Formation

If both the galaxy's gas supply $\dot{M}\_{\rm in}$ and the star formation timescale $t_{\rm SF}$ remain sufficiently steady, then the star formation converges toward the steady-state rate $\dot{M}\_\* = \dot{M}\_{\rm in} / (1 + \eta_M)$. In that limit, the minimalist regulator model reduces to a system of just two differential equations,  

<p>
  $$\dot{M}_{\rm CGM} = \dot{M}_{\rm acc} - \frac {\dot{M}_{\rm in}} {1 + \eta_M - f_{\rm rec}}$$
</p>

<p>
  $$\dot{E}_{\rm CGM} = \dot{E}_{\rm acc} + \left( \frac {\eta_E \varepsilon_{\rm SN}} {1 + \eta_M} - \varepsilon_{\rm loss} \right) \dot{M}_{\rm in} + \dot{E}_{\varphi,{\rm cos}}$$
</p>

in which $\varepsilon_{\rm loss}$ is the sum of $\varepsilon_{\rm in}$ and $\varepsilon_{\rm rad} \equiv \dot{E}\_{\rm rad} / \dot{M}\_{\rm in}$ and all of the feedback is assumed to come from stars. **ExpCGM** calls this system of two equations the *reduced version* of the minimalist regulator model.


## Regulator with Enrichment

The cooling functions ($\Lambda$ or $\Lambda_\rho$) that **ExpCGM** uses to calculate $\dot{M}\_{\rm in}$ and $\dot{E}\_{\rm rad}$ depends strongly on enrichment of the CGM, brought about by galactic winds. Tracking the evolution of enrichment can be done with the three additional differential equations, as follows.

### Overall Enrichment

Stars convert hydrogen and helium into heavier elements and returns those heavier elements to the ISM. **ExpCGM** expresses that heavy element yield in terms of the parameter $y_Z = M_Z / M_\*$, where $M_Z$ is the mass of newly made heavy elements added to the ISM during recycling and $M_\*$ is the stellar mass remaining *after recycling*. While stars are adding newly made heavy elements, cosmological accretion can be adding elements to the CGM at the rate $Z_{\rm acc} \dot{M}\_{\rm acc}$, where $Z_{\rm acc}$ is the mass fraction of accreting gas that is not hydrogen or helium. The total mass in such elements therefore increases according to

<p>
  $$\dot{M}_{\rm Z} = Z_{\rm acc} \dot{M}_{\rm acc} + (1 - f_{\rm rec}) y_Z \frac {M_{\rm ISM}} {t_{\rm SF}}$$
</p>

### ISM Enrichment

The interstellar medium gains and loses heavy elements through several channels. It gains heavy elements through stellar recycling. A galaxy's gas supply add more heavy elements at the rate $Z_{\rm CGM} \dot{M}\_{\rm in}$, where $Z_{\rm CGM} = M_{\rm Z,CGM} / M_{\rm CGM}$ is the heavy-element proportion of the CGM. The two loss channels are both proportional to the star-formation rate. The ISM loses heavy elements to stars at the rate $Z_{\rm ISM} M_{\rm ISM} / t_{\rm SF}$ and to the CGM at the rate $Z_{\rm ISM} \eta_M M_{\rm ISM} / t_{\rm SF}$, where $Z_{\rm ISM} = M_{\rm Z,ISM} / M_{\rm ISM}$. Enrichment of the ISM therefore evolves according to 

<p>
  $$\dot{M}_{\rm Z,ISM} = Z_{\rm CGM} \dot{M}_{\rm in} + (1 - f_{\rm rec}) y_Z \frac {M_{\rm ISM}} {t_{\rm SF}} - Z_{\rm ISM} (1 + \eta_M) \frac {M_{\rm ISM}} {t_{\rm SF}}$$
</p>

### CGM Enrichment

Evolution of the mass of heavy elements in the CGM ($M_{\rm Z,CGM}$) can be expressed in terms of the channels we have already described:

<p>
  $$\dot{M}_{\rm Z,CGM} = Z_{\rm acc} \dot{M}_{\rm acc} - Z_{\rm CGM} \dot{M}_{\rm in} + Z_{\rm ISM} \eta_M \frac {M_{\rm ISM}} {t_{\rm SF}}$$
</p>

### Stellar Enrichment

The total mass of heavy elements contained within stars is then
  $$M_{Z,*} = M_Z - M_{Z,{\rm CGM}} - M_{Z,{\rm ISM}}$$
and does not need to be tracked with a separate differential equation.

## Black Hole Feedback

**ExpCGM** does not currently include an implementation of black hole feedback. Developing one is a high priority task for the collaboration.




