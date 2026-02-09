# Interdisciplinary Numerical Methods: "Hub" 18.C21/16.C21

This new MIT course (**Spring 2026**) introduces numerical methods and numerical analysis to a broad audience (assuming 18.03, 18.06, or equivalents, and some programming experience).  It is divided into two 6-unit halves:

*  **18.C21/16.C21** (first half-term “hub”): basic numerical methods, including curve fitting, root finding, numerical differentiation and integration, numerical differential equations, and floating-point arithmetic. Emphasizes the complementary concerns of accuracy and computational cost.  [Prof. Steven G. Johnson](http://math.mit.edu/~stevenj) and [Prof. Youssef Marzouk](https://aeroastro.mit.edu/people/youssef-m-marzouk/).

*   Second half-term: two options for 6-unit “spokes”

    *   [18.C21A/16.C21A — numerical methods for partial differential equations](https://github.com/Shania99/numerical_spoke_pde): finite-difference, finite-volume, and finite-element methods; boundary conditions, accuracy, and stability. [Prof. Youssef Marzouk](https://aeroastro.mit.edu/people/youssef-m-marzouk/).

    *   [18.C21B/16.C21B — large-scale linear algebra](https://github.com/mitmath/numerical_spoke_linalg): sparse matrices, iterative methods, randomized methods. [Prof. Steven G. Johnson](http://math.mit.edu/~stevenj).

Taking both the hub and any spoke will count as an 18.3xx class for math majors, similar to 18.330, and as 16.90 for AeroAstro majors. Weekly homework, *exam* TBD, and spokes will include a final project with an in-class presentation.

This repository is for the "hub" course 18.C21/16.C21.

## 18.C21/16.C21 Syllabus, Spring 2026

**Instructors**: [Prof. Steven G. Johnson](http://math.mit.edu/~stevenj) and [Prof. Youssef Marzouk](https://aeroastro.mit.edu/people/youssef-m-marzouk/).  TAs: [Andrey Bryutkin](https://math.mit.edu/directory/profile.html?pid=2582) and [Rodrigo Arrieta Candia](https://math.mit.edu/directory/profile.html?pid=2409).

**Lectures**: MWF10 in 2-142 (Feb 2 - Mar 20), slides and notes posted below.

**Homework and grading**: 5 weekly psets, due Wednesdays at midnight (Feb. 11, 18, 25; Mar. 11, 18).  Students will have oral check-ins on 3 psets (randomly selected) where they have to explain their work (pass/fail per problem). One in-class exam, March 6. For accommodations, speak with [S3](https://studentlife.mit.edu/s3) and have them contact the instructors.  Grade percentages: 40% psets, 30% check-ins, 30% exam.

* Homework assignments will require some programming — you can use either **Julia or Python** (your choice; instruction and examples will use a mix of languages).

* Submit your homework *electronically* via [Gradescope on Canvas](https://canvas.mit.edu/courses/36170) as a *PDF* containing code and results (e.g., from a Jupyter notebook) and a scan of any handwritten solutions.

* **Collaboration policy:** Talk to anyone you want to and read anything you want to, with two caveats: First, make a solid effort to solve a problem on your own before discussing it with classmates or googling. Second, no matter whom you talk to or what you read, write up the solution on your own, without having their answer in front of you (this includes ChatGPT and similar). (You can use [psetpartners.mit.edu](https://psetpartners.mit.edu/) to find problem-set partners.)

**Office Hours**: Prof. Johnson: Wednesdays at 3pm in 2-345.  Prof. Marzouk: TBD.

**Resources**: [Piazza discussion forum](https://piazza.com/mit/spring2026/18c21), [math learning center](https://math.mit.edu/learningcenter/), [TSR^2 study/resource room](https://ome.mit.edu/programs/talented-scholars-resource-room-tsr2), [pset partners](https://psetpartners.mit.edu/).

**Textbook**: No required textbook, but suggestions for further reading will be posted after each lecture.  The book [*Fundamentals of Numerical Computation* (FNC)](https://fncbook.com/) by Driscoll and Braun is **freely available online**, has examples in Julia, Python, and Matlab, and is a valuable resource.  [*Fundamentals of Engineering Numerical Analysis* (FENA)](https://www.cambridge.org/core/books/fundamentals-of-engineering-numerical-analysis/D6B6B75172AD7A5A555DC506FDDA9B99) by Moin is another useful resource ([readable online](https://www-cambridge-org.libproxy.mit.edu/core/books/fundamentals-of-engineering-numerical-analysis/D6B6B75172AD7A5A555DC506FDDA9B99) with MIT certificates).

This document is a *brief* summary of what was covered in each
lecture, along with links and suggestions for further reading.  It is
*not* a good substitute for attending lecture, but may provide a
useful study guide.

## Lecture 1 (Feb 3)

* Overview and syllabus: [slides](https://docs.google.com/presentation/d/1n6VT8jQgS0T49lmyQ_89rcplTXAR8zlxU_7c1aMqO9o/edit?usp=sharing)
* Finite-difference approximations: [Julia notebook and demo](notes/finite-differences.ipynb)

Brief overview of the huge field of numerical methods, and outline of the small portion that this course will cover. Key new concerns in numerical analysis, are (i) performance (traditionally, arithmetic counts, but now memory access often dominates) and (ii) accuracy (both floating-point roundoff errors and also convergence of intrinsic approximations in the algorithms).  In contrast, the more pure, abstract mathematics of continuity is called "analysis", and is mainly concerned with (ii) but not (i): they are happy to prove that limits converge, but don't care too much how *quickly* they converge.  Whereas traditional discrete computer science is concerned with mainly with (i) but not (ii): they care about performance and resource usage, but traditional algorithms like sorting are either right or wrong, never approximate.

As a starting example, considered the the convergence of **finite-difference approximations** to derivatives df/dx of given functions f(x), which appear in many areas of numerical analysis (such as solving differential equations) and are also closely tied to *polynomial approximation and interpolation*.   By examining the errors in the finite-difference approximation, we immediately see *two* competing sources of error: *truncation* error from the non-infinitesimal Δx, and *roundoff* error from the finite precision of the arithmetic.  Understanding these two errors will be the gateway to many other subjects in numerical methods.

**Further reading**: [FNC book: Finite differences](https://fncbook.com/finitediffs), [FENA book: chapter 2](https://www-cambridge-org.libproxy.mit.edu/core/books/fundamentals-of-engineering-numerical-analysis/numerical-differentiation-finite-differences/96E5E7ED237DF7FD71F8B0F7DCA2EF7A).  There is a lot of information online on [finite difference approximations](https://en.wikipedia.org/wiki/Finite_difference),  [these 18.303 notes](https://github.com/mitmath/18303/blob/fall16/difference-approx.pdf), or [Section 5.7 of *Numerical Recipes*](http://www.it.uom.gr/teaching/linearalgebra/NumericalRecipiesInC/c5-7.pdf).   The Julia [FiniteDifferences.jl package](https://github.com/JuliaDiff/FiniteDifferences.jl) provides lots of algorithms to compute finite-difference approximations; a particularly robust and powerful way to obtain high accuracy is to employ [Richardson extrapolation](https://github.com/JuliaDiff/FiniteDifferences.jl#richardson-extrapolation) to smaller and smaller δx.  If you make δx too small, the finite precision (#digits) of [floating-point arithmetic](https://en.wikipedia.org/wiki/Floating-point_arithmetic) leads to [catastrophic cancellation errors](https://en.wikipedia.org/wiki/Catastrophic_cancellation).


## Lecture 2 (Feb 5)

* [Floating-point introduction](notes/Floating-Point-Intro.ipynb)
* [pset 1](psets/pset1.ipynb), due Wednesday Feb. 11 at midnight.

One of the most basic sources of computational error is that **computer arithmetic is generally inexact**, leading to [roundoff errors](https://en.wikipedia.org/wiki/Round-off_error).  The reason for this is simple: computers can only work with numbers having a **finite number of digits**, so they **cannot even store** arbitrary real numbers.  Only a _finite subset_ of the real numbers can be represented using a particular number of "bits", and the question becomes _which subset_ to store, how arithmetic on this subset is defined, and how to analyze the errors compared to theoretical exact arithmetic on real numbers.

In **floating-point** arithmetic, we store both an integer coefficient and an exponent in some base: essentially, scientific notation. This allows large dynamic range and fixed _relative_ accuracy: if fl(x) is the closest floating-point number to any real x, then |fl(x)-x| < ε|x| where ε is the _machine precision_. This makes error analysis much easier and makes algorithms mostly insensitive to overall scaling or units, but has the disadvantage that it requires specialized floating-point hardware to be fast. Nowadays, all general-purpose computers, and even many little computers like your cell phones, have floating-point units.

Went through some simple definitions and examples in Julia (see notebook above), illustrating the basic ideas and a few interesting tidbits.  In particular, we looked at **error accumulation** during long calculations (e.g. summation), as well as examples of [catastrophic cancellation](https://en.wikipedia.org/wiki/Loss_of_significance) and how it can sometimes be avoided by rearranging a calculation.

**Further reading:** [FNC book: Floating-poing numbers](https://fncbook.com/floating-point).  [Trefethen & Bau's *Numerical Linear Algebra*](https://people.maths.ox.ac.uk/trefethen/text.html), lecture 13. [What Every Computer Scientist Should Know About Floating Point Arithmetic](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.22.6768) (David Goldberg, ACM 1991). William Kahan, [How Java's floating-point hurts everyone everywhere](http://www.cs.berkeley.edu/~wkahan/JAVAhurt.pdf) (2004): contains a nice discussion of floating-point myths and misconceptions.   A brief but useful summary can be found in [this Julia-focused floating-point overview](https://discourse.julialang.org/t/psa-floating-point-arithmetic/8678) by Prof. John Gibson. Because many programmers never learn how floating-point arithmetic actually works, there are [many common myths](https://github.com/mitmath/18335/blob/spring21/notes/fp-myths.pdf) about its behavior.   (An infamous example is `0.1 + 0.2` giving `0.30000000000000004`, which people are puzzled by so frequently it has led to a web site [https://0.30000000000000004.com/](https://0.30000000000000004.com/)!)

## Lecture 3 (Feb 6)

* [Julia notebook and demo on piecewise linear interpolation](notes/linear-interpolation.ipynb)

Discussed the important problem of **interpolating** a function $f(x)$ from a set of discrete data points, which shows up in a vast number of real problems and connects to many other areas of numerical methods (e.g. differentiation and integration).  To begin with, considered the simplest algorithm of [piecewise linear interpolation](https://en.wikipedia.org/wiki/Linear_interpolation) in one dimension, with points $x$ at spacing $\Delta x$, and showed that (for a twice-differentiable function) the error (the difference between the interpolant and the true function) converges as $O(\Delta x^2)$ (second-order convergence).

In particular, we looked at the *maximum* error $\max_{x \in [a,b]} |f(x) - p(x)|$ between the true function $f(x)$ and the interpolant $p(x)$.  This is also called the ["infinity norm"](https://en.wikipedia.org/wiki/Uniform_norm) $\Vert f - p \Vert_{\infty}$ as well as a variety of other names.  (Analogously, for a column vector, the infinity norm is just the [maximum absolute value](https://mathworld.wolfram.com/L-Infinity-Norm.html) of any element.)

An alternative derivation of the convergence rate, using the Taylor series of $f(x)$ on an interval, can also be found in [the 18.C21 notes from spring 2025](https://colab.research.google.com/drive/1NyU48yqYY91h2a6sZZvr1WcFUxCiDZXS?usp=sharing).

Showed some numerical examples of piecewise linear interpolation and the convergence rate (see notebook).

Finally, expressed a piecewise linear interpolant $p(x) = \sum_k f(x_k) H_k(x)$ as a linear combination of ["hat function" or "tent function"](https://en.wikipedia.org/wiki/Triangular_function) basis functions $H_k(x)$ centered at each knot.

**Further reading:** [FNC chapter 5](https://fncbook.com/overview-4) and FENA chapter 1.  Piecewise linear interpolation (among other options) is implemented  in Python by [`numpy.interp`](https://numpy.org/doc/stable/reference/generated/numpy.interp.html#numpy.interp), and several other interpolation schemes by [`scipy.interpolate`](https://docs.scipy.org/doc/scipy-1.15.1/tutorial/interpolate.html).  Interpolation packages in Julia include [Interpolations.jl](https://github.com/JuliaMath/Interpolations.jl) and [BasicInterpolators.jl](https://github.com/markmbaum/BasicInterpolators.jl) (both of which include piecewise-linear options), [Dierckx.jl (splines)](https://github.com/JuliaMath/Dierckx.jl), and [FastChebInterp.jl (high-order polynomials)](https://github.com/JuliaMath/FastChebInterp.jl).

## Optional Julia Tutorial (Feb 6 @ 5pm in 34-101)

A basic overview of the Julia programming environment for numerical computations.   This tutorial will cover what Julia is and the basics of interaction, scalar/vector/matrix arithmetic, and plotting — just as a "fancy calculator" for now (without the "real programming" features).

* [Tutorial materials](https://github.com/mitmath/julia-mit) (and links to other resources)

If possible, try to install Julia on your laptop beforehand using the instructions at the above link.  Failing that, you can run Julia in the cloud (see instructions above).

This won't be recorded, but you can find a [video of a similar tutorial by Prof. Johnson a previous year (MIT only)](https://mit.zoom.us/rec/share/oirQFHELtxJkopybssFzml7YrudRyvIlmXjmgq4YemqmjT0P7wMGCd9ilC7SMZ_o.iBZ-UO6_ww9WjwF0?startTime=1705960822000), as well as many other tutorial videos at [julialang.org/learning](https://julialang.org/learning/).


## Lecture 4 (Feb 9)
* [Julia notebook on polynomial interpolation](polynomial-interpolation.ipynb)

One approach to generalizing piecewise-linear interpolation is to interpolate $n$ points with a polynomial $p(x)$ of degree of degree $n-1$.  This is an important technique for many applications, and the general topic of approximating functions by polynomials has vast importance in numerical analysis, but requires care if $n$ becomes much larger than 4 (cubic) or so, even for smooth functions (no noise).

A general conceptual approach is to set up a system of linear equations for the polynomial coefficients $c_i$, satisfying $p(x_k) = y_k$ for each data point $(x_k, y_k)$.  This can be expressed in matrix form $Ac = y$, where the $n \times n$ matrix $A$ is known as a [Vandermonde matrix](https://en.wikipedia.org/wiki/Vandermonde_matrix) if you are using the usual monomial basis $p(x) = c_0 + c_1 x + c_2 x^2 + \cdots$.   One quickly runs into two difficulties, however:

1. Polynomial interpolation from **equally spaced points** (in *any* basis!) can **diverge exponentially** from the underlying "true" smooth function **in between the points** (even in exact arithmetic, with no roundoff errors!).  This is called a [Runge phenomenon](https://en.wikipedia.org/wiki/Runge%27s_phenomenon).   **Solution 1**: Use carefully chosen non-equispaced points.  A good choice that leads to *exponentially good* polynomial approximations (for smooth functions) is the [Chebyshev nodes](https://en.wikipedia.org/wiki/Chebyshev_nodes), which are clustered near the endpoints.  **Solution 2**: use a *lower* degree ($<< n$) polynomial and perform an *approximate fit* to the given points, rather than requiring the polynomial to go through them exactly.  (More on this soon.)

2. The matrix $A$ is **nearly singular** for large $n$ in the monomial basis, so floating-point roundoff errors are exacerbated.  (We will say that it has a large [condition number](https://en.wikipedia.org/wiki/Condition_number) or is "ill-conditioned", and will define this more precisely next time.)   **Solution:** It turns out that monomials are just a poorly behaved basis for high-degree polynomials, and it is much better to use [orthogonal polynomials](https://en.wikipedia.org/wiki/Orthogonal_polynomials), most commonly [Chebyshev polynomials](https://en.wikipedia.org/wiki/Chebyshev_polynomials), as a basis — with these, people regularly go to degrees of thousands or even millions.

If you address both of these problems, high-degree polynomial approximation can be a fantastic tool for describing *smooth* functions.  If you have *noisy* data, however, you should typically use much lower-degree polynomials to avoid [overfitting](https://en.wikipedia.org/wiki/Overfitting) (trying to match the noise rather than the underlying "true" function).

**Further reading**: [FNC book, chapter 9](https://fncbook.com/polynomial).  Beware that the FENA book starts with the ["Lagrange formula"](https://en.wikipedia.org/wiki/Lagrange_polynomial) for the interpolating polynomial, but this formula is very badly behaved ("numerically unstable") for high degrees and should not be used; it is superseded by the "barycentric" Lagrange formula (see FNC book; [reviewed in much more detail by Berrut and Trefethen, 2004](https://people.maths.ox.ac.uk/trefethen/barycentric.pdf)).  The subject of polynomial interpolation is the entry point into [approximation theory](https://en.wikipedia.org/wiki/Approximation_theory); if you are interested, the [book by Trefethen](https://people.maths.ox.ac.uk/trefethen/ATAP/) and accompanying [video lectures](https://people.maths.ox.ac.uk/trefethen/atapvideos.html) are a great place to get more depth.    The [`numpy.polynomials`](https://numpy.org/doc/2.2/reference/routines.polynomials.html) module contains a variety of functions for polynomial interpolation, including with Chebyshev polynomials.  A package for multi-dimensional Chebyshev polynomial interpolation in Julia is [FastChebInterp](https://github.com/JuliaMath/FastChebInterp.jl).  A pioneering package that shows off the power of Chebyshev polynomial interpolation is [`chebfun` in Matlab](https://www.chebfun.org/), along with [related packages in other languages](https://www.chebfun.org/about/projects.html).  (This approach is taken to supercomputing extremes by the [Dedalus package](https://dedalus-project.org/) to solve partial differential equations using exponentially converging polynomial approximations.)   It turns out that if you are interpolating using Chebyshev polynomials, and you choose your points $x_k$ to be Chebyshev points (either extrema or roots of the degree $n$ Chebyshev polynomial), then you don't need to form any Vandermonde-like matrix explicitly — you can get the coefficients *much* more quickly, in $O(n \log n)$ operations, using [fast Fourier transforms (FFTs)](https://en.wikipedia.org/wiki/Fast_Fourier_transform); given these, a polynomial in the Chebyshev basis can then be evaluated at any $x$ with $O(n)$ operations using the [Clenshaw algorithm](https://en.wikipedia.org/wiki/Clenshaw_algorithm).  If you don't need the explicit coefficients, the barycentric Lagrange formula (for an arbitrary set of points) has $O(n^2)$ setup cost to obtain a set of weights from the $x_k$ — still cheaper than the $O(n^3)$ cost of solving the Vandermonde system — after which the polynomial can be evaluated cheaply ($`O(n)`$) at any point $x$ similar to the other schemes.
