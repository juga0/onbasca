.. _pidcontrol:

Engineering background
========================

PID controller
---------------

PID controller continuously calculates an error value :math:`e(t)` 
as the difference between a desired setpoint (SP) and 
a measured process variable (PV) and applies a correction based on 
proportional, integral, and derivative terms 
(denoted P, I, and D respectively), hence the name. 

Standard form
~~~~~~~~~~~~~~

.. math::

    U(t) = MV(t) =
      K_{p}{e(t)}+
      K_{i}\int _{0}^{t}{e(\tau )}{d\tau }+
      K_{d}{\frac {d}{dt}}e(t)

where:

:math:`K_{p}` the proportional gain, a tuning parameter,

:math:`K_{i}` is the integral gain, a tuning parameter,

:math:`K_{d}` is the derivative gain, a tuning parameter,

:math:`e(t)=SP-PV(t)` is the error (SP is the setpoint, and PV(t) is the 
process variable),

:math:`t` is the time or instantaneous time (the present),
τ is the variable of integration (takes on values from time 0 to the present 
:math:`t`.

Ideal form
~~~~~~~~~~~~~~

:math:`K_{p}` gain is applied to the :math:`I_{out}`, and :math:`D_{out}` 
terms, yielding:

.. math::

    MV(t) = 
      K_{p}\left(
        e(t)+
        \frac{1}{T_{i}}\int _{0}^{t}{e(\tau )}\,{d\tau }+
        T_{d}{\frac {d}{dt}}e(t)
      \right)

where:

:math:`T_{i}` is the integral time and :math:`T_{d}` is the derivative time

the gain parameters are related to the parameters of the standard form through 
:math:`K_{i}={\frac {K_{p}}{T_{i}}}` and :math:`K_{d}=K_{p}T_{d}`

Algorithm
~~~~~~~~~~

.. code-block:: none

    previous_error = 0
    integral = 0
    loop:
      error = setpoint - measured_value
      integral = integral + error * dt
      derivative = (error - previous_error) / dt
      output = Kp * error + Ki * integral + Kd * derivative
      previous_error = error
      wait(dt)
      goto loop


.. image:: images/File_PID_en.svg

[PIDcontr]_
