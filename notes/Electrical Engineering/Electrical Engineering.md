[docs](Fundamentals of Electrical Circuits)

# Electricity

## Voltage

∂
Measured in Volts (V). This can be thought of the pressure between charges between two points. If the thing separating it is very permittive or if the area is big, then the voltage is small because it doesn't take much work to move the charge up to the other point.

$$
v_{AB} \triangleq \frac{\delta w}{\delta q}
$$

where:

- \(\delta w\) is the incremental work required to move a small charge,
- # \(\delta q\) is that small amount of charge.
  Measured in Volts (V). This is a the charge difference between two points.∂

## Current

Measured in Amperes (A) and denoted as I. The rate at which charge is flowing.

## Capacitance

Capacitance is the ratio between the Voltage difference between two plates and the charges difference between those same two plates.
`q = Cv`
Where C is the capacitance of the capacitor and measured in Farads (F). One Farad is equal to 1 coulomb / volt.

Capacitance of a capacitor is calculated from its physical properties.
`C = ∈A/d`
Where ∈ is the permittivity of the dielectric material. A is the surface area of the plates and d the distance between the plates. This assumes the capacitor is made up of parallel plates.

Basically, a higher capacitance means that the for a given charge difference, the voltage (pressure) is the same, but a lower capacitance might would result in a lower voltage.

## Power

Power is time rate of expending or absorbing energy in Watts.

$$
p \triangleq \frac{\delta w}{\delta t}
$$

This can be simplified as:

$$
p = vi
$$

If power is positive, then it is power delivered to the element, but if it is negative, then it is supplied by element. The `law of conservation of energy` must be obeyed in any electrical circuit so everything supplied to the circuit must be absorbed.

$$
P = \frac{\delta E}{\delta q} * \frac{\delta q}{\delta t} = \frac{\delta E}{\delta t}
$$

Energy is often measured in terms of `Watt hours` which is the energy consumed in terms of hours.s

## Circuit Elements

Two types of elements: `active` or `passive`.
Active elements are capable of generating energy, examples are: generators, batteries, op amps. While passive element examples are resistors, capacitors, and inductors.

For the active elements, we have our voltage and current sources.

We have the concepts for `dependent` and `independent` power sources as well for `voltage` and `current`. So a voltage dependent voltage source means that the voltage out of the element depends on the voltage given to the element. A voltage dependent current source is sort like a transistor. We have current controlled sources as well. The symbol used is a diamond for dependent sources while the symbol used for independent sources is a circle (or a battery symbol for voltage).

## Resistance

Measured in Ohms (Ω). This is the material's tendency to resist the current.

$$
R = p l /A
$$

Where p is the resistivity of the material in ohm meters and l is the length and A is the cross-sectional area.

## Ohm's Law

$$
V = IR
$$

This is the relationship between voltage, current and resistance where the voltage determines the
product of the flow and the resistance. Typically, an engineer will have control over the voltage and the resistance of the material, while the current is the result of these two properties.

This means that voltage is proportional to current.

Not all resistors obey Ohm's Law. The ones that do are `linear resistors` while the ones that don't are `non-linear` where the resistance varies with current. Two examples are the lightbulb and the diode, but for practical purposes we assume they are linear.

## Conductance

The inverse of resistance. Measured in mho or ℧.

$$
G = 1 / R = i /v
$$
