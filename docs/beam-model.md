*Author: Marcus Merryfield*

The beam model used for various analyses in the First CHIME/FRB Catalog is now available to download. The instructions for installation may be found in the CHIME/FRB beam model [GitHub repository](https://github.com/chime-frb-open-data/chime-frb-beam-model). This tutorial will guide the user on some useful routines the beam model can perform. Note that this release of the beam model only encompasses the version used for analysis in the First CHIME/FRB Catalog. The CHIME primary beam model is still being improved (see e.g. https://arxiv.org/abs/2201.11822), and as such the CHIME/FRB composite beam model (which includes both the CHIME/FRB synthesized FFT beams and the CHIME primary beam) will likely be updated in the future.

TODO: Primer on beam coordinates? I remember there was a primer somewhere with nice graphics of how the coordinate system worked, but I can't remember where it is...

## Generating beam model sensitivities

When generating sensitivities from the CHIME/FRB beam model, there are three important inputs to `get_sensitivity`. The first is the length `N` array of beam numbers for which you want to calculate sensitivities. The second is the `(M,2)` array of beam model `(x,y)` coordinate positions for which you want to calculate sensitivities. The last is the length `K` array of frequencies for which to calculate sensitivities. The returned array of sensitivities has shape `(M,N,K)`. 

In the following example, we'll calculate sensitivities for a single beam, at one coordinate position, and for all 16384 CHIME/FRB frequencies.

???+ Example "Beam coordinates"
    ```python
    import numpy as np
    import cfbm

    # First create a beam model object of the CHIME/FRB composite beam model
    bm = cfbm.CompositeBeamModel()

    # First choose a beam number (here we'll use only one)
    beam_no = [1064]

    # Next choose a position in the beam. We'll use the center of our chosen beam
    # at 600 MHz (note the beam shape & size change as a function of frequency)
    pos = bm.get_beam_positions(beam_no, freqs=[600])

    # In this example, squeeze the unused array dimensions, as get_beam_positions 
    # returns an NxMx2 array where N is the length of the beam number array and M 
    # is the length of the frequencies array
    pos = np.squeeze(pos)

    # The last ingredient is a list of frequencies: we'll get beam sensitivities
    # for all 16k frequencies from 400 to 800 Mhz
    freqs = np.linspace(400, 800, 16384)

    # Finally, calculate the sensitivities
    sensitivities = bm.get_sensitivity(beam_no, pos, freqs)

    # Once again, in this example we'll squeeze unused array dimensions
    sensitivities = np.squeeze(sensitivities)
    
    # Now we have our array of sensitivities representing the beam sensitivity in
    # beam 1064, at 16384 frequencies from 400 to 800 MHz, and at the position of the
    # 600 MHz beam center!
    ```

Oftentimes you may want to calculate sensitivities like in the above example, but for (RA, Dec) pairs instead of beam model coordinate pairs. Fortunately there are functions in `cfbm` to convert between beam model coordinates and sky coordinates given a datetime. `cfbm.get_equatorial_from_position` converts beam position to equatorial coordinates given a time, and `cfbm.get_position_from_equatorial` does the reverse.

???+ Example "Sky coordinates"
    ```python
    import datetime
    import numpy as np
    import cfbm

    # Create a beam model, choose a beam number, and generate the frequencies as before
    bm = cfbm.CompositeBeamModel()
    beam_no = [1064]
    freqs = np.linspace(400, 800, 16384)

    # Setting the RA and Dec coordinates, choosing a time, and getting the corresponding
    # x and y coordinates
    ra = 370.7
    dec = 24.0
    t = datetime(year=2022, month=1, day=1)
    x, y = cfbm.get_position_from_equatorial(ra, dec, t)
    pos = np.array([x,y])

    # Finally, get the beam sensitivities with the (x,y) position and squeeze
    # unused dimensions
    sensitivities = bm.get_sensitivity(beam_no, pos, freqs)
    sensitivities = np.squeeze(sensitivities)
    ```