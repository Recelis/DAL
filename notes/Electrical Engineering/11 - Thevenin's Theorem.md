# Thevenin's Theorem

# Dependencies
This is a technique for simplying a circuit analysis. Especially in regards to a circuit with a variable load and a fixed load, where when a variable load is changed, the subsequent circuit analysis does not need to be all recalculated because the fixed load parts of the circuit has been simplified into a Thevenin equivalent.

This equivalent may be a voltage and a resistance.

The full definition:

Thevenin's theorem states that a linear two-terminal circuit can be replaced by an equivalent circuit consisting of a voltage source V_Th in series with a resistor R_th, where V_Th is the open-circuit voltage at the terminals and R_Th is the input or equivalent resistance at the terminals when the independent sources are turned off.`

# Implementations
Case 1: Network has no dependent sources. Which means that we can turn off all independent sources. R_th is the network looking between terminals a and b.

Case 2: If network has dependent sources, we turn off all independent sources. We either set a voltage source v_o at terminals a and b and determine resulting current i_o. Then R_th = v_o / i_o. 
Or we set a current source i_o at terminals a-b and find the voltage v_o.

# Dependents

# Interactions
