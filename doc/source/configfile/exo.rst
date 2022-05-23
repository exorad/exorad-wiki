data.exo
^^^^^^^^

The ``data.exo`` file is used to set parameters for the physics used in ``expeRT/MITgcm``.

A typical ``data.exo`` file may look like this:

.. code::

    C--   EXORAD interface parameter (namelist EXORAD_PARAMS):
    # EXORAD_NEWTON :: flag to switch on/off simplfied radiative forcing
    # EXORAD_FULL :: flag to switch on/off full radiative transfer
    # EXORAD_LOW :: flag to enforce special treatment of lower layers
    # EXORAD_TYPE :: flag to choose type of planet, GASY or ROCK (Char*4)

     &EXORAD_PARAMS
     EXORAD_NEWTON =.FALSE.,
     EXORAD_FULL =.TRUE.,
     EXORAD_LOW =.FALSE.,
     EXORAD_TYPE ='GASY',
     &

    # SPONGE LAYER SETTING
    # Rayleigh friction prescription to U and V wind field
    # EXORAD_KTOP :: sets friction time scale (the same for both U and V)
    #                [1/days-1]
    # EXORAD_SIGMA_T :: Sets vertical pressure [Pa] for sponge layer
    # EXORAD_PREF :: Reference pressure [Pa] for setting sponge layer. 1.0E6 means 1e-4 bar as the starting of the spongelayer (with sigma_T = 1.0E-10)
    # EXORAD_DEITRICK_K_SPONGE :: Use the sponge layer magnitude description of Deitrick et al. (2019) instead of exorad description
    # EXORAD_SPONGE_ZONAL :: Soft (True) or hard spongelayer (False)

     &EXORAD_SPONGE
     EXORAD_KTOP = 20.0,
     EXORAD_SIGMA_T=1.0E-10,
     EXORAD_PREF=1.0E6,
     EXORAD_SPONGE_ZONAL=.TRUE.
     EXORAD_DEITRICK_K_SPONGE=.FALSE.
     &

The ``EXORAD_PARAMS`` section sets flags for the physics that should be used in ``expeRT/MITgcm``.
If you want to use ``expeRT/MITgcm`` with full radiative transfer, you will need to set ``EXORAD_FULL`` to ``.TRUE.`` and ``EXORAD_NEWTON`` to ``.FALSE.``.
``EXORAD_TYPE`` should always be ``GASY`` since ``ROCKY`` planets are not yet implemented in ``expeRT/MITgcm``.
``EXORAD_LOW`` can be used to enforce temperature convergence to a specific temperature profile (as specified in ``data.exodeep``).
This method is described in more detail in `Carone et al. (2020) <https://ui.adsabs.harvard.edu/abs/2020MNRAS.496.3582C/abstract>`_.

The ``EXORAD_SPONGE`` section is used to set the parameters of the sponge layer.
There are two types of sponge layers implemented in ``expeRT/MITgcm``: a soft and a hard sponge layer,
where the soft spongelayer forces the zonal velocity towards its zonal mean instead of to zero (hard sponge layer).
You can switch between those by using the ``EXORAD_SPONGE_ZONAL`` flag (``.TRUE.`` uses the soft sponge layer, ``.FALSE.`` the hard).
The magnitude of the sponge layer is set using ``EXORAD_KTOP``.

.. warning::

    If you encounter that your model crashs in the upper atmosphere due to weird behaviour in the upper atmosphere,
    you could consider using a stronger sponge layer (value for ``EXORAD_KTOP``) and see if that helps.
