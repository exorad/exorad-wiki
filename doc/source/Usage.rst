Usage
-----

For seting up ``expeRT/MITgcm`` on a cluster see also the `cluster setup <https://github.com/exorad/clustersetup>`_ docs.

Quick guide
^^^^^^^^^^^
The easiest way to setup a new ``expeRT/MITgcm`` simulation is:

1. create a new folder for the simulation
2. copy the ``code`` and ``input`` folder from the HD2 experiment in the ``verification`` folder
3. change ``opac.yaml`` in the input folder
4. create opacitys using the command ``exorad_opac_create`` in the simulation folder (more below)
5. check the temperature profile using ``exorad_plot_ic``
6. change files in code directory (e.g., ``SIZE.h`` and ``EXORAD_OPTIONS.h``)
7. compile ``MITgcm``
8. continue with changes to the input files (e.g., sponge layer settings, radiative timesteps, duration of simulation, ...)
9. Have fun

.. note::

  An automated approach to get the parameters that need to be changed can be found here:

  .. toctree::

    notebooks/params


We will now continue in this guide to give detailed info about the individual steps

Preperation
^^^^^^^^^^^
.. note::

    Please familiarise with `MITgcm <https://mitgcm.readthedocs.io/en/latest/>`_ beforehand

A ``MITgcm`` run folder (see verification folders as an example) usually comes with 4 mandatory folders: ``code``, ``input``, ``run``, ``build``.
The ``code`` folder includes all the mods that are nescessary for MITgcm to include ``exorad``.
The ``input`` folder is the home of all config and input files.
The ``run`` and the ``build`` folder are used for output files and binary files respectively.

You can use the ``exorad_build`` folder as an example to setup ``expeRT/MITgcm``.

.. code::

    exorad-example-run-folder
    ├── code   # contains the mods nescessary to load exorad
    │   ├── CPP_OPTIONS.h
    │   ├── DIAGNOSTICS_SIZE.h
    │   ├── EXORAD_OPAC.h     # automatically set by the opacity script
    │   ├── EXORAD_OPTIONS.h  # change to enable/disable flags for exorad
    │   ├── MDSIO_BUFF_3D.h
    │   ├── PARAMS.h
    │   ├── SIZE.h            # change for dimensions of the grid and multiprocessing
    │   ├── apply_forcing.F
    │   ├── do_atmospheric_phys.F
    │   ├── do_the_model_io.F
    │   ├── ini_theta.F
    │   ├── mon_init.F
    │   ├── packages.conf
    │   ├── packages_boot.F
    │   ├── packages_check.F
    │   ├── packages_error_msg.F
    │   ├── packages_init_fixed.F
    │   ├── packages_init_variables.F
    │   ├── packages_print_msg.F
    │   ├── packages_readparms.F
    │   ├── packages_unused_msg.F
    │   └── packages_write_pickup.F
    ├── input  # configuration and input files
    │   ├── data            # standard configuration file
    │   ├── data.exo        # global parameters for exorad, including spongelayer
    │   ├── data.exoprt     # parameters for radiative transfer in exorad
    │   ├── data.exorad     # only used for newtonian cooling
    │   ├── data.exofric    # parameters for friction
    │   ├── ml_coeff_0.data # weights for the DeepSet mixing
    │   ├── ml_coeff_0.meta # 
    │   ├── ml_coeff_1.data # 
    │   ├── ml_coeff_1.meta # 
    │   ├── opac.yaml       # parameters for init of opacity and temperature
    │   └── ...
    ├── run     # contains all the files that are created during run
    └── build   # used to build the MITgcm binary
.. Created with tree.nathanfriend.io


Preprocessing
^^^^^^^^^^^^^

.. warning::

    Before proceeding, please familiarise with the structure of the :ref:`opac.yaml` file and change it accordingly.

Once successfully installed and once all parameters in ``opac.yaml`` are set, you can invoke the preprocessing script using:

.. code::

   exorad_opac_create

.. code::

    usage: exorad_opac_create [-h] [-R RESOLUTION] [-nu]

    options:
      -h, --help            show this help message and exit
      -R RESOLUTION, --resolution RESOLUTION
                            specify the resolution of your wavelength grid
      -nu, --noupdate       specify if you want to update the data file (1=True,
                            0=False)

The current standard resolution is ``S0`` (see Schneider et al. 2022 for more details).
Possible resolutions are: ``S0``, ``S1``, ``S2`` as well as any float.
If you choose a float value, the resolution (lambda/dlambda) will be the that float.

.. note:: The script should always be invoked in the directory of your simulation (e.g., parent directory of the code, input, etc. folders).
.. warning:: The preprocessing script changes the input ``data`` file!
.. warning:: Perform this step BEFORE you compile ``MITgcm``. This script will change/create ``EXORAD_OPAC.h``.

You may want to plot the initial temperature profile.
There is a function for that:

.. code::

   exorad_plot_ic

Please note, that you can still invoke the ``exorad_opac_create`` script after compilation.
In that case, please make sure to not change any parameters that would change ``EXORAD_OPAC.h``.
This is currently: The resolution of the vertical grid (``press_init`` section in ``opac.yaml``), of the temperature grid (``grid`` section in ``opac.yaml``) and of the wavelength grid (-R flag).

If you want to use exorad concurrently with two different wavelength resolutions, you could preprocess and compile exorad twice.
This would give you two binaries of MITgcm which you could then both use (e.g., in sequence).

.. code::

   exorad_opac_create -R S0
   ... compile and rename MITgcm binary ...
   exorad_opac_create -R S1
   ... compile and rename MITgcm binary ...


If you want to use ``exorad_opac`` in a way that is beyond the possibilities of the ``exorad_opac_create`` command, you might want to have a look at this Tutorial:
  
  .. toctree::

    notebooks/exorad_opac_custom

Compilation
^^^^^^^^^^^
.. warning:: Do the above steps, before you compile

Compilation of ``expeRT/MITgcm`` is not different than the standard ``MITgcm`` compilation.
The reader is referred to the ``MITgcm`` docs.


Opacity mixing
^^^^^^^^^^^^^^
If you want to use the opacity mixing feature of expeRT/MITgcm (see Schneider et al. 2023, in review), you may once create weights for the opacity mixing. 
Please read the paper and the instructions in the `opac mixer docs <https://opacmixer.readthedocs.io/en/latest/>`_. 
Once you obtained the weights and exported them (using the ``export`` function of the ``emulator``), you can copy them to the input folder from which they will then be available to the code.

This step only needs to be done once and the weights should in principle work with any chemical composition or wavelength resolution (read the paper to understand what it does and for a discussion).

The opacity mixing is disabled by default and needs to be enabled in ``code/EXORAD_OPTIONS.h`` by setting ``#define ALLOW_EXORAD_MIX``.
The preprocessing, however, is the same. The preprocessing (``exorad_opac_create``) creates the individual opacity files for mixing alongside an eq.chem. abundancy grid and a premixed grid (used when ``#undef ALLOW_EXORAD_MIX``).

The type of mixing is set in the ``data.exoprt`` file with the variable ``EXORAD_MIX_METHOD``.


Running
^^^^^^^
Running expeRT/MITgcm is like running MITgcm.
The reader is referred to the ``MITgcm`` docs.