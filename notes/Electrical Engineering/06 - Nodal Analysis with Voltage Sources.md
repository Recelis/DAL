# Nodal Analysis with Voltage Sources

# Dependencies
This is a subset of `Nodal Analysis`, if voltage sources are involved.
There are two cases:

- When voltage source is connected between a reference node and a non-reference node. i.e. the voltage source is between node v1 and ground, so v1 is whatever the voltage source is.
- When a voltage source is connected betwen two non-reference nodes. i.e. 5V between v2 and v3. This forms a `supernode`. Solving this requires treating the entire supernode as a node and then applying `KCL` and `KVL` to solve for the voltages in the nodes.

# Implementations


# Dependents

# Interactions
