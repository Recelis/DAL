# Mesh Analysis

# Dependencies
Mesh analysis is a way to find unknown currents in a circuit. It is only applicable to circuits that are `planar`. A `planar` circuit is one where it can be drawn in a `plane` without any `branches` `crossing` over each other. A non-planar circuit can be handled by nodal analysis.

A `mesh` is a loop which does not contain any other loops within it.

The steps to solve mesh currents are:

- assign mesh currents i_1, i_2...i_n to n meshes.
- apply KVL to each of the n meshes. Use `Ohm's Law` to express voltages in terms of mesh currents.
- solve the simulataneous equations.

# Implementations


# Dependents

# Interactions
