# ECCO COastal UPwelling (ECCO-UP)

This project uses [NASA ECCO](https://ecco-group.org/) products to investigate temperature and salinity anomalies in the coastal upwelling region south of Java. It was developed as a part of the [2025 ECCO Summer School](https://ecco-summer-school.github.io/ecco-2025/).

## Collaborators

- [Andrew Scherer](https://github.com/andrew-s28)
- [Milagro Urricariet](https://github.com/unamili)
- [Sreelekha Jarugula](https://github.com/jslekha)

## Motivation

Salinity and temperature in the eastern Indian Ocean is closely linked to the [Indian Ocean Dipole](https://www.climate.gov/news-features/blogs/enso/meet-enso%E2%80%99s-neighbor-indian-ocean-dipole). During the positive phase, the eastern Indian Ocean is relatively cool and salty; during the negative phase, the eastern Indian Ocean is relatively warm and fresh. The region of interest chosen for this study spans from 105&deg;E to 115&deg;E and from 12&deg;S to 8&deg;S.

<p align="center">
  <img src="https://github.com/ECCO-Summer-School/ESS25-Team_ECCO-UP/blob/main/figures/ecco_sal_temp_area_plot.png?raw=true" width="400px" height="auto" alt="4 area plots south of Java. The left column shows cool and salty anomalies in 1994 and the right column shows warm and fresh anomalies in 2010.">
</p>

We expect the salinity anomaly to be a result of anomalous freshwater flux (i.e., rainfall), but it is less obvious what is forcing the temperature. Therefore, we use ECCO to investigate the specific forcings that cause changes in sea surface temperature (SST) and sea surface salinity (SSS) and determine how these forcings are different in warm and fresh vs. cool and salty years.

## Data and Methods

Temperature, salinity, surface wind stress, freshwater flux, and heat flux were obtained from the [ECCO v4r4 state estimate](https://ecco-group.org/products-ECCO-V4r4.htm). Remote sensed sea surface temperature and sea surface salinity were obtained for comparison from [NOAA OISST](https://www.ncei.noaa.gov/products/optimum-interpolation-sst) and [SMAP SSS](https://podaac.jpl.nasa.gov/dataset/SMAP_RSS_L3_SSS_SMI_MONTHLY_V4), respectively. All datasets had the long-term mean and annual climatology consisting of three harmonics (one, two, and three cycles per year) removed from the data.

The attribution, budget, adjoint gradients, convolution, and adjoint tracer simulations were conducted using the [ECCO Modeling Utilities (EMU)](https://ecco-group.org/docs/01_16_fukumori_emu_ecco_2024_03.pdf). EMU outputs were analyzed using the [emu-utilities package](https://github.com/andrew-s28/emu-utilities), which was also developed as a part of this project.

> [!WARNING]
> Code within this repository depends on datasets which were obtained and stored on compute clusters only available during the 2025 ECCO Summer School and therefore may not be immediately reproducible.

## Project Results

### ECCO and Satellite Comparison

The ECCO product successfully reproduces the observed interannual variability in the remote sensed sea surface temperature and sea surface salinity.

<p align="center">
  <img src="https://github.com/ECCO-Summer-School/ESS25-Team_ECCO-UP/blob/main/figures/poster_pres_figs/sst_conv_reconstructed_2010_objf.png?raw=true" width="400px" height="auto" alt="A time series comparison between ECCO v4r4 SST and NOAA OISST that shows good agreement of anomalies.">
</p>

ECCO is using these observations to constrain the v4r4 state estimates but it is important to double check that the interannual variability we are interested in is present in the state estimate.

### Adjoint Gradients and Convolution

Two adjoint gradients were computed using sea surface temperature and sea surface salinity in the box region over three months as the defined objective function. *This was a mistake and made the interpretation of the gradients somewhat tricky*. However, we didn't have time to re-run the gradients with the objective function defined only over a month. Nevertheless, we will proceed with examining these results.

<p align="center">
  <img src="https://github.com/ECCO-Summer-School/ESS25-Team_ECCO-UP/blob/main/figures/poster_pres_figs/EMU_SST_SSS_Adjoint_Lag24weeks_2010.png?raw=true" width="600px" height="auto" alt="Adjoint gradients for SST and SSS for controls of freshwater flux, heat flux, and winds.">
</p>

The SST responded strongly to both winds (particularly westward winds) and heat flux. The response to winds is both local and remote, with a strong negative response to local westward winds; when westward winds increase, SST decreases, a response consistent with local winds causing Ekman transport, i.e., coastal wind-driven upwelling. The SSS responds similarly, except with a strong sensitivity to freshwater flux rather than heat flux. Both SST and SSS also depend on remote winds, heat, and freshwater flux, especially to the northeast in a pathway that follows the Indonesian flow-through.

Using the convolution of the adjoint gradients with the forcings we find the explained variance as a function of both lag and control variable. As shown in the adjoint gradients themselvs, both heat flux and winds explain the majority of the interannual variability in SST and both freshwater flux and winds explain the majority of the interannual variability in SSS. In SST, the majority of variance is explained within a lag of six months.

<p align="center">
  <img src="https://github.com/ECCO-Summer-School/ESS25-Team_ECCO-UP/blob/main/figures/poster_pres_figs/exp_variance.png?raw=true" width="400px" height="auto" alt="Explained variance in SST and SSS as a function of control variable and lag.">
</p>

The convolutions are also used to reconstruct a time series of SST and compared with the ECCO v4r4 monthly fields. However, we observe an approximately six month phase lag between the ECCO v4r4 SST and the reconstructed SST. This could be due to the definition of the SST objective function over a period of four months or due to issues with how the EMU tool is converting the directions of wind stress, but it couldn't be determined within the summer school period. A four month rolling mean filter is applied to the ECCO v4r4 SST data in the plot below to try to resolve the issue, but it didn't make much of a difference.

<p align="center">
  <img src="https://github.com/ECCO-Summer-School/ESS25-Team_ECCO-UP/blob/main/figures/poster_pres_figs/sst_conv_reconstructed_2010_objf.png?raw=true" width="400px" height="auto" alt="Time series plot of reconstructed SST and ECCO v4r4 SST.">
</p>

### Adjoint Tracer Simulations

A passive tracer was released on August 1st in 1994 and 2010 within the top grid cell (0-10 m) evenly spread over the study region from 105&deg;E to 115&deg;E and from 12&deg;S to 8&deg;S. Using the adjoint machinery within ECCO, this tracer was tracked backwards one year. The results of this release are shown below, with the tracer units represented as a fraction of total tracer.

<p align="center">
  <img src="https://github.com/ECCO-Summer-School/ESS25-Team_ECCO-UP/blob/main/figures/poster_pres_figs/AdjointTracer_depth_integrated_1994_2010_animation_1x2.gif?raw=true" width="auto" height="auto" alt="A gif showing the tracer release tracked backwards in 1994 and 2010.">
</p>

The adjoint tracer simulation shows that in both 1994 and 2010, the majority of the tracer in the surface south of Java originates to the northeast within the Maritime Continent region six months prior. However, in 2010 - a warm and fresh year - there is also a significant contribution of tracer from the northwest. This suggests an advective anomaly that could explain the different conditions south of Java between the two years. However, it would take further research, perhaps a water mass analysis of the origination regions, to determine this for certain.

## Repository Structure

* **`contributors/`**
<br> Code from each of the team members for this project.
* **`figures/`**
<br> Figures output from the analyses in this project.
* **`notebooks/`**
<br> Notebooks that are considered delivered results.
* **`scripts/`**
<br> Code that is shared by the team.
