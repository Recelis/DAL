# Kirchoff's Laws

# Dependencies

There are two laws. Kirchhoff's current law (KCL) and Kichhoff's voltage law (KVL).

KCL says that the algebraic sum of currents entering a node (or a closed boundary) is zero.

$$
\sum_{n=1}^{N} I_n = 0
$$

Where n is the nth current entering of leaving the node and N is the number of branches connected to the node. Currents entering the node are positive, while currents leaving are negative. A closed boundary is a generalised case of a node, but essentially it is a closed path. All current entering the closed boundary must equal all current leaving.

KVL says that the algebraic sum of all voltages around a closed path (or loop) is zero.

$$
\sum_{m=1}^{M} v_m = 0
$$

Where m is the mth voltage and M is the number of voltage in the loop.

# Implementations

# Dependents
`Circuit Analysis` when combined with `Ohm's Law`.
# Interactions
