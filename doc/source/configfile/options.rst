EXORAD_OPTIONS.h
^^^^^^^^^^^^^^^^

The ``EXORAD_OPTIONS.h`` file is located in the code base of the ``exorad`` module and should be overwritten by a custom ``EXORAD_OPTIONS.h`` file in the ``code`` directory of a simulation.
The ``EXORAD_OPTIONS.h`` file is used to set compile flags for ``exoradPRT/MITgcm``.

These compileflags are currently available:

.. list-table::
   :widths: auto
   :header-rows: 1

   * - Flag
     - Meaning
   * - ``ALLOW_EXORAD_FULL``
     - allow full radiative transfer in exorad
   * - ``ALLOW_EXORAD_CLOUDS``
     - allow clouds for radiative transfer (not yet documented!)
   * - ``ALLOW_EXORAD_NG``
     - allow for NG acceleration to converge the source function (test this if you have issues to converge the source fct)
   * - ``ALLOW_EXORAD_PRINT``
     - debugging mode with lots of information printed out to STDOUT


