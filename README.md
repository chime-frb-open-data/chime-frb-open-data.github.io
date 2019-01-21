## Overview

Welcome to Canadian Hydroden Intensity Mapping Experiment / Fast Radio Bursts (CHIME/FRB) open data releases. The purposes of this page is to be the central resource for accessing data and accompanying code snippets to get started working with the data.

## Data Releases

###### [Detection of Fast Radio Bursts at Radio Frequencies Down to 400 MHz](https://arxiv.org/pdf/1901.04524.pdf)

[FRB Data](http://www.canfar.net/storage/list/AstroDataCitationDOI/CISTI.CANFAR/19.0004)

###### [A Second Repeating Fast Radio Burst](https://arxiv.org/pdf/1901.04525.pdf)

[Repeater Data](http://www.canfar.net/storage/list/AstroDataCitationDOI/CISTI.CANFAR/19.0005)

## Data Format
The CHIME/FRB Experiment uses the msgpack data format to store channelized intensity data while CHIME/Pulsar Experiment uses the filterbank format.

###### msgpack
`msgpack` data is the beamformed and channelized intensity data which consists of 16k frequency channels at 1-ms cadence. This data is scaled, offset, and packed into 8-bit integers files each consisting of 1.00663296s worth of data. For more information refer to [The CHIME Fast Radio Burst Project: System Overview](https://arxiv.org/pdf/1803.11235.pdf). In order to read and uncompress the msgpack data into numpy arrays, the following [python package]() can be used.


###### filterbank
Filterbank data for the FRBs presented in the data release were analyzed using `presto` and `sigproc`.

## Support or Contact
For any further information or questions please contact charanjot.brar@mcgill.ca
