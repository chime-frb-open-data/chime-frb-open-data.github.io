*Author: Marcus Merryfield*

The injections dataset used for catalog 1 is separated into two files. The first encompasses ~5 million synthetic FRBs which were generated from population distributions based on The First CHIME/FRB Catalog. The second encompasses a subset of these 5 million bursts (~85,000 total) which were actually injected into the live CHIME/FRB intensity data stream. The first file is available as an `hdf5` file, while the second is available as a pickle file intended to be read by the [`pandas`
library](https://pandas.pydata.org/) as a `DataFrame`.

???+ Example "Read in the CHIME/FRB injection data"

    === "HDF5"
        ```python
        import hdf5
        ```

    === "pickle"
        ```python
        import pandas as pd

        # Read in the pickle file as a pandas DataFrame
        data = pd.read_pickle("first_catalog_injections.p")

        # Now separate the data into categories: detected, non-detected, and RFI
        snr_threshold = 9.
        rfi_threshold = 5.
        high_snr_override = 100.

        # Make a detection mask following CHIME/FRB pipeline logic
        detected_mask = np.logical_and.reduce(
            (
                data.bonsai_snr.values > snr_threshold,
                np.logical_or(
                    data.l2_rfi_grade.values > rfi_threshold,
                    data.bonsai_snr.values > high_snr_override
                )
            )
        )

        # Detected & non-detected injections
        det = data[detected_mask]
        nondet = data[~detected_mask]

        # Make an RFI mask, where RFI are the subset of non-detections which had
        # SNRs above the SNR threshold
        rfi_mask = np.logical_and(
            pd.notna(nondet.l2_rfi_grade),
            nondet.bonsai_snr.values > snr_threshold
        )

        # RFI injections & update non-detected injections
        rfi = nondet[rfi_mask]
        nondet = nondet[~rfi_mask]
        ```