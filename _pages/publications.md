---
layout: page
permalink: /publications/
title: Publications
description: List of publications
nav: true
nav_order: 3
---
<style>
  h2.bibliography {
    font-weight: 400 !important;   
    color: var(--global-text-color) !important; 
    opacity: 1 !important;         
  }
    /* Make the horizontal divider line darker/more visible */
  .publications .bibliography {
    border-bottom: 1px solid var(--global-text-color) !important; 
  }
</style>

<!-- _pages/publications.md -->

<!-- Bibsearch Feature -->

{% include bib_search.liquid %}

<div class="publications">

{% bibliography %}

</div>
