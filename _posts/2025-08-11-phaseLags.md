---
layout: post
title: "Cross Correlation Reveals Phase Lags Between Key Time-series Events"
author: "Sage Malingen"
categories: blog
tags: [blog]
image: peonies.jpg
---

Picture this: you're trying to understand the relationship between several noisy time series data. Maybe you're looking at consumer purchasing habits, maybe the timing of insect and flower life stages, or maybe electricity usage.

One thing these signals have in common is they have underlying periodicity. That was the case for my data.

I wanted to understand how muscle's sub-cellular machinery works together. Our experiment was setup to measure how the lattice of molecular machinery changes shape as the flight muscle of a large moth contracted and relaxed during flight. One thing was clear looking at the raw data: while there was underlying periodicity, there was also a lot of noise from wing beat to wing beat for several of the traces. But based on my understanding of the muscle, I wondered if there were relationships between the timing of signals within every wingbeat.

Here you can see an example of these time series data (this figure is from a paper we published in the Journal of Experimental Biology (1)).

<!--![Figure showing time series data with periodic structure and phase lags between the data. ](assets/img/phase_lags_blog_traces.jpg)-->
<img src="assets/img/phase_lags_blog_traces.jpg" width="450" height="450" alt="Figure showing time series data with periodic structure and phase lags between the data." >

In this blog post, we'll pay attention to the d1,0 signal and the i2,0/i1,0 signal. These correspond to the spacing between the thick and thin filaments of muscle and the binding of myosin molecular motors to the thin filaments.

We also measured the activation of the muscle (the EMG signal) and used the timing of EMG peaks to phase average each of the data traces recorded from the muscles. The terminology we used here was "inter-spike interval," and the idea behind it is to capture the time it takes to accomplish one wing-beat, or one cycle of muscle contraction and elongation.

<!-- ![Figure showing time series data after phase averaging using the EMG signal.](assets/img/phase_lags_blog_STAs.jpg)-->
<img src="assets/img/phase_lags_blog_STAs.jpg" width="450" height="450" alt="Figure showing time series data after phase averaging using the EMG signal." >

To understand the relationship between these signals, I used the cross correlation at various phase lags. This revealed that the phase lag at which the lattice spacing change and myosin motor binding was most correlated was diffrerent between different moths!

<!-- ![Cross correlation of phase averaged molecular motor binding and lattice spacing.](assets/img/phase_lags_blog_cross_correlation.jpg) -->
<img src="assets/img/phase_lags_blog_cross_correlation.jpg" width="450" height="300" alt="Cross correlation of phase averaged molecular motor binding and lattice spacing." >

Often, we imagine that muscles operate in very predictable, simple ways. But the variation in phase between these two signals tells me that there's more flexibility and potentially more modalities for controlling the force a muscle creates.

This analysis framework is generalizable. So when a colleague shared periodic signals recorded using a fluorescence microscopy technique with me recently, I was excited to create a quick jupyter notebook to share how to do that with some of her data. She is early in collecting data, so there are only two cells (4 and 25) that have worked so far. This experiment will be pretty cool since she's collecting time-series data for several different fluorescent tags, but here I'm only sharing data for "trace 1" and "trace 2". It's unclear from the underlying biology what the phase relationships between these signals will be and I'm hopeful that this analysis will give new insights into cell mechanics. One thing that is very clear is that the data for cell 4 were much noisier than for cell 25. As scientists, sometimes we have to make a judgement call about what level of noise is acceptable, which is something we dealt with a lot in (1).

<!-- ![Cross correlation of time series fluorescence microscopy signals.](assets/img/phase_lags_blog_fluorescence_signals.jpg) -->
<img src="assets/img/phase_lags_blog_fluorescence_signals.jpg" width="450" height="300" alt="Cross correlation of time series fluorescence microscopy signals.">

Maybe you also have some periodic data to investigate. I'd love it if you adapt this code for your work, too. It's located here: https://github.com/SageMay/datascience_scripts/tree/main/timeseriesAnalysis


1. Malingen, S. A., Asencio, A. M., Cass, J. A., Ma, W., Irving, T. C., & Daniel, T. L. (2020). In vivo X-ray diffraction and simultaneous EMG reveal the time course of myofilament lattice dilation and filament stretch. Journal of Experimental Biology, 223(17), jeb224188.
