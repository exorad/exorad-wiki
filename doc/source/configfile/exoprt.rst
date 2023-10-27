data.exoprt
^^^^^^^^^^^

The ``data.exoprt`` file is used to control the radiative transfer bit of ``expeRT/MITgcm``.
This is an example file:

.. code::

    C--   Full radiative transfer input:
    # EXORAD_DAY_DT :: radiative timestep for dayside (needs to be multiple of deltaT)
    # EXORAD_NIGHT_DT :: radiative timestep for nightside (needs to be multiple of deltaT)
    # EXORAD_INTERP_STEP :: radiative interpolation step (should be 1 or 2). 1 = No interpolation, 2 = every second column
    # EXORAD_SCATTERING  :: scattering flag. If set to False, we will not use scattering. Warning: only set to false if photon_destruct_prob = 1 and opac_scat = 0!
    # EXORAD_MAX_I_ITER_SCAT :: max. amount of iterations performed to converge the source function
    # EXORAD_MAX_REL_DELTA_S :: convergence criterion for the convergence of the source function
    # EXORAD_MIN_TEMP :: minimal temperature, where the GCM will stop forcing to cooler temperatures (we dont want negative temperatures)
    # EXORAD_INTERP_METH :: interpolatation method of temperature to cell interfaces, if you set it to one, it will use the Bezier interpolation, else it will use a linear interpolation
    # EXORAD_TINT :: interior temperature to force bottom boundary flux. If set to neg. values, it will not set the boundary fluxes.

     &EXORAD_PRT
     EXORAD_deltaT = 100.0,
     EXORAD_INTERP_STEP = 2,
     EXORAD_SCATTERING = .TRUE.,
     EXORAD_MAX_I_ITER_SCAT = 500,
     EXORAD_MAX_REL_DELTA_S = 0.02,
     EXORAD_MIN_TEMP = 50.0,
     EXORAD_INTERP_METH = 1,
     EXORAD_TINT = -1.0,
     &


    C-- Chemistry in radiative transfer
    # EXORAD_MIX_METHOD       :: Opacity mixing method, if mixing is enabled (ALLOW_EXORAD_MIX), -1 (or anything else) is RORR, 0 is adding up, 1 is ML mixing, 2 is AEE
    # EXORAD_CHEM_METHOD      :: disequilibrium chemistry method to use (1=rainout, choose anything else for no diseq)
     &EXORAD_CHEM
     EXORAD_MIX_METHOD = -1
     EXORAD_CHEM_METHOD = -1
     &


Clearly, the most important parameter in this file is ``EXORAD_deltaT``, which sets the frequency of flux updates (the radiative timestep).
High values in ``EXORAD_deltaT`` may lead to incorrect radiative fluxes that would cause the model to heat/cool to unphysical temperatures, which could cause crashes.

.. note::

   If you encounter crashes in your model, you might want to lower the value of ``EXORAD_deltaT``.

.. note::

   ``EXORAD_INTERP_METH`` and ``EXORAD_TINT`` are new parameters. Handle with caution. Using ``EXORAD_INTERP_METH=1`` should increase the stability of the model.

.. warning::

   ``EXORAD_deltaT`` needs to be a multiple of the dynamical timestep!


You can turn on or off scattering with the ``EXORAD_SCATTERING`` flag. The convergence behaviour of the source function is controlled with ``EXORAD_MAX_I_ITER_SCAT`` and ``EXORAD_MAX_REL_DELTA_S``.
For more information on the convergence of the source function see Schneider et al. (2022).
``EXORAD_INTERP_STEP`` controls whether you interpolate fluxes horizontally or not.
For more information on the interpolation of fluxes see Schneider et al. (2022).

All parameters in the  ``EXORAD_CHEM`` namelist are only used if ``#define ALLOW_EXORAD_MIX`` has been set in ``code/EXORAD_OPTIONS.h``. 
If this wasn't set, it will as default use the premixed opacity tables.
You can decide with ``EXORAD_MIX_METHOD`` which method to use for the k-table mixing. 
See Schneider et al. (2023) for a discussion of the implemented methods.
