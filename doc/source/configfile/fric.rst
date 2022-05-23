data.exofric
^^^^^^^^^^^^

The ``data.exofric`` file is used to set parameters for the dragtreatment used in ``expeRT/MITgcm``.

.. warning::

    Previous versions of exorad used to treat friction in the ``data.exo`` file.
    This has changed now!

A typical ``data.exofric`` file may look like this:

.. code::

    # FRICTION
    # Rayleigh friction prescription to U and V wind field

    # EXORAD_FRIC_TYPE:: Type of friction. <1: friction on U and V, >1: friction on geographic U only;
    #          0, ABS>3 = no friction
    #          +-1      = Carone2020, Schneider2022
    #          +-2      = global friction with timescale: EXORAD_FRIC_GLOBAL_TAU
    #          +-3      = ohmic drag like Rauscher2013
    # EXORAD_FRIC_HEAT  :: True if you want to dissipate the heat, false if you just want the drag
    # EXORAD_FRIC_GLOBAL_TAU:: Set a global dragtimescale in seconds (only used when EXORAD_FRIC_TYPE = +-2)
    # EXORAD_FRIC_OHM_B:: Magnetic field strength (in Tesla), whenever ohmic dissipation (EXORAD_FRIC_TYPE = +-3) is used
    # EXORAD_KF :: sets friction time scale, Unit 1/days-1 (only used when EXORAD_FRIC_TYPE = +-1)
    # EXORAD_SIGMA_B :: Sets vertical pressure [Pa] for lower boundary
    #                   Defined wrt lower bounary as specified in data (only used when EXORAD_FRIC_TYPE = +-1)
    # EXORAD_FRIC_DUMP :: Flag to output or not diagnostics of the rayleigh drag
    # EXPRAD_FRIC_MIN_P :: Minimum pressure for friction

    &EXORAD_FRIC
         EXORAD_KF = 1.0
         EXORAD_SIGMA_B = 0.7
         EXORAD_FRIC_HEAT = .TRUE.
         EXORAD_FRIC_TYPE = 1
         EXORAD_FRIC_GLOBAL_TAU = 0.0
         EXORAD_FRIC_OHM_B = 0.0005
         EXORAD_FRIC_DUMP = .TRUE.
         EXORAD_deltaT_FRIC = 100.0
         EXORAD_FRIC_MAX = 1.0E-4
         EXORAD_INTERP_STEP_FRIC = 2
         EXORAD_FRIC_MIN_P = 10.0
    /

    # Elemental data for ohmic dissipation
    # Arrays need to be filled up onto EXORAD_OHM_NEL. Change EXORAD_OHM_NEL in EXORAD_IONISATION_DATA.h if requiered.
    # EXORAD_OHM_ELABU  :: Elemental abundancies, sorted with proton number
    #                        log(eps_X)=log(NX/NH) + 12
    #                        see Asplund2009, Table 1, for more information about
    #                        the units.
    #                        Note: the elemental abundancies will be
    #                        automatically converted to NX/NH during readin.
    # EXORAD_OHM_POTENTIAL :: first ionisation potential of elements, sorted
    #                        with proton number, needs to be in eV, will be converted during readin
    &EXORAD_OHM_DATA
         EXORAD_OHM_ELABU = 12.00, 10.93, 1.05, 1.38, 2.70, 8.43, 7.83, 8.69,
                        4.56, 7.93, 6.24, 7.60, 6.45, 7.51, 5.41, 7.12,
                        5.50, 6.40, 5.03, 6.34, 3.15, 4.95, 3.93, 5.64,
                        5.43, 7.50, 4.99, 6.22
         EXORAD_OHM_POTENTIAL = 13.59844, 24.58741, 5.39172, 9.32263,
                        8.29803, 11.2603, 14.53414, 13.61806, 17.42282,
                        21.56454,  5.13908, 7.64624, 5.98577, 8.15169,
                        10.48669, 10.36001, 12.96764, 15.759, 4.34066,
                        6.11316, 6.56144, 6.8282, 6.7462, 6.7665,
                        7.43402, 7.9024, 7.8810, 7.6398

    /


The ``EXORAD_FRIC`` section is used to control the strength of the boundary layer friction.
Parameters are described above. There are four types of friction that can be applied to the model.

The ``EXORAD_OHM_DATA`` section is used to set abundancies and ionisation potentials for ohmic dissipation.

.. warning::

    Ohmic dissipation is tested. However, please mind that there are no results yet published using the ohmic dissipation treatment outlined here.
