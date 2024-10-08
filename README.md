In this notebook we demonstrate a simple approach to increase the temporal resolution of climate projection from one day to one hour. The method consists in a search of sequences of daily values in reanalysis that are similar to the climate projection. The length of the sequences is limited to a few days (typically 7 days) to ensure that analogous situation can be found in the reanalysis. Once an analog daily sequences is found, the hourly reanalysis are used to populate the temporally downscaled climate projection.

The search of the analogs is made for each day independantly. The analogy is evaluated by estimating the difference between the daily climate projection sequence and the daily reanalysis sequence. We considered the quadratic difference weighted by a Gaussian Kernel centered on the current day. To ensure the plausibility of the method, the analog search is limited to exclude situation from a totally different season or time of the day (for wind only). 

The search being made on a daily basis, there is a risk to obtain jumps between days. This risk is mitigated by two means: 
 - using sequences for the search ensures that the choice of a day take into consideration the previous and following days avoiding by construction strong differences
 - not only the current day is used for the building the downscaled time series but all days of the sequence (i.e. the 7 days), each day being weighted using the kernel (the central day is weighted by far the most and the preceeding and subsequent day less)

The choice of the Kernel width is therefore a very important hyperparameter of the method. A too large kernel will result in a long sequence which will be difficult to find. In addition, it will result in an oversmoothing of the result because the difference in weight between the current, preceeding and subsequent day will be too similar. A too small kernel will not consider the dynamic of the signal and will ultimately result in jumps between days (see previous paragraph).

The current example is built upon regionally averaged wind power capacity factors calculated in the clim2power project. We recomment to use this approach for geographically averaged values because their lower variability will facilitate the performance algorithm. It is however possible to apply it at grid cell level but it should then be kept in mind that the spatial coherence is not ensured so that a later aggregation may yields irrealistic results.

It is important to note that - while similar in principle - the implementation detail of the method is different for wind and solar energy. The reason is that the daily climate projection values are instantaneous values with a sampling period of 24 hours while solar data are daily averages. As a result, two separate notebooks have been made for the two energy sources. The present notebook deals with wind energy.

The limitations of the algorithm are the following:
 - the output of the algorithm doesn't ensure a spatial coherence: it should thus be applied at the end of the chain
 - there is no coherence among the difference energy sources: there is no guarantee that the downscaled wind power capacity factor is coherent with a downscaled solar PV capacity factor.
 - the output of the algorithm should be considered as scenario or synthetic data: it is a possible time series that has the desired properties at daily scales but there are many other potential time series.
 - if climate projection includes sequences that are not present in the past, the algorithm will fail
 
 The data used in this notebook can be downloaded on the following link:
 https://cloud.minesparis.psl.eu/index.php/s/WjqM1HqQRuuWdXU
