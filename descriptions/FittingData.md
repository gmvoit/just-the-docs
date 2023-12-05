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

This page outlines how to fit observational data and simulated galactic atmospheres with **ExpCGM** models.

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

All **ExpCGM** models for galactic atmospheres are based on a user-specified gravitational potential function $\varphi(\mathbf{r})$ and a user-specified shape function $\alpha(\mathbf{r})$ for the atmosphere's pressure profile. In general, they can be functions of a three dimensional vector $\mathbf{r}$. Spherically symmetric models depending only on $r = |\mathbf{r}|$ are often sufficient, and this introductory page focuses on such models.

Parametric models are the starting point for obtaining predicted observables: 
* **Gravitational Potential** $\varphi( r ; v_\varphi , r_{\rm s} , r_0 , ... )$.

### Basic Model

### NFW + Galaxy Model

## Output Models

### Thermodynamic Profiles

### Projected Observables

## Scaling Laws


