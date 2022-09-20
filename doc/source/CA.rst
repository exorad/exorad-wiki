Convergence acceleration
------------------------

.. attention::

   This is only relevant to you, if you want to do force the potential temperature to a specific potetential temperature in the GCM.


The functionality of convergence acceleration (CA) relates to a procedure that forces the potential temperature to a specific potetential temperature in the GCM.

The details of this procedure can be found in the Appendix of Schneider et al. (2022b).

.. note::

    Convergence acceleration is activated/deactivated by setting the following two parameters:

    ``input/data.exo``: ``EXORAD_DEEP_CONVERGENCE =.TRUE.``

    ``code/EXORAD_OPTIONS.h``: ``#define ALLOW_EXORAD_CA`` (pre compilation)


data.exoca
^^^^^^^^^^

The data.exoca file controls the parameters used for the forcing towards the goal theta.

Here is an example (used in Schneider et al. 2022b):

.. code::

  # EXORAD_CA_APPL :: Timespan over which the forcing is applied
  # EXORAD_CA_PDAMP :: pressure, above which we want the full artificial forcing
  # EXORAD_CA_PMIN :: minimum pressure, where you want to start the artificial forcing
  # EXORAD_CA_GOAL_THETA :: The final potential temperature to which you want to force

  &EXORAD_CA_NML
    EXORAD_CA_APPL = 864000.0
    EXORAD_CA_PDAMP = 10e5
    EXORAD_CA_PMIN = 1e5
    EXORAD_CA_GOAL_THETA = 4658.899408445296
  /


.. note::

   Please note that the potetential temperature for ``MITgcm`` is calculated with reference to the lower interface of the deepest pressure layer.

CA in detail
^^^^^^^^^^^^

There are two different smoothing schemes in the CA that are of importance:

1. time based smoothing
2. vertical smoothing

The time based smoothing is a way to make sure that the forcing from the CA is applied in a way that is stable enough to make sure that the GCM does not crash.
It works such that you set a timespan (``EXORAD_CA_APPL``) over which you want to go to the desired final theta (``EXORAD_CA_GOAL_THETA``)
This time should be typially in the order of a few days.
The CA will then force towards the goal theta following a smooth function (first half of a sinuscurve), where the forcing towards the goal theta is the strongest at the middle of the timespan.

The vertical smoothing makes sure that the forcing towards the final theta has a smooth transition to the upper atmosphere.
It is done in such a way that it applies the the CA only to the layers below ``EXORAD_CA_PMIN``.
It then has a smooth transition between ``EXORAD_CA_PMIN`` to ``EXORAD_CA_PDAMP``, from which it does the full forcing towards the goal potential temperature.

You can easily extract the smoothing functions in the code (``pkg/exorad/exorad_ca.F``).

The final potential temperature (for layers below ``EXORAD_CA_PMIN``) can be written as:

.. math::

   \Theta = \Theta_0 + v\cdot(\Theta_\mathrm{goal}-\Theta_0),

where :math:`\Theta` is the potential temperature after the CA, :math:`\Theta_0` is the potential temperature before CA, :math:`\Theta_\mathrm{goal}` is the potential temperature to which we want to force (``EXORAD_CA_GOAL_THETA``) and :math:`v` is the vertical smoothing factor.
The vertical smoothing factor goes from 0 to 1 between ``EXORAD_CA_PMIN`` and ``EXORAD_CA_PDAMP``.
