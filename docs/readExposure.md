## Read in Exposure Map Data

??? Example

    === "Creating an exposure map for both upper and lower transits"

        ```python
        # import your packages here
        import numpy as np
        import matplotlib.pyplot as plt
        import healpy as hp
        from astropy.coordinates import SkyCoord
        import astropy.units as u

        fname_u = "exposure_int_20180828_20191001_transit_U_beam_FWHM-600_res_4s_0.86_arcmin.npz"
        fname_l = "exposure_int_20180828_20191001_transit_L_beam_FWHM-600_res_4s_0.86_arcmin.npz"
        
        with np.load(fname_u) as data:
            exposure = data["exposure"]
        
        #setting parameters for map resolution 

        # spatial
        nside = 4096
        npix = hp.nside2npix(nside)

        # temporal
        t_res = 4 
        
        # Initializing a healpy map
        hpxmap = np.zeros(npix, dtype=np.float) 
        hpxmap[0:len(exposure)] += t_res * exposure/(3600.) #seconds to hours
        hpxmap[hpxmap==0] = hp.UNSEEN #masking pixels with zero exposure
        
        # Plotting
        hp.mollview(hpxmap, coord=['C','G'], norm='log', unit="Hours")
        ![high res image](hires.png)
        # Check exposure time in hours for R1 repeater
        coord = SkyCoord("05:31:58.70", "+33:08:52.5", frame='icrs', unit = u.deg)
        print("Exposure (in hours): %.2f"%hpxmap[hp.ang2pix(nside, coord.ra.deg, coord.dec.deg,  lonlat=True)])


        ### Obtaining a lower resolution map ###
        nside_out = 1024 
        print("Resolution of new map : %.2f arcmin"%(hp.nside2resol(nside_out, arcmin=True)))
        # Degrade healpix resolution to nside_out
        hpxmap_dg = hp.ud_grade(hpxmap, nside_out) 
        hp.mollview(hpxmap_dg, coord=['C','G'], norm='log', unit="Hours")
        ![low res image](lowerres.png)
        ```

??? hint
    where,

    - `nside_out` Varying nside_out parameter below will change the resolution. The nside parameter for the current map is 4096. You can switch to a lower value. However, do not use an nside lower than 512 as you would not be nyquist sampling the CHIME/FRB beam pattern in that case.
    - `hpxmap` Your HEALpix map will live here.
    - `hpxmap_dg` Your downgraded HEALpix map will live here.
    - `hp.mollview` Plots a Mollweide projection of your HEALpix map.
