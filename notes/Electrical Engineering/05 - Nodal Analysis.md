# Nodal Analysis

# Dependencies
This allows you to calculate the currents and voltages in a circuit. Relies on `KCL` to do this. There are three steps to this.

1. Select a node as a reference point and assign voltages references to other nodes.
2. Apply KCL to each node. This will result in an equation per node. For the current direction, a general rule is that current flows from higher potential to lower potential.
3. Solve the simultanous equations to calculate the node voltages.

# Implementations
Typically used in circuit analysis software.

# Dependents
`Nodal Analysis with Voltage Sources`

# Interactions
