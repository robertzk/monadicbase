# R with a history monad

The R base library equipped with a history monad will allow
us to perform timeless computation. Specifically, instead of thinking
of an R script as a "program" that "runs" forwards in time, we will be able
to roll back to arbitrary points in time as if it was a single
mathematical structure of which we were computing "time
[sections](https://en.wikipedia.org/wiki/Section_(category_theory))."
These time sections are a *view* into the program (equipped with
a set of inputs) and the union over time sections gives us
full *history* of the program given a fixed set of inputs.
This is equivalent to being able to retro-actively place
a `browser` anywhere in the program, or run it backwards,
and fully generalizes [`dump.frames`](https://stat.ethz.ch/R-manual/R-devel/library/utils/html/debugger.html),
which merely offers a *single* time section.

The main advantage of this approach is that we implicitly are
forced to track the dependency of every computation. However,
as R operates on *hierarchical heterogeneous structures* (lists),
we can perform a static analysis to deterine minimally parallelisable
"substructure" computations.

In particular, if we are munging
a dataset and feeding it to a statistical classifier,
we should be capable of multi-threadedly parallelizing the
computation of each variable in the final munged dataframe
by performing a *destructuring* of the final program into
a sequence of *subprograms* which together compose the 
original program, but are *totally parallelized*. Applied 
recursively to a large computation, this allows for the
most general possible parallelization of subcomputations
without losing the flexibility of R syntax and pliability.

The goal here is to develop an extension or re-formulation of
R which is capable of performing just-in-time restructuring
of arbitrary R programs into an isomorphic collection of
R programs (collection, not sequence, since now by definition
the order is independent) which are provably not further parallelizable.
 
