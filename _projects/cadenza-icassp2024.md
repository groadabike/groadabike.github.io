---
layout: page
title: ICASSP 2024
description: A tutorial-style guide to the ICASSP 2024 Cadenza Challenge
img: assets/img/projects/icassp2024/icassp2024-crosstalk.png
importance: 4
category: Cadenza Project
related_publications: true
giscus_comments: true
---

<h2 style="display: flex; align-items: center; gap: 0.75rem; flex-wrap: wrap;">
  <img
    src="{{ 'assets/img/logos/cadenza_logo.png' | relative_url }}"
    alt="Cadenza project logo"
    style="height: 60px; width: auto;"
  >
  <span>ICASSP 2024 Grand Challenge</span>
</h2>

<div style="margin: 0 0 1.5rem 0;" markdown="1">

{% include figure.liquid path="assets/img/projects/icassp2024/icassp2024-banner.png" title="Abstract CAD1 banner showing a network-like flow of audio signals splitting and recombining to represent source separation, personalised processing, and remixing" class="img-fluid rounded z-depth-1" %}

</div>

This page presents the ICASSP 2024 Cadenza Grand Challenge as a follow-on tutorial project: what we extend after [CAD1]({{ '/projects/cadenza-cad1/' | relative_url }}), why we extended it, and how others can still use the challenge materials.
Although the challenge concluded in January 2024, the submitted signals and evaluation results remain openly available for further research.

If you are new to this line of work, it helps to read this page together with the
[official ICASSP 2024 challenge introduction](https://cadenzachallenge.org/docs/icassp_2024/intro)
and the earlier [CAD1 project page]({{ '/projects/cadenza-cad1/' | relative_url }}).

---

## Why this challenge 

The ICASSP 2024 Cadenza Grand Challenge {% cite roadabike2025_ojsp %} was designed directly in response to what we learned from [CAD1]({{ '/projects/cadenza-cad1/' | relative_url }}).

In [CAD1]({{ '/projects/cadenza-cad1/' | relative_url }}), strong off-the-shelf source separation baselines already performed very well, especially in the headphone task.
That was an important result: it showed that the out-of-the-box technologies are strong in studio quality signals.

The ICASSP 2024 challenge therefore made the problem harder and more realistic.
Instead of headphone listening, it focused on a listener wearing hearing aids in front of stereo loudspeakers, where acoustic cross-talk changes the mixture reaching each ear.
This created a more demanding task and a clearer opportunity for new methods.

## What's different from CAD1

Compared with [CAD1]({{ '/projects/cadenza-cad1/' | relative_url }}), this challenge introduced three key changes:

1. **Loudspeaker reproduction instead of headphone playback**, making the listening setup closer to real-world music listening.
2. **Cross-talk between loudspeakers and ears**, meaning the left and right ear signals no longer matched the original stereo channels in a simple way.
3. **Independent target gains for each VDBO stem**, requiring more flexible remixing than the earlier task.

Together, these changes turned the challenge from a relatively constrained demixing-and-amplification pipeline into a richer personalised remixing problem.

## The scenario

<div style="max-width: 650px; margin: 0 auto;"  markdown="1">

{% include figure.liquid path="assets/img/projects/icassp2024/icassp2024-crosstalk.png" title="Diagram of the ICASSP 2024 scenario, where two loudspeakers, in front of a head, have arrows indicating how the sound travel differently to each ear." class="iimg-fluid rounded z-depth-1" %}

***Figure 1**. The scenario.*
</div>

The signals are captured at hearing-aid microphones positioned at each ear while music is played through a pair of stereo loudspeakers in an anechoic room.
Because of loudspeaker cross-talk each ear receives a frequency-dependent mixture of both speakers rather than a simple left/right channel pair.

This cross-talk was modelled with head-related transfer functions (HRTFs), so standard stereo demixing systems had to cope with distortions not normally present in source-separation benchmarks.

Participants had to estimate the VDBO (vocals, drums, bass, other) components from this loudspeaker mixture, apply listener-specific target gains to each stem, and then apply hearing-aid amplification personalised to the listener's audiogram.
Both causal and non-causal approaches were allowed, and participants were free to use either explicit demixing pipelines or end-to-end systems.

## How the challenge worked

Systems were evaluated with HAAQI on the final remixed stereo signal.

The challenge therefore served two purposes:

1. **Method development**, by giving participants a harder benchmark than [CAD1]({{ '/projects/cadenza-cad1/' | relative_url }}).
2. **Challenge design**, by testing whether a more realistic listening setup would create more headroom over strong baselines.

## Baseline systems

Two out-of-the-box source separation models were provided as baselines: **Hybrid Demucs** and **Open-Unmix**, both distributed via PyTorch.
Each baseline estimated the VDBO stems from the loudspeaker mixture, applied the target gains, remixed the stems to stereo, and then applied NAL-R amplification personalised to the listener.

<div style="max-width: 650px; margin: 0 auto;"  markdown="1">

{% include figure.liquid path="assets/img/projects/icassp2024/icassp2024-baseline.png" title="Diagram with the baseline system, showing how different components are used." class="iimg-fluid rounded z-depth-1" %}

***Figure 2**. Detailed schematic of the baseline system.*

</div>

## What we learned

Several participant systems **outperform the Hybrid Demucs baseline**.
That outcome confirmed the main design hypothesis behind the challenge: introducing loudspeaker cross-talk and independent per-stem gains made the out-of-the-box source separation model underperformed, needing new and more clever models.

### Selected results

| Rank    | Team                   | ID         | HAAQI      |
|---------|------------------------|------------|------------|
| 1       | T022                   | E047       | 0.6317     |
| 2       | T022                   | E022       | 0.6309     |
| 3       | T003                   | E003_sup   | 0.5929     |
| 4       | T003                   | E003       | 0.5923     |
| 5       | T011                   | E011_aug   | 0.5857     |
| 6       | T018                   | E018       | 0.5849     |
| 7       | T011                   | E011       | 0.5798     |
| 8       | T012                   | E012       | 0.5731     |
| 9       | T046                   | E046       | 0.5704     |
| **10**  | **Baseline (HDemucs)** | —          | **0.5697** |
| ...     |                        |            |            |
| 16      | Baseline (Open-Unmix)  | —          | 0.5113     |

## How to participate now

Even though the competition is over, the ICASSP 2024 challenge remains useful as an open research and teaching resource.

You can participate by:

1. **Reading the original challenge docs** at [cadenzachallenge.org](https://cadenzachallenge.org/docs/icassp_2024/intro) 
2. **Downloading the public submissions and scores** from [Zenodo](https://zenodo.org/records/13285307).
3. **Reproducing the baselines** and comparing them against the published HAAQI results.
4. **Citing the challenge papers** if you build on the task or data.

## Open resources

- [Official ICASSP 2024 challenge introduction](https://cadenzachallenge.org/docs/icassp_2024/intro)
- [CAD1 project page]({{ '/projects/cadenza-cad1/' | relative_url }})
- [Submitted signals and HAAQI scores on Zenodo](https://zenodo.org/records/13285031)
- {% cite roadabike2025_ojsp %}
