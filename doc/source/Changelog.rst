Changelog
---------

Here is an uncomplete changelog that reports on changes to exorad

easychem
^^^^^^^^
`merged 23.11.2023 <https://github.com/exorad/exorad/pull/45>`_

* replaces the interpolation of chemical abundancies by a proper eq.chem calculation following easychem.
* Also adds tqdm timers to the preprocessing for better progress reports
* Note: Eq. chem calculations take ~5mins

k-table mixing
^^^^^^^^^^^^^^
`merged 27.10.2023 <https://github.com/exorad/exorad/pull/45>`_

* Discribed and discussed in Schneider et al. (2023)
* the exorad_opac writer is now written in a more generalized way, reducing possible errors, when extending in the future
* From now on we will always seperate line opacities and continuum absorption opacities. Line opacities are now in opac_kappa_abs files, and continuum opacities are in opac_kappa_cont files! Note that we need both!! from now on.
* In case of mixing, we need to set the mixmethod and chemistry method in data.exoprt

Eccentricity
^^^^^^^^^^^^
`merged 25.03.2023 <https://github.com/exorad/exorad/pull/39>`_

Mathijs implementation of the Eccentricity and non sync rotaion in exorad

Flux scaling + rewrite of exorad_opac
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
`merged 13.03.2023 <https://github.com/exorad/exorad/pull/43>`_
`merged 08.03.2023 <https://github.com/exorad/exorad/pull/41>`_

Big cleanup in exorad_opac

There are two new features that change the way that exorad_opac works:

The stellar flux can now (by default) automatically be scaled to the black body of the input stellar spectrum -> better accuracy by ~5%!! Especially for low resolution opacities and stellar spectra!
(See Fig. B1, column 1 in Schneider et al. 2022a for what the problem with the current implementation is)
The MMW and cp are now selfconsistently calculated from the initial temperature profile instead of from a mean of the chemistry/opacity grid
Here is how the behaviour can be changed in the opac.yaml:

.. code:: yaml

    ...
    star:
      ...
      flux_scaling: 'BB'  # new default = blackbody scaling, possible values: False (old behaviour) or value 
    ...
    chemistry:
      ...
      use_temp_init: True # new default = use temp_init, set to false for old behaviour

Furthermore:

The flux integration in expeRT uses the information about the cell edges of the frequency grid.
This ensures that the scaling in exorad_opac is now also using the cell edges of the frequency grid, therefore being more consistent with expeRT. This way of integration preserves the outcome of the bolometric flux in expeRT.


State of Schneider et al. (2022)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Including new sponge layer and radiative transfer from petitRADTRANS