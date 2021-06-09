## Release 01 | [FRBs @ 400MHz](https://arxiv.org/abs/1901.04524)
For reading `msgpack` data provided in this release, headover to the [CHIME/FRB Open Data](https://github.com/chime-frb-open-data/chime-frb-open-data) Python package. 

??? Example

    === "Single File"

        ```python
        from cfod.analysis.intensity import chime_intensity as ci
        fn = `astro_5941664_20180406203904337770_beam0147_00245439_02.msgpack`
        intensity, weights, fpga0, fpgaN, binning, frame0_nano, nrfifreq, rfi_mask = ci.unpack_data(fn)
        ```

    === "Multiple files"

        ```python
        from cfod.analysis.intensity import chime_intensity as ci
        fns = ['file1', 'file2', 'file3']
        intensity, weights, fpga0s, fpgaNs, binning, rfi_mask, frame0_nanos = ci.unpack_datafiles(fns)
        ```

??? hint
    where,

    - `intensity` is a 2D Intensity array.
    - `weights ` are the corresponding 2D array weights to the intensity array.
    - `fpga0 (int)` is start fpga count of the data chunk. (Internally used to track time, can be ignored). The fpga count increments at the rate of 2.56us.
    - `fpgaN (int)` is number of fpga counts in the data chunk read
    - `binning (int)` is the downsampling of the data from the ringbuffer
    - `frame0_nano` is the conversion from fpga timestamp to utc timestamp (Currently not supported.)
    - `nrfifreq` is the number of frequences masked by the realtime rfi system (Currently not supported.)
    - `rfi_mask` is currently not supported


## Release 03 | [Periodic FRB](https://arxiv.org/abs/2001.10275)
The burst dynamic spectra (waterfalls) for this release constitutes of both intensity and baseband data, stored in `npz` files. 

### Intensity Data
The waterfalls from intensity data have file names `burst_*_16k_wfall.npz` and are stored at the full resolution of `16,384` frequency channels over 400 MHz with a `0.00098304s` time resolution, dedispersed to `348.82 pc cm-3.`

### Baseband Data
The waterfalls derived from complex voltage (baseband) data have file names `burst_*_bb_1k_wfall.npz` and are stored at a resolution of 1,024 frequency channels over 400 MHz with time resolution and dedispersed to the DM as in `Extended Data Figure 1` of the paper: {40.96, 40.96, 20.48, 81.92}us and {348.78, 348.82, 348.82, 348.86} pc cm-3. In all cases zapped channels due to RFI are replaced by `np.nan`.

Note that the bursts are too dim too see in individual frequency channels at full resolution. In the paper, we have downsampled the data in frequency for visualization.

Data can be accessed and displayed in Python as, e.g.:
???+ Example

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


## Release 04 | [Galactic Magnetar](https://arxiv.org/abs/2005.10324)
### CHIME/FRB Detection
These files for this data release have names `chimefrb_SGR1935+2154_20200428_B????.npz` where `B????`
corresponds to the CHIME/FRB beam that recorded that data. The highest S/N detection was made by beam `2067`.

The data have a `1024` frequency channels over 400 MHz with time resolution of 
`0.98304ms` and are dedispersed to `332.7206 pc cm-3`. In all cases zapped 
channels due to RFI are replaced by `np.nan`. Data can be accessed and 
displayed in Python as using the following code,

??? Example

    ```python
    import glob
    import matplotlib.pyplot as plt
    import numpy as np
    fnames = glob.glob("chimefrb_SGR1935+2154_20200428_B????.npz")
    for fname in fnames:
        data = np.load(fname)
        print(data.files)
        intensity = data["intensity"]
        times = data["times"]
        frequencies = data["frequencies"]
        plt.figure()
        plt.imshow(intensity, aspect="auto", origin="lower", interpolation="nearest")
        plt.show()
    ```

The NumPy arrays stored in the npz files are:

??? hint
    - `center_frequencies`: center frequency of each channel, in MHz
    - `center_time`: center time of each sample, in s
    - `df`: channel bandwidth, in MHz
    - `dm`: dispersion measure, in pc cm-3
    - `dt`: sampling time, in s
    - `fbottom`: frequency at the bottom of the band, in MHz
    - `frequencies`: lower edge of each channel, in MHz
    - `ftop`: frequency at the top of the band, in MHz
    - `intensity`: burst dynamic spectrum
    - `nchan`: number of channels
    - `nsamp`: number of samples
    - `tend`: end of the samples, in s
    - `times`: left edge of each sample, in s
    - `tstart`: start of the samples, in s



### Algonquin Radio Observatory Detection

This data has been recorded with the 10-m dish at the Algonquin Radio Observatory and is named, `aro_SGR1935+2154_20200428_baseband.npz`

The NumPy arrays stored in the npz file are:

??? hint
    - `V` : coherently dedispersed complex voltages, with shape (nt, nf, npol)
    - `start_time` : start time of the observation, referenced to 800. MHz
    - `end_time` : end time of the observation, referenced to 800. MHz
    - `DM` : dispersion measure, in pc cm-3, to which the data is coherently dedispersed


Note that the DM to which the data has been coherently dispersed, 
`332.80925424 pc cm-3`, is slightly different than the optimal DM measured by
CHIME, `332.7206 pc cm-3`.

Below is an example of reading in complex voltages, determining Stokes 
parameters, and plotting the total intensity:

??? Example

    ```python
    import matplotlib.pyplot as plt
    import numpy as np

    def get_stokes(data):
        X = data[:,:,0]
        Y = data[:,:,1]
        I = abs(X) ** 2 + abs(Y) ** 2
        Q = abs(X) ** 2 - abs(Y) ** 2
        U = 2 * np.real(X * np.conj(Y))
        V = -2 * np.imag(X * np.conj(Y))
        return I, Q, U, V

    data = np.load("aro_SGR1935+2154_20200428_baseband.npz")
    print(data.files)
    cv = data["V"]

    # complex voltages are shaped (nt, nf, npol)
    nt, nf, _ = cv.shape
    tstart = np.datetime64(str(data["start_time"]))
    tstop = np.datetime64(str(data["stop_time"]))
    dt = (tstop - tstart) / nt
    dm = data["DM"]
    freq = np.linspace(800, 400, 1024, endpoint=False)[::-1]
    I, Q, U, V = get_stokes(cv)

    # change shape to (nf, nt) with the bottom frequency at index 0
    intensity = np.flipud(I.T)

    # self-calibrate data
    for ii in range(nf):
        chan = intensity[ii,:]
        if np.nansum(chan) == 0.:
            continue
        mean = np.nanmean(chan)
        chan[:] = chan[:] / mean
        chan[:] = chan[:] - 1
        var = np.nanvar(chan)
        chan[:] = chan[:] / var

    # downsampling factors
    ds = 384
    sub_factor = 4

    # downsample if necessary
    if ds > 1:
        new_num_spectra = int(nt / ds)
        num_to_trim = nt % ds
        if num_to_trim > 0:
            intensity = intensity[:,:-num_to_trim]
        intensity = np.array(np.column_stack(
            [np.mean(intensities, axis=1) for intensities \
                in np.hsplit(intensity, new_num_spectra)]))
        nf, nt = intensity.shape

    # subband if necessary
    if sub_factor > 1:
        intensity = np.nanmean(intensity.reshape(
            -1, sub_factor, intensity.shape[1]), axis=1)
        freq = np.nanmean(freq.reshape(-1, sub_factor), axis=1)
    time_s = np.arange(tstart, tstop - dt * ds, dt * ds)
    variance = np.nanvar(intensity, axis=1)

    # zap outlier channels
    intensity[variance > 0.004, ...] = 0.

    # plot waterfall
    plt.imshow(intensity, origin="lower", interpolation="nearest", aspect="auto")
    plt.savefig("aro_wfall.png")
    ```