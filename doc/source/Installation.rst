Installation
------------

Quick guide
^^^^^^^^^^^
There are a few steps that should be carried out prior to the installation of exorad.

1. Download and familiarise with `MITgcm <https://mitgcm.readthedocs.io/en/latest/>`_
2. Get permission to the exorad repository
3. Clone the exorad repository and move inside the repo.
4. Checkout the ``expeRT`` branch: ``git checkout exoradRT``
5. run ``install_exorad.sh`` to link exorad into MITgcm
6. Install `anaconda <https://www.anaconda.com/>`_
7. create a virtual environment for exorad ``conda create -n exorad``
8. Install ``exorad_opac``
9. Download and set the opacity sources for `petitRADTRANS <https://petitradtrans.readthedocs.io/en/latest/>`_ into this environment


Install exorad
^^^^^^^^^^^^^^

MITgcm
""""""

To install exorad you will first need to install `MITgcm <https://mitgcm.readthedocs.io/en/latest/>`_.
Follow the instructions in `MITgcm <https://mitgcm.readthedocs.io/en/latest/>`_ to download the code and to understand how ``MITgcm`` works.

.. note::

    This guide does not cover how to install or use ``MITgcm``. Please read the ``MITgcm`` `documentation <https://mitgcm.readthedocs.io/en/latest/>`_ carefully.

Once you downloaded ``MITgcm`` it is important to set the ``MITGCM_ROOTDIR`` environmental variable:

.. code::

    export MITGCM_ROOTDIR=<path to MITgcm>

You can do so by appending the above line to your ``.zshrc`` / ``.bashrc`` / ``.bash_profile`` file.



``MITgcm`` is still developed. To go to a version that is known to work with ``expeRT`` do the following (inside the ``MITgcm`` directory):

.. code::

   git checkout 8b41e6235cdfefca3b75a9c4c8cc300edbe9af64

exorad
""""""
.. note::

    This guide only covers ``expeRT/MITgcm`` (Schneider et al. 2022)

.. note::

    Please contact `Aaron Schneider <mailto:aaron.schneider@nbi.ku.dk>`_ to get access to ``expeRT/MITgcm``.

The first installation step is to clone ``expeRT/MITgcm`` (into any directory of your choice):

.. code::

    git clone https://github.com/exorad/exorad.git

.. note::

    Most users prefer to clone private `repositories via ssh. <https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account>`_

    You can then do

    .. code::

       git clone git@github.com:exorad/exorad.git


Next checkout the ``exoradRT`` branch:

.. code::

    git checkout exoradRT

Link exorad to the MITgcm directory using the supplied ``install_exorad.sh`` script.

.. code::

    cd <directory of exorad repo>
    sh install_exorad.sh

The script comes with an option to prettify the exorad code (using `findent <https://sourceforge.net/projects/findent/>`_).
More information on how to use the script can be found using the ``-h`` argument.


Install python packages (6-9)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
``expeRT/MITgcm`` uses `petitRADTRANS <https://petitradtrans.readthedocs.io/en/latest/>`_ to create the opacity input files.

.. note::

    This guide bases on `anaconda <https://www.anaconda.com/>`_. Other python installations are possible as well.

Install `anaconda <https://www.anaconda.com/>`_. Once installed, create a virtual environment for ``expeRT/MITgcm`` and ``petitRADTRANS``:

.. code::

    conda env create -n exorad python=3 pip numpy

Activate the environment using

.. code::

    conda activate exorad

Install ``petitRADTRANS`` and the preprocessing package of ``expeRT/MITgcm`` using

.. code::

    cd <directory of exorad repo>
    pip install -e exorad_opac

The installation of ``exorad_opac`` will automatically trigger the installation of ``petitRADTRANS``.
The last step is then to install the opacity sources for ``petitRADTRANS``.

.. important:: instructions for the installation of the opacity sources can be found `here <https://petitradtrans.readthedocs.io/en/latest/content/installation.html>`_.
