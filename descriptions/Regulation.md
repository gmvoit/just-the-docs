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

Galaxies appear to regulate long-term star formation by interacting with the gas supply coming from the circumgalactic medium. Galaxy winds driven by supernovae add energy that can expand the atmospheres of low-mass galaxies. Outflows driven by energy released during black-hole accretion seem necessary to regulate and quench star formation in high mass galaxies. This page explains how **ExpCGM** represents those interactions and models their effects on a galaxy's gas supply.

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

**ExpCGM** tracks ***all*** of the baryons originally cospatial with a halo's dark matter. If a mass $M_{\rm dm}$ of dark matter has accreted onto a halo, then the total baryon mass to be tracked is $M_{\rm acc} = [f_{\rm b} / (1 - f_{\rm b})] M_{\rm dm}$, where $f_{\rm b}$ is the cosmic mean baryon mass fraction. All baryons not within galaxies are assigned to the CGM, so that
  $$M_{\rm CGM} = M_{\rm acc} - M_* - M_{\rm ISM}$$
where $M_*$ and $M_{\rm ISM}$ are the stellar mass and ISM mass of the halo's galaxies. For simplicity, this page treats the halo's central galaxy as the only one that matters. A future extension of **ExpCGM** will include satellite galaxies and their contribution to the central galaxy's atmosphere.

Baryons that galactic feedback has pushed beyond a halo's virial radius are considered to remain part of a galaxy's atmosphere so that the CGM can be treated as a closed system that retains information about the cumulative input of feedback energy over cosmic time. The total energy associated with $M_{\rm CGM}$ is then the sum of gravitational, thermal, and non-thermal energy:
  $$E_{\rm CGM} = E_\varphi + E_{\rm th} + E_{\rm nt}$$
See the [Essentials](Essentials) page for explanations of how **ExpCGM** uses a force-balanced atmosphere model to calculate the amount of energy in each of those forms.  

## Minimalist Regulator Model

Evolution of an **ExpCGM** galactic atmosphere depends on how $E_{\rm CGM}$ and $M_{\rm CGM}$ change with time, and possibly also on time-dependent changes in the halo's gravitational potential $\varphi(r)$, the atmosphere's thermalization fraction $f_{\rm th}(r)$, anad the pressure profile's shape function $\alpha(r)$. To evolve the atmosphere model, a set of equations coupling atmospheric evolution to galaxy evolution is needed. The simplest approach within **ExpCGM** is the ***minimalist regulator model*** outlined here.

In the minimalist regulator model, cosmological accretion supplies atmospheric baryons at the rate $\dot{M}\_{\rm acc}$, and the atmosphere supplie the central galaxy's ISM with baryons at the rate $\dot{M}\_{\rm in}$. Star formation proceeds at the rate $\dot{M}_* = M_{\rm ISM} / t_{\rm SF}$, in which the ***star-formation timescale*** $t_{\rm SF}$ is a model parameter. The energy released by that stellar population drives a baryonic outflow into the CGM at the rate $\eta_M \dot{M}_*$, where $\eta_M$ is ***mass loading parameter***. Mass conservation then implies

<p>
  $$\dot{M}_{\rm CGM} = \dot{M}_{\rm acc} - \dot{M}_{\rm in} + \eta_M \frac {M_{\rm ISM}} {t_{\rm SF}}$$
</p>

<p>
  $$\dot{M}_{\rm ISM} = \dot{M}_{\rm in} - ( 1 + \eta_M ) \frac {M_{\rm ISM}} {t_{\rm SF}}$$
</p>

The most basic version of the minimalist regulator model does not track recycling of baryonic mass from stars into the ISM, but it can be included as described in the *Interstellar Recycling* section below.

Closure of this set of equations requires an expression for $\dot{M}\_{\rm in}$, which is a function of both $M_{\rm CGM}$ and $E_{\rm CGM}$, according to **ExpCGM**. A third equation tracking the evolution of $E_{\rm CGM}$ is therefore needed:

<p>
  $$\dot{M}_{\rm CGM} = \dot{M}_{\rm acc} - \dot{M}_{\rm in} + \eta_M \frac {M_{\rm ISM}} {t_{\rm SF}}$$
</p>


### Reduced Version

### Interstellar Recycling

## Regulator with Enrichment

## Black Hole Feedback





