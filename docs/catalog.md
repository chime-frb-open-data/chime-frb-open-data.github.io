If you want to take a closer look at the CHIME/FRB Catalog 1 Data, you'll need to know how to read it in. We've made both a CSV and FITS file version of Catalog 1 data available. You'll need the Python packages: **numpy** and **pandas** to open the CSV version or just **astropy** to open the FITS version. See below for sample scripts to open both versions. 

??? Example

    === "Read in CSV version"

        ```python
        import numpy as np
        import pandas as pd
        # csv file columns (extracted directly from the list that is presented in the Catalog 1 paper)
        col_list = ['tns_name', 'previous_name','repeater_name','ra','ra_err','ra_notes','dec','dec_err','dec_notes',
            'gl','gb','exp_up','exp_up_err','exp_up_notes','exp_low','exp_low_err', 'exp_low_notes', 'bonsai_snr',
            'bonsai_dm','low_ft_68','up_ft_68','low_ft_95','up_ft_95','snr_fitb','dm_fitb','dm_fitb_err',
            'dm_exc_ne2001','dm_exc_ymw16','bc_width','scat_time','scat_time_err','flux','flux_err','flux_notes',
            'fluence','fluence_err','fluence_notes','sub_num','mjd_400','mjd_400_err','mjd_inf','mjd_inf_err',
            'width_fitb','width_fitb_err','sp_idx','sp_idx_err','sp_run','sp_run_err',
            'high_freq','low_freq','peak_freq', 'excluded_flag']  
        # reading in the csv file
        df = pd.read_csv("catalog1.csv",usecols=col_list)
        # The Catalog 1 Data now lives in df. You can view they keys for examples with: 
        print(df.keys())
        ```

    === "Read in FITS version"

        ```python
        # use the fits file reader in astropy
        from astropy.io import fits
        fits_catalog = "catalog1.fits"
        # open the fits file with a context manager, i.e., using with
        with fits.open(fits_catalog) as hdul:
        # The Catalog 1 Data now lives in hdul. You can view more information about the file with: 
            hdul.info()        
        ```

Tutorial provided by Alice Curtin and Sabrina Berger.