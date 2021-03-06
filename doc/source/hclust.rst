Hierarchical Clustering
========================

`Hierarchical clustering <https://en.wikipedia.org/wiki/Hierarchical_clustering>`_ algorithms build a dendrogram of nested clusters by repeatedly merging or splitting clusters.

**Functions**

.. function:: hclust(D; linkage=:single)

	Perform hierarchical clustering on distance matrix `D` with specified cluster `linkage` function.

	:param D: The pairwise distance matrix. ``D[i,j]`` is the distance between points ``i`` and ``j``.
	:param linkage: A `Symbol` specifying how the distance between clusters (aka *cluster linkage*) is measured. It determines what clusters are merged on each iteration. Valid choices are:
	- ``:single``: use the minimum distance between any of the members
	- ``:average``: use the mean distance between any of the cluster's members
	- ``:complete``: use the maximum distance between any of the members.
	- ``:ward``: the distance is the increase of the average squared distance of a point to its cluster centroid after merging the two clusters.
	- ``:ward_presquared``: same as ``:ward``, but assumes that the distances in ``D`` are already squared.

The function returns an object of type `Hclust` with the fields
	 - ``merges`` the sequence of subtree merges.  Leafs are indicated by negative numbers, the ids of non-trivial subtrees refer to the rows in the ``merges`` matrix and the elements of the ``heights`` vector.
	 - ``heights`` subtrees heights, i.e. the distances between left and right top branches of each subtree.
	 - ``order`` indices of points ordered such that there are no intersecting branches on the *dendrogram* plot.  This ordering brings points of the same cluster close together.
	 - ``linkage`` the cluster `linkage` used.

Example:

	.. code-block:: julia

		D = rand(1000, 1000)
		D += D'  # symmetric distance matrix (optional)
		result = hclust(D, linkage=:single)

.. function:: cutree(result; [k=nothing], [h=nothing])

	Cuts the dendrogram to produce clusters at the specified level of granularity.

	:param result: Object of type ``Hclust`` holding results of a call to ``hclust()``.
	:param k: Integer specifying the number of desired clusters.
	:param h: Real specifying the height at which to cut the tree.

If both `k` and `h` are specified, it's guaranteed that the number of clusters is ``≥ k`` and their height ``≤ h``.

The output is a vector specifying the cluster index for each datapoint.
