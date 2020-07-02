## Overview

Welcome to Canadian Hydroden Intensity Mapping Experiment / Fast Radio Bursts (CHIME/FRB) open data releases. The purposes of this page is to be the central resource for accessing data and accompanying code snippets to get started working with the data.

If you make use of these datasets for academic work, please cite the respective papers.

## Data Releases
###### [Detection of Fast Radio Bursts at Radio Frequencies Down to 400 MHz](https://arxiv.org/abs/1901.04524)

Data for the the 13 FRBs presented is this publication can be found [here](http://www.canfar.net/citation/landing?doi=19.0004).

###### [A Second Repeating Fast Radio Burst](https://arxiv.org/abs/1901.04525)

All of the repeater data for this publication can be downloaded from [here](http://www.canfar.net/citation/landing?doi=19.0005).

###### [Periodic activity from a fast radio burst source](https://arxiv.org/pdf/2001.10275.pdf)

All of the bursts included in this publication can be downloaded [here](https://doi.org/10.11570/20.0002)

## Data Format
The CHIME/FRB Experiment uses the msgpack data format to store channelized intensity data while CHIME/Pulsar Experiment uses the filterbank format.

###### msgpack
`msgpack` data is the beamformed and channelized intensity data which consists of 16k frequency channels at 1-ms cadence. This data is scaled, offset, and packed into 8-bit integers files each consisting of 1.00663296s worth of data. For more information refer to [The CHIME Fast Radio Burst Project: System Overview](https://arxiv.org/pdf/1803.11235.pdf). In order to read and uncompress the msgpack data into numpy arrays, the [cfod](https://github.com/chime-frb-open-data/chime-frb-open-data) python package can be used.

###### filterbank
Filterbank data for the FRBs presented in the data release were analyzed using `presto` and `sigproc`.

###### npz
A dictionary-like object with lazy-loading of files in the zipped archive, for further reading see [official numpy documentation](https://numpy.org/doc/stable/reference/generated/numpy.load.html).

Burst dynamic spectra (waterfalls) for "Periodic activity from a fast radio burst source", both from intensity and baseband data, are stored in npz files.

The waterfalls from intensity data have file names `burst_*_16k_wfall.npz` and are stored at the full resolution of 16,384 frequency channels over 400 MHz with a `0.00098304s` time resolution, dedispersed to 348.82 pc cm-3. 

The waterfalls derived from complex voltage (baseband) data have file names `burst_*_bb_1k_wfall.npz` and are stored at a resolution of 1,024 frequency channels over 400 MHz with time resolution and dedispersed to the DM as in Extended Data Figure 1 of the paper: {40.96, 40.96, 20.48, 81.92} us and {348.78, 348.82, 348.82, 348.86} pc cm-3. In all cases zapped channels due to RFI are replaced by `np.nan`. 

Note that the bursts are too dim too see in individual frequency channels at full resolution. In the paper, we have downsampled the data in frequency for visualization.

Data can be accessed and displayed in Python as, e.g.:
```python
import matplotlib.pyplot as plt
import numpy as np

fname = "burst_9_bb_1k_wfall.npz"
data = np.load(fname)
wfall = data["wfall"]
dt_s = data["dt_s"]
center_freq_mhz = data["center_freq_mhz"]
df_mhz = center_freq_mhz[1] - center_freq_mhz[0]
plt.imshow(
    wfall,
    origin="lower",
    aspect="auto",
    interpolation="nearest", 
    extent=(0, dt_s*wfall.shape[1], center_freq_mhz[0]-df_mhz/2.,center_freq_mhz[-1]+df_mhz/2.)
)
plt.xlabel("Time [s]")
plt.ylabel("Frequency [MHz]")
```
## Support
As a first step, make sure to read through the [issues](https://github.com/chime-frb-open-data/chime-frb-open-data.github.io/issues?q=) to see if your specific question has already been answered. If not, open a new issue, and we would be happy to help you.
