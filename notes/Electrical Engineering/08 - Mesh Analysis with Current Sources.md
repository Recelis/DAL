# Mesh Analysis with Current Sources

# Dependencies
There are two special cases:

- Case 1: Mesh exists in only one mesh. We set that current for the mesh element as the current that it is given. i.e. $i_1 = 5A$;

Case 2: When current sources exists between two meshes. We create supermesh by excluding current source and any elements connected in series.

Then we apply KV; to the node where the two meshes intersect.

# Implementations


# Dependents

# Interactions
