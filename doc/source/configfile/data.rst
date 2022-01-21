data
^^^^

The data file of ``exoradPRT/MITgcm`` is identical to the datafile of ``MITgcm``.

There are a few things that one should change when modelling different hot Jupiters:

.. list-table:: Paramters in ``data``
   :widths: auto
   :header-rows: 1

   * - Parameter
     - Meaning
   * - ``rotationperiod``
     - Value for the orbital period (in s) of the planet
   * - ``gravity``
     - Surface gravity value (constant)
   * - ``atm_po``
     - bottom pressure of the model
   * - ``atm_rd``
     - specific gas constant (set by ``exorad_opac``)
   * - ``atm_cp``
     - heat capacity at constant pressure (set by ``exorad_opac``)
   * - ``r0_sealevel``
     - bottom pressure of the model (make sure that there is no conflict with ``atm_p0``)
   * - ``rsphere``
     - radius of the planet
   * - ``radius_fromhorizgrid``
     - radius of the grid input file. If using MITgcm files, be sure to use the earth radius here!
   * - ``delr``
     - delta pressures, automatically adjusted by ``exorad_opac``


There are probably a few more things in here, that are not mentioned. Be sure to base your data file on the data file supplied.