## Overview

Welcome to Canadian Hydroden Intensity Mapping Experiment / Fast Radio Bursts (CHIME/FRB) open data releases. The purposes of this page is to be the central resource for accessing data and accompanying code snippets to get started working with the data.

If you make use of these datasets for academic work, please cite the respective papers.

## Data Releases
###### [Detection of Fast Radio Bursts at Radio Frequencies Down to 400 MHz](https://arxiv.org/abs/1901.04524)

Data for the the 13 FRBs presented is this publication can be found [here](http://www.canfar.net/storage/list/AstroDataCitationDOI/CISTI.CANFAR/19.0004/data).

###### [A Second Repeating Fast Radio Burst](https://arxiv.org/abs/1901.04525)

All of the repeater data for this publication can be downloaded from [here](http://www.canfar.net/storage/list/AstroDataCitationDOI/CISTI.CANFAR/19.0005/data).

## Data Format
The CHIME/FRB Experiment uses the msgpack data format to store channelized intensity data while CHIME/Pulsar Experiment uses the filterbank format.

###### msgpack
`msgpack` data is the beamformed and channelized intensity data which consists of 16k frequency channels at 1-ms cadence. This data is scaled, offset, and packed into 8-bit integers files each consisting of 1.00663296s worth of data. For more information refer to [The CHIME Fast Radio Burst Project: System Overview](https://arxiv.org/pdf/1803.11235.pdf). In order to read and uncompress the msgpack data into numpy arrays, the [cfod](https://github.com/chime-frb-open-data/chime-frb-open-data) python package can be used.

###### filterbank
Filterbank data for the FRBs presented in the data release were analyzed using `presto` and `sigproc`.

## Support or Contact
For any further information or questions please email charanjot.brar@mcgill.ca
