# TIaRA-TESS-Yields

This repository contains machine readable tables of the results from [Rodel et al (2024)](https://eur02.safelinks.protection.outlook.com/?url=https%3A%2F%2Ftrack.smtpsendmail.com%2F9032119%2Fc%3Fp%3DS6ORnKLKpTofVkXG-xc1BoN-d4AKV87_QoKrUpftEQO4tmW5LX_zWhiFp-cHwJNLafRpQFn4TFvXStF5cdeoKFLSwRT-lMkx0q-xQGvut9ebpDV7TaxUAe5gUcbvlaPHwGum95-CN2xCLPQO10gCIdQzEB7Geqdjr_1b2pJ-ltOi13X5idYFfsrYeuKAD7gRIxOl05VDvLSpVJbAjOwwOUPPvy5mMR01gI99yU6EQJ-mhAswkJWIyi2y1Jc9Wcfov0oithSXr76EqEBd6wiP_0dVW-Brx6qrRmqsi9VLCuSGYVariN6bieO-X03dVDr11mlzRvQ_VIw0mY-1z7owhg%3D%3D&data=05%7C02%7Ctrodel01%40qub.ac.uk%7Cc4717f76a6a34f85825908dc3236263c%7Ceaab77eab4a549e3a1e8d6dd23a1f286%7C0%7C0%7C638440455350238607%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C0%7C%7C%7C&sdata=aAVrs7z0cWsvsWGaUZeEIdAjeUvGlK9NI3MnkR3Hh0k%3D&reserved=0)

The content and structure of these tables is described below. Please contact the authors if you have queries or difficulty using these files.

## Repo structure

Results are divided by TESS mission years, the first year only and Year 1 combined with Year 3. Within each of these are the combined yield estimates for all AFGKM stars in ``AFGKM_yield.csv`` and a table of the stellar parameters used to create these in ``STAR_PARAMS.feather``.

There are also folders for each spectral type containing the yield and sensitivity tables for each.

## Sensitivity tables

The format for sensitivity file names is ``[spectral type]_sensitivity.csv''.

Column headers are described below:

### Period and radius columns

Each grid cell is represented by one row of a csv file as described below:

|Header|Description|Example value|
|------|-----------|-------------|
|``periodbin``|Orbital period bin edges in days with format ``"([lower edge], [upper edge]]"``|``"(0.78, 1.56]"``|
|``radiusbin``|Planetary radius bin edges in Earth radii with format ``"([lower edge], [upper edge]]"``|``"(0.5, 0.71]"``|

### Sensitivities

The sensitivity columns are named by the function used to calculate a probability of detection based on signal to noise as described below:

|Function prefix|Description|
|---------------|-----------|
|`GAMMA`|Gamma function using a continuous probability parametrised on the Kepler data and applied to TESS (see paper for details)|
|``THR_7-3``|Signal to noise threshhold step function where probability of detection is 0 below SNR 7.3 and 1 above it.|
|``THR_20``|Signal to noise threshhold step function where probability of detection is 0 below SNR 20 and 1 above it.|

Sensitivities are marked by the function above and whether it includes the probability of transit and observation as well as detection:

|Sensitivity column|Description|
|------------------|-----------|
|``[Function]_mean``|Mean detection probability for the respective grid cell calculated using one of the functions above|
|``[Function]_OBS_mean``|Mean of the product of the probability of detection and the probability of observation|
|``[Function]_OBS_TRANS_mean``|Mean of the product of the probability of detection, the probability of observation by TESS and the geometric probability of transit|

So if I wanted to retrieve the probability of a planet being in a transiting configuration, having at least one transit seen by TESS and then being detected according to a gamma function I would be looking at the ``GAMMA_OBS_TRANS_mean`` column.

### Counts and auxiliary stats

As well as the probability of detection columns shown above we also store the counts of planets included in the simulation.

|Count column|Description|
|------------|-----------|
|``SIM_COUNT``|Number of planets included in the simulation after radius cutoff|
|``COUNT``|Adjusted count of planets included in the simulation with planets cut off for low radius added back in|

As well as these counts we also included sensitivities and counts for planets in the simulation which meet different critera. The sensitivity values and counts for these can be found by adding them as a prefix to the column you are looking for.

For example for monotransits, the gamma detection probability would be ``MONO_GAMMA_mean`` and the count of monotransit signals in the simulation would be ``MONO_COUNT``.

|Prefix|Description|
|------|-----------|
|``MONO``|Monotransits, signals with only one transit observed in the simulation|
|``DUO``|Duotransits, signals with two transits observed in the simulation|
|``TRUE_DUO``|Biennial duotransits, signals with one transit observed in one year of TESS and a second at least a year later|
|``TRIO``|Triotransits, signals with three transits observed in the simulation|
|``MULTI``|Multitransits, Signals with 4 or more transits observed in the simulation|
|``TSM_CUTOFF``|Signals with TSM that exceeds Radius based cutoffs laid out in Kempton et al (2018)|

## Yield tables

The general format for yield table filenames is ``[Spectral type]_yield.csv``.

The yield tables are structured similarly to the sensitivity tables with yield columns labelled by the probability of detection function used to calculate them.

So for example the yield from the gamma function would be ``GAMMA_YIELD`` and ``MONO_GAMMA_YIELD`` specifically for monotransits.

In addition to this the upper and lower errors are in colums with the added suffix ``_upper`` and ``_lower`` respectively.
