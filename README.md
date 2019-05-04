# polyhedrec
A python library to reconstruct 3d convex polyhedra from their face normals and areas.
To use the library, import it into python (works with both python 2 and python 3). For instructions on how to use it, use the python `help` function on the `reconstruct` function from the library:
```python
import polyhedrec

help(polyhedrec.reconstruct)
```

For reference, the output of the help function is the following:
```
Reconstruct (up to translation) a polyhedron from its outward unit normals
and face areas.

The unit normals must span 3D space, the areas must be positive and the
closure constraint ``sum(areas[i]*unormals[i] for i in range(n)) == 0``
must be satisfied, otherwise a ValueError exception is raised.

Inputs
------
unormals : list
    The list of unit normals (each normals should be a numpy array or a
    list/tuple).
areas : list
    The list of face areas.
D : float, optional
    Parameter affecting the initial guess for the root finding algorithm:
    each face is at distance D from a point inside the polyhedron. If D is
    not specified the function computes the appropriate distance; an
    explicit value should only be used if the algorithm with the built-in
    distance fails.
    The value entered for D has to be positive.
options : dict, optional
    A dictionary of additional options. The default is:
        {'rtol': 1e-05,'atol': 1e-08,'ftol': 1.5e-08,'xtol': 1.5e-08}
    'rtol' : float
        Relative tolerance parameter used when comparing numbers with
        numpy.isclose()
    'atol' : float
        Absolute tolerance parameter used when comparing numbers with
        numpy.isclose(). Used when comparing something with 0.
    'ftol': 1.5e-08
        Relative error desired when comparing the function to 0 in the
        root finding algorithm.
    'ftol': 1.5e-08
        Relative error desired in theapproximate solution in the root
        finding algorithm.

Returns
-------
P : Polyhedron
    Reconstructed polyhedron as an object of the Polyhedron class.

Raises
------
ReconstructError
    If the root algorithm is unable to reconstruct the polyhedron.
    In this case, specifying an explicit value for D may solve the problem. 

Notes
-----
The unit normals should be distinct; if they are not, duplicate vectors
are going to be merged into one by summing the associated areas. As a
consequence, the returned polyhedron may have less faces than expected.

Examples
--------
>>> import polyhedrec as pr

>>> unormals = [numpy.array([-0.17447189, -0.43229383, -0.88469294]),
                numpy.array([ 0.60815785,  0.73855608,  0.29099648]),
                numpy.array([-0.53342276, -0.44253185, -0.72085069]),
                numpy.array([-0.29876108,  0.45137263,  0.84083563]),
                numpy.array([-0.41552563, -0.69427644,  0.58763822])]

>>> areas = [7.049773119515445,
             8.521720027005253,
             1.9790800361257508,
             2.5983018666756377,
             5.1034298342172928]

>>> P = pr.reconstruct(unormals,areas)

>>> P.vertices
(array([-0.,  0., -0.]),
 array([ 6.02548243, -5.56480979,  1.53087654]),
 array([ 0.60717762, -2.63092577,  1.16582546]),
 array([-0.44385591,  0.31140014,  0.13727997]),
 array([ 1.70842807, -2.31842303,  2.31374446]),
 array([-0.2875289 , -1.90977608,  1.3851845 ]))

>>>P.f_adjacency_matrix 
array([[0, 1, 1, 0, 1],
       [1, 0, 1, 1, 1],
       [1, 1, 0, 1, 1],
       [0, 1, 1, 0, 1],
       [1, 1, 1, 1, 0]])

>>> P.v_adjacency_matrix
array([[0, 1, 1, 1, 0, 0],
       [1, 0, 1, 0, 1, 0],
       [1, 1, 0, 0, 0, 1],
       [1, 0, 0, 0, 1, 1],
       [0, 1, 0, 1, 0, 1],
       [0, 0, 1, 1, 1, 0]])

>>> P.inequalities
(array([ 0.        ,  0.17447189,  0.43229383,  0.88469294]),
 array([ 0.        , -0.60815785, -0.73855608, -0.29099648]),
 array([ 0.        ,  0.53342276,  0.44253185,  0.72085069]),
 array([ 0.38859427,  0.29876108, -0.45137263, -0.84083563]),
 array([ 2.25937552,  0.41552563,  0.69427644, -0.58763822]))

>>> P.print_inequality(0)
-0.174471886106*x + -0.432293832351*y + -0.884692943043*z <= 0.0

>>> P.faces[0].vertices
(0, 1, 2)

>>> P.faces[0].area
(7.049773119515445)

>>> P.faces[0].unormal
array([-0.17447189, -0.43229383, -0.88469294])
```
