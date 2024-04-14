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

A galaxy's circumgalactic medium (CGM) supplies the gas that sustains star formation. Galaxies appear to regulate that supply by interacting with the CGM. Supernovae in galaxies less massive than the Milky Way can reduce inflows from the CGM by driving galactic winds that expand the atmospheres of those galaxies. Black hole accretion in higher-mass galaxies appears to provide enough atmospheric energy to quench star formation by limiting a galaxy's supply of cold gas. This page explains how **ExpCGM** represents those interactions and models their effects on the CGM.

<details closed markdown="block">
  <summary>
   Contents
  </summary>
  {: .text-delta}
- TOC
{:toc}  
</details>

## Comprehensive Accounting

**ExpCGM** tracks ***all of the baryons*** originally cospatial with a halo's dark matter. If a mass $M_{\rm dm}$ of dark matter has accreted onto a halo, then the total baryonic mass to be tracked is
 $$M_{\rm acc} = \left( \frac {f_{\rm b}} {1 - f_{\rm b}} \right) M_{\rm dm}$$
in which $f_{\rm b}$ is the cosmic mean baryon mass fraction. 

All baryons not within galaxies are assigned to the CGM, so that
  $$M_{\rm CGM} = M_{\rm acc} - M_* - M_{\rm ISM}$$
where $M_*$ and $M_{\rm ISM}$ are the stellar mass and ISM mass of the halo's galaxies. For simplicity, this page treats a halo's central galaxy as its only galaxy. A future extension of **ExpCGM** will include satellite galaxies and their contributions to the atmosphere.

In **ExpCGM**, baryons that galactic feedback has pushed beyond a halo's virial radius are considered to remain part of a galaxy's atmosphere so it can be treated as a closed system that retains information about the cumulative input of feedback energy over cosmic time. The total energy associated with $M_{\rm CGM}$ is then the sum of gravitational, thermal, and non-thermal energy:
  $$E_{\rm CGM} = E_\varphi + E_{\rm th} + E_{\rm nt}$$
See the [Essentials](Essentials) page for explanations of how **ExpCGM** uses force-balanced atmosphere models to assess the amount of energy in each of those categories.  

## Minimalist Regulator Model

Evolution of an **ExpCGM** galactic atmosphere depends on how $E_{\rm CGM}$ and $M_{\rm CGM}$ change with time, and possibly also on time-dependent changes in the halo's gravitational potential $\varphi(r)$, the atmosphere's thermalization fraction $f_{\rm th}(r)$, and the pressure profile's shape function $\alpha(r)$. To evolve the atmosphere model, a set of equations coupling atmospheric evolution to galaxy evolution is needed. The simplest approach within **ExpCGM** is the ***minimalist regulator model*** outlined here.

### Mass Conservation

In the minimalist regulator model, cosmological accretion supplies atmospheric baryons at the rate $\dot{M}\_{\rm acc}$, and the atmosphere supplies the central galaxy's ISM with baryons at the rate $\dot{M}\_{\rm in}$. Star formation proceeds at the rate $M_{\rm ISM} / t_{\rm SF}$, in which the ***star-formation timescale*** $t_{\rm SF}$ is a model parameter. A fraction $f_{\rm rec}$ of the gas going into stars recycles back into the ISM. The energy released by stars drives a baryonic outflow into the CGM at the rate $\eta_M M_{\rm ISM} / t_{\rm SF}$, where $\eta_M$ is a ***mass loading parameter***. Mass conservation therefore implies

<p>
  $$\dot{M}_{\rm CGM} = \dot{M}_{\rm acc} - \dot{M}_{\rm in} + \eta_M \frac {M_{\rm ISM}} {t_{\rm SF}}$$
</p>

<p>
  $$\dot{M}_{\rm ISM} = \dot{M}_{\rm in} - ( 1 + \eta_M - f_{\rm rec}) \frac {M_{\rm ISM}} {t_{\rm SF}}$$
</p>

According to this accounting system, the galaxy's stellar mass increases as $\dot{M}\_\* = (1 - f_{\rm rec}) M_{\rm ISM} / t_{\rm SF}$ because recycling of stellar gas back into the ISM is effectively instantaneous. 

### Energy Conservation

Closure of this set of equations requires an expression for $\dot{M}\_{\rm in}$, which according to **ExpCGM** is a function of both $M_{\rm CGM}$ and $E_{\rm CGM}$. A third equation tracking the evolution of $E_{\rm CGM}$ is therefore needed:

<p>
  $$\dot{E}_{\rm CGM} = \dot{E}_{\rm acc} - \dot{E}_{\rm rad} - \dot{E}_{\rm in} + \dot{E}_{\rm fb} + \dot{E}_{\varphi,{\rm cos}}$$
</p>

Cosmological accretion adds atmospheric energy at a rate $\dot{E}\_{\rm acc}$ given by a user-supplied model for halo growth (see the [Accretion](/ExpCGM/extensions/Accretion) page for more detail). The radiative loss rate $\dot{E}\_{\rm rad}$ comes from integration over the atmosphere model, as described on the [Essentials](Essentials) page. Gas flowing from the CGM into the ISM removes energy from the CGM at a rate $\dot{E}\_{\rm in}$ equal to the product of $\dot{M}\_{\rm in}$ and $\varepsilon_{\rm in} = \varepsilon (r_{\rm gal})$, which is the specific energy of atmospheric gas at the transitional radius $r_{\rm gal}$ separating the CGM from the ISM. If galactic feedback comes only from stellar sources, the feedback energy supply is 

<p>
  $$\dot{E}_{\rm fb} = \eta_E \varepsilon_{\rm SN} \frac {M_{\rm ISM}} {t_{\rm SF}}$$
</p>

in which $\varepsilon_{\rm SN} M_{\rm ISM} / t_{\rm SF}$ is the rate at which supernovae produce kinetic energy and the ***energy loading factor*** $\eta_E$ is the fraction that couples with the CGM. Finally, $\dot{E}\_{\varphi,{\rm cos}}$ is the change in atmospheric energy stemming from changes in the gravitational potential (see the [Essentials](Essentials) page).

These three differential equations for $\dot{E}\_{\rm CGM}$, $\dot{M}\_{\rm CGM}$, and $\dot{M}\_{\rm ISM}$ make up the minimalist regulator model. To integrate the model over time, an **ExpCGM** user needs to specify an atmosphere model that provides $\dot{M}\_{\rm in}$ and $\dot{E}\_{\rm rad}$ as functions of $E_{\rm CGM}$, $M_{\rm CGM}$. A user also needs to specify the model parameters $\varepsilon_{\rm SN}$, $t_{\rm SF}$, $\eta_E$, $\eta_M$, and $f_{\rm rec}$, plus a cosmological halo model that provides $\dot{M}\_{\rm acc}$ and $\dot{E}\_{\rm acc}$. 

### Steady Star Formation

If both the galaxy's gas supply $\dot{M}\_{\rm in}$ and the star formation timescale $t_{\rm SF}$ remain sufficiently steady, then star formation converges toward the steady-state rate 

<p>
  $$\frac {M_{\rm ISM}} {t_{\rm SF}} = \frac {\dot{M}_{\rm in}} {1 + \eta_M - f_{\rm rec}}$$ 
</p>

In that limit, the minimalist regulator model reduces to a system of just two differential equations,  

<p>
  $$\dot{M}_{\rm CGM} = \dot{M}_{\rm acc} - \frac {\dot{M}_{\rm in}} {1 + \eta_M - f_{\rm rec}}$$
</p>

<p>
  $$\dot{E}_{\rm CGM} = \dot{E}_{\rm acc} + \dot{E}_{\varphi,{\rm cos}} + \left( \frac {\eta_E \varepsilon_{\rm SN}} {1 + \eta_M - f_{\rm rec}} - \varepsilon_{\rm loss} \right) \dot{M}_{\rm in}$$
</p>

Here, the specific energy $\varepsilon_{\rm loss}$ is the sum of $\varepsilon_{\rm rad} \equiv \dot{E}\_{\rm rad} / \dot{M}\_{\rm in}$ and $\varepsilon_{\rm in}$, and all of the feedback is assumed to come from stars. **ExpCGM** calls this system of two differential equations the *reduced version* of the minimalist regulator model.

<figure>
    <img src="../MinimalismCartoon.jpg"
         alt="MinimalismCartoon">
</figure>

## Regulator with Enrichment

The cooling functions that **ExpCGM** uses to calculate $\dot{M}\_{\rm in}$ and $\dot{E}\_{\rm rad}$ depend strongly on enrichment of the CGM with elements heavier than hydrogen or helium. Tracking the mass fraction $Z_{\rm CGM}$ of those elements in the CGM can be done with three additional differential equations, as follows.

### Overall Enrichment

Stars convert hydrogen and helium into heavier elements and return much of their enriched gas mass to the ISM and the CGM. **ExpCGM** expresses that heavy element yield in terms of the parameter $y_Z = M_Z / M_\*$, where $M_Z$ is the mass of newly made heavy elements ejected during recycling and $M_\*$ is the stellar mass remaining *after recycling*. While stars are adding newly made heavy elements to the ISM, cosmological accretion can be adding elements to the CGM at the rate $Z_{\rm acc} \dot{M}\_{\rm acc}$, where $Z_{\rm acc}$ is the mass fraction of accreting gas that is not hydrogen or helium. The total mass of heavy elements associated with a halo's baryons therefore increases according to

<p>
  $$\dot{M}_Z ~=~ Z_{\rm acc} \dot{M}_{\rm acc} ~+~ (1 - f_{\rm rec}) y_Z \frac {M_{\rm ISM}} {t_{\rm SF}}$$
</p>

### ISM Enrichment

The interstellar medium gains and loses heavy elements through several channels. There are two source channels. When stars eject heavy elements, a fraction $1 - \eta_Z$ mixes with the ISM, while a complementary fraction $\eta_Z$ passes into the CGM through a galactic wind. A galaxy's circumgalactic gas supply brings in additional heavy elements at the rate $Z_{\rm CGM} \dot{M}\_{\rm in}$. There are also two loss channels, both proportional to the star-formation rate. Heavy elements from the ISM are locked into stars at the rate $Z_{\rm ISM} (1 - f_{\rm rec}) M_{\rm ISM} / t_{\rm SF}$ and flow from the ISM into the CGM at the rate $Z_{\rm ISM} \eta_M M_{\rm ISM} / t_{\rm SF}$, where $Z_{\rm ISM} = M_{Z,{\rm ISM}} / M_{\rm ISM}$. The total mass $M_{Z,{\rm ISM}}$ of heavy elements in the ISM therefore evolves according to 

<p>
  $$\dot{M}_{Z,{\rm ISM}} ~=~ Z_{\rm CGM} \dot{M}_{\rm in} ~+~ \left[ (1 - f_{\rm rec}) (1 - \eta_Z) y_Z - Z_{\rm ISM} (1 + \eta_M - f_{\rm rec}) \right] \frac {M_{\rm ISM}} {t_{\rm SF}}$$
</p>

### CGM Enrichment

Evolution of the mass $M_{Z,{\rm CGM}}$ of heavy elements in the CGM can be expressed in terms of channels already described. Cosmological accretion adds them at the rate $Z_{\rm acc} \dot{M}\_{\rm acc}$. Inflow into the central galaxy removes them at the rate $Z_{\rm CGM} \dot{M}\_{\rm in}$. Galactic outflows add them both directly from supernovae at the rate $(1 - f_{\rm rec}) \eta_Z y_Z M_{\rm ISM} / t_{\rm SF}$ and via ISM ejection at the rate $Z_{\rm ISM} \eta_M M_{\rm ISM} / t_{\rm SF}$. The result is

<p>
  $$\dot{M}_{Z,{\rm CGM}} ~=~ Z_{\rm acc} \dot{M}_{\rm acc} ~-~ Z_{\rm CGM} \dot{M}_{\rm in} ~+~ \left[ (1 - f_{\rm rec}) \eta_Z y_Z + Z_{\rm ISM} \eta_M \right] \frac {M_{\rm ISM}} {t_{\rm SF}}$$
</p>

Dividing $M_{Z,{\rm CGM}}$ by $M_{\rm CGM}$ then gives the enrichment proportion $Z_{\rm CGM}$ needed to compute radiative losses from the CGM.

### Stellar Enrichment

The total mass of heavy elements contained within stars is
  $$M_{Z,*} = M_Z - M_{Z,{\rm CGM}} - M_{Z,{\rm ISM}}$$
It does not need to be tracked with a separate differential equation.

## Black Hole Feedback

**ExpCGM** does not currently include an implementation of black hole feedback. Developing such algorithms is a high priority future task for the collaboration.




