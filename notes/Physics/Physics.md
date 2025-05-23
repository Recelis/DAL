# Dimensional Analysis

[docs](David Morin Introduction to classical mechanics)
Dimensional analysis is a way of describing a physical phenomena only by the units of measurement and some common rules that you know to extract the formula for the thing you want to describe.

i.e.

A spring with constant k has a mass m. The spring force is F(x) = -kx where x is the displacement from equilibrium. What can you say about the frequency of oscillations?

# Velocity

Velocity is speed with a direction. Where speed is a distance covered over a certain amount of time. Integrating velocity by time between two points in time will lead to the total distance traveled. Taking the derivative will lead to the acceleration.

# Acceleration

Acceleration is the change of velocity over time. Acceleration itself has a direction.

# Terminal Velocity

[docs](https://en.wikipedia.org/wiki/Terminal_velocity)
Terminal velocity is the maxiumum velocity that an object can fall through a fluid. This limit is caused when the `drag force (F_d)` + `buoyancy` is equal to the `force of gravity`.

Vt = sqrt(2mg/pAC_d)

(Without buoyancy effects)

Vt is terminal velocity
m is the mass of the falling object
g is the acceleration due to gravity
C_d is the `drag coefficient`
p is the `density` of the fluid which the object is falling
A is the `projected area` of the object

An object will reach its terminal speed `asymptotically`.

## Derivation

`Ricatti equation` solvable by reducing to a `second-order linear differential equation`.
<i>Skipping a lot of derivation because I don't follow the integrations</i>

The resulting speed as a function of time that reaches terminal velocity becomes:

v = V_t tanh(t g/V_t)

`tanh` is an example of a `hyperbolic function`. When the `tanh` gets to 1, then the velocity reaches terminal velocity.
