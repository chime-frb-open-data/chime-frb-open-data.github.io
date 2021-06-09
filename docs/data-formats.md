The CHIME/FRB Experiment either the `msgpack` data format to store raw channelized intensity data, `npz` file format for processed intensity or baseband data and the CHIME/Pulsar Experiment uses the `filterbank` data format.
For more information on the instrument parameters refer to [The CHIME Fast Radio Burst Project: System Overview](https://arxiv.org/pdf/1803.11235.pdf). 

## msgpack
`msgpack` data is the beamformed and channelized intensity data which consists of `16384` frequency channels at `1ms` cadence. This data is scaled, offset, and packed into 8-bit integers files each consisting of `1.00663296s` worth of data. 

In order to read and uncompress the msgpack data into numpy arrays, checkout the [`cfod`](https://github.com/chime-frb-open-data/chime-frb-open-data) python package.

## filterbank
Filterbank data for the fast radio bursts presented in the data release were analyzed using pubicly availaible packages [`presto`](https://www.cv.nrao.edu/~sransom/presto/) and [`sigproc`](http://sigproc.sourceforge.net).

## npz
A dictionary-like object with lazy-loading of files in the zipped archive, for further reading see [official numpy documentation](https://numpy.org/doc/stable/reference/generated/numpy.load.html). See the [code snippets](code.md) section for more details on paper specific details.

## h5
The Hierarchical Data Format version 5 (`HDF5`), is an open source file format that supports large, complex, heterogeneous data. HDF5 uses a file directory like structure that allows you to organize data within the file in many different structured ways, as you might do with files on your computer