opac.yaml
^^^^^^^^^
The preproccessing of exorad using the python package ``exorad_opac`` requires a parameter file with the name ``opac.yaml`` in the ``input`` folder.

The ``opac.yaml`` comes with three mandatory sections:

 - The ``star`` section sets the stellar parameters (as needed for the radiative transfer)
 - The ``temp_init`` section which specifies the input temperature profile for MITgcm
 - The ``press_init`` section which sets the vertical pressure grid of MITgcm
 - (optionally) The ``grid`` section which sets properties on the opacity grid
 - (optionally) The ``chemistry`` section which sets properties on the chemistry for the opacities
 - (optionally) The ``prt`` section which sets arguments for ``petitRADTRANS`` (when creating the ``Radtrans`` object)

Many parameters have defaults that ``exorad_opac`` will fallback to if those are not set in ``opac.yaml``.
Checkout ``DEFAULT_<name>`` values in ``config.py``.

An example ``opac.yaml`` file can look like this:

.. code:: yaml

    star:   # Section containing the stellar properties of the host star
      Tstar: 4520
      Rstar: 0.667
      semimajoraxis: 0.01526
      flux_scaling: BB

    temp_init:   # Section containing the temperature init of MITgcm
      theta_deep: 1400
      warn_theta_inversion: False

    press_init:  # Section containing the vertical grid of MITgcm
      np_log: 41
      zero_eps: 2.0e-5
      p0: 700

    chemistry:  # Section containing FeH and CO for chemical equilibrium in opacities
      FeH: 0.0
      CO: 0.55
      use_temp_init: True

    grid:  # Section containing arguments for the temperature gridding in the opacity grid

    prt:  # Section containing arguments of the Radtrans object of petitRADTRANS


``expeRT/MITgcm`` needs to know and set the incident stellar flux.
The ``star`` section in ``opac.yaml`` helps to compute that.

.. list-table:: Paramters in ``opac.yaml`` - ``star``
   :widths: auto
   :header-rows: 1

   * - Parameter
     - Unit
     - Meaning
   * - ``Tstar``
     - K
     - Temperature of the host star
   * - ``Rstar``
     - solar radii
     - radius of the host star
   * - ``semimajoraxis``
     - AU
     - orbital seperation of planet
   * -  ``flux_scaling``
     - str, or erg/cm^2/s/Hz
     - the scaling of the stellar intensity. Per default it scales to a black body, can be a specific value as well
 
 

There is a large variety on possible input parameters for the initial temperature profile.
The default temperature profile will equal to the profile used in Schneider et al. (2022) with the minimum parameter to set: ``theta_deep``.

All parameters are passed down to ``ic.py`` to the function ``calc_init_temperature()``, where many parameters can be hidden in ``**kwargs``.

.. list-table:: Paramters in ``opac.yaml`` - ``temp_init``
   :widths: auto
   :header-rows: 1

   * - Parameter
     - Unit
     - Meaning
   * - ``theta_deep``
     - K
     - Temperature of the initial temperature profile at 1 bar (adiabat correction)
   * - ``warn_theta_inversion``
     - bool
     - default to ``False``. If true, it will fail, when there is an inversion in the temperature profile at depth.
   * - ``profile``
     - string
     - "guillot", "parmentier", "heng" or "isothermal" for guillot2010 or parmentier2015 or heng2011 or isothermal models respectively.
   * - ``use_thorngren``
     - bool
     - Use or dont use the intirior temperature fit from Thorngren et al. (2018) as a guess for the initial intirior temp.
   * - ``T_int``
     - K
     - Manually chosen T_int
   * - ``p_min_deep``, ``p_max_deep``
     - bar
     - minimal and maximal pressure between which the adiabat is interpolated to the upper atmosphere temperature
   * - more
     -
     - see the individual temperature functions (``isothermal``, ``heng``, ``guillot_global``, ``tp_parmentier``) in ``ic.py`` for more parameters

Opacities are computed by assuming chemical equilibrium applying the equilibrium interpolator of petitRADTRANS.
There are two parameters that you can change for that:

 .. list-table:: Paramters in ``opac.yaml`` - ``chemistry``
    :widths: auto
    :header-rows: 1

    * - Parameter
      - Unit
      - Meaning
    * - ``CO``
      - number
      - C/O ratio (default=0.55 - solar)
    * - ``FeH``
      - log
      - log metalicity (default=0.0 - solar)
    * - ``use_temp_init``
      - bool
      - Use the initial temperature profile to calculate the MMW and cp


There are also a few parameters that can be tweaked for the temperature gridding in the final opacity grid.
These things are set in the ``grid`` section of ``opac.yaml``.

.. list-table:: Paramters in ``opac.yaml`` - ``grid``
   :widths: auto
   :header-rows: 1

   * - Parameter
     - Unit
     - Meaning
   * - ``tmin``
     - K
     - Minimal temperature of the precalculated opacity grid 
   * - ``tmax``
     - K
     - Maximum temperature of the precalculated opacity grid
   * - ``tresolution``
     - int
     - Temperatureresolution of the precalculated opacity grid (e.g., how many T-points)

The vertical coordinate in MITgcm can be set via the ``press_init`` section. These are the arguments:

.. list-table:: Paramters in ``opac.yaml`` - ``press_init``
   :widths: auto
   :header-rows: 1

   * - Parameter
     - Unit
     - Meaning
   * - ``np_log``
     - int
     - Number of logarithmic vertical layers
   * - ``zero_eps``
     - bar
     - Difference between the upper most pressure point to 0
   * - ``p0``
     - bar
     - boundary pressure
   * - ``dp_lin``
     - bar
     - Pressure differences in the linear part of the grid


.. warning:: Do not forget to match ``SIZE.h``!


``exorad_opac`` uses `petitRADTRANS <https://petitradtrans.readthedocs.io/en/latest/>`_ to create opacities.
You can initialise ``exorad_opac`` in the same way as you would use ``petitRADTRANS``.
It is easily possible to specify line species, wlen regions, etc or whatever in the ``prt`` section of ``opac.yaml``.
Some parameters have default values in ``exorad_opac``.
You can easily overwrite these values if you use your own values in ``opac.yaml``.

These are the default values:

.. code:: yaml

   prt:
       line_species: ['H2O_Exomol', 'Na_allard', 'K_allard', 'CO2', 'CH4', 'NH3', 'CO_all_iso_Chubb', 'H2S', 'HCN', 'SiO', 'PH3', 'TiO_all_Exomol', 'VO', 'FeH']
       rayleigh_species: ['H2', 'He']
       continuum_opacities: ['H2-H2', 'H2-He', 'H-']
       wlen_bords_micron: [0.2, 100.]
       # + any other argument of the Radtrans class

This is equivilant to

.. code:: python

    from petitRADTRANS import Radtrans

    line_species= ['H2O_Exomol', 'Na_allard', 'K_allard', 'CO2', 'CH4', 'NH3', 'CO_all_iso_Chubb', 'H2S', 'HCN', 'SiO', 'PH3', 'TiO_all_Exomol', 'VO', 'FeH']
    rayleigh_species= ['H2', 'He']
    continuum_opacities= ['H2-H2', 'H2-He', 'H-']
    wlen_bords_micron= [0.2, 100.]

    atmosphere = Radtrans(..., line_species=line_species, rayleigh_species=rayleigh_species, continuum_opacities=continuum_opacities, wlen_bords_micron=wlen_bords_micron)
