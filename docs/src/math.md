# Introduction
Let us think of our types geometrically. In other words, elements of a type form a _manifold_.
This document will explain this point of view in some detail.

## Some terminology/conventions.

Let ``p`` be an element of type M, which is defined by some assignment of numbers ``x_1,...,x_m``,
say ``(x_1,...,x_m) = (a_1,...,1_m)`` 

A _function_ ``f:M -> K`` on ``M`` is (for simplicity) a polynomial ``K[x_1, ... x_m]``

The tangent space ``T_pM`` of ``T`` is the ``K``-vector space spanned by derivations ``d/dx``. 
The tangent space acts linearly on the space of functions. They act as usual on functions. Our starting point is 
that we know how to write down ``d/dx(f) = df/dx``.

The collection of tangent spaces ``{T_pM}`` for ``p\in M`` is called the _tangent bundle_ of ``M``.

Let ``df`` denote the first order information of ``f`` at each point. This is called the differential of ``f``. 
If the derivatives of ``f`` and ``g`` agree at ``p``, we say that ``df`` and ``dg`` represent the same cotangent at ``p``.
The covectors ``dx_1, ..., dx_m`` form the basis of the cotangent space T^*_pM at ``p``. Notice that this vector space is 
dual to ``T_p``

The collection of cotangent spaces ``{T^*_pM}`` for ``p\in M`` is called the _cotangent bundle_ of ``M``.

### Push-forwards and pullbacks

Let ``N`` be another type, defined by numbers ``y_1,...,y_n``, and let ``g:M -> N`` be a _map_, that is, 
an ``n``-dimensional vector ``(g_1, ..., g_m)`` of functions on ``M``.

We define the _push-forward_ ``g_*:TM -> TN`` between tangent bundles by ``g_*(X)(h) = X(g\circ h)`` for any tangent vector ``X`` and function ``f``.
We have ``g_*(d/dx_i)(y_j) = dg_j/dx_i, so the push-forward is equal to the Jacobian when written in coordinates.

Similarly, the pullback of the differential ``df`` is defined by
``g^*(df) = d(g\circ f)``. So for a coordinate differential ``dy_j``, we have
``g^*(dy_j) = d(g_j)``. Notice that this is a covector, and we could have defined the pullback by its action on vectors by
``g^*(dh)(X) = g_*(X)(dh) = X(g\circ h)`` for any function ``f`` on ``N`` and ``X\in TM``. In particular, 
``g^*(dy_j)(d/dx_i) = d(g_j)/dx_i``. If you work out the action in a basis of the cotangent space, you see that it acts
by the adjoint of the Jacobian.

Notice that the pullback of a differential and the pushforward of a vector have a very different meaning, and this should
be reflected on how they are used in code.

The information contained in the push-forward map is exactly _what does my function do to tangent vectors_.
Pullbacks, acting on differentials of functions, act by taking the first order information of a function.
This works in a coordinate invariant way, and works without the notion of a metric.
_Gradients_ recall are vectors, yet they should contain the same information of the differential ``df``.
Assuming we use the standard euclidean metric, we can identify ``df`` and ``\nabla f`` as vectors.
But pulling back gradients still should not be a thing.

If the goal is to evaluate the gradient of a function ``f=g\circ h:M -> N -> K``, where ``g`` is a map and ``h`` is a function,
we have two obvious options:
First, we may push-forward a basis of ``M`` to ``TK`` which we identify with K itself. 
This results in ``m`` scalars, representing components of the gradient.
Step-by-step in coordinates:
1. Compute the push-forward of the basis of ``T_pM``, i.e. just the columns of the Jacobian ``dg_i/dx_j``.
2. Compute the push-forward of the function ``h`` (consider it as a map, K is also a manifold!) to get ``h_*(g_*T_pM) = \sum_j dh/dy_i (dg_i/dx_j)

Second, we pull back the differential ``dh``: 
1. compute ``dh = dh/dy_1,...,dh/dy_n`` in coordinates.
2. pull back by (in coordinates) multiplying with the adjoint of the Jacobian, resulting in ``g_*(dh) = \sum_i(dg_i/dx_j)(dh/dy_i)``.
