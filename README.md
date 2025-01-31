# Interdisciplinary Numerical Methods: "Hub" 18.S190/16.S090

This new MIT course (**Spring 2025**) introduces numerical methods and numerical analysis to a broad audience (assuming 18.03, 18.06, or equivalents, and some programming experience).  It is divided into two 6-unit halves:

*  **18.S190/16.S090** (first half-term “hub”): basic numerical methods, including curve fitting, root finding, numerical differentiation and integration, numerical differential equations, and floating-point arithmetic. Emphasizes the complementary concerns of accuracy and computational cost.  [Prof. Steven G. Johnson](http://math.mit.edu/~stevenj) and [Prof. Qiqi Wang](https://aeroastro.mit.edu/people/qiqi-wang/).

*   Second half-term: three options for 6-unit “spokes”

    *   18.S191/16.S091 — numerical methods for partial differential equations: finite-difference and finite-volume methods, boundary conditions, accuracy, and stability. [Prof. Qiqi Wang](https://aeroastro.mit.edu/people/qiqi-wang/).

    *   18.S097/16.S097 — large-scale linear algebra: sparse matrices, iterative methods, randomized methods. [Prof. Steven G. Johnson](http://math.mit.edu/~stevenj).

    *   18.S192/16.S098 — parallel numerical computing: multi-threading and distributed-memory, and trading off computation for parallelism — _may be taken simultaneously_ with other spokes! [Prof. Alan Edelman](https://math.mit.edu/~edelman/).

Taking both the hub and any spoke will count as an 18.3xx class for math majors, similar to 18.330.  Taking both the hub and the PDE spoke will substitute for 16.90. Weekly homework, *no exams*, but spokes will include a final project.

This repository is for the "hub" course (currently assigned the temporary numbers 18.S190/16.S090).

## 18.S190/16.S090 Syllabus, Spring 2025

**Instructors**: [Prof. Steven G. Johnson](http://math.mit.edu/~stevenj) and [Prof. Qiqi Wang](https://aeroastro.mit.edu/people/qiqi-wang/).

**Lectures**: MWF10 in 2-143 (Feb 3 – Mar 31), slides and notes posted below.

**Homework and grading**: 6 weekly psets, posted Fridays and due Friday *midnight*; psets are accepted up to 24 hours late with a 20% penalty; for any other accommodations, speak with [S3](https://studentlife.mit.edu/s3) and have them contact the instructors.  Homework assignments will require some programming — you can use either **Julia or Python** (your choice; instruction and examples will use a mix of languages). No exams.

* **Collaboration policy:** Talk to anyone you want to and read anything you want to, with two caveats: First, make a solid effort to solve a problem on your own before discussing it with classmates or googling. Second, no matter whom you talk to or what you read, write up the solution on your own, without having their answer in front of you. (You can use [psetpartners.mit.edu](https://psetpartners.mit.edu/) to find problem-set partners.)

**Teaching Assistants**: [Mo Chen](https://math.mit.edu/directory/profile.html?pid=2176) and TBD.

**Office Hours**: TBD.

**Resources**: [Piazza discussion forum](TBD), [math learning center](https://math.mit.edu/learningcenter/), [TSR^2 study/resource room](https://ome.mit.edu/programs/talented-scholars-resource-room-tsr2), [pset partners](https://psetpartners.mit.edu/).

**Textbook**: No required textbook, but suggestions for further reading will be posted after each lecture.  The book [*Fundamentals of Numerical Computation* (FNC)](https://fncbook.com/) by Driscoll and Braun is **freely available online**, has examples in Julia, Python, and Matlab, and is a valuable resource.

This document is a *brief* summary of what was covered in each
lecture, along with links and suggestions for further reading.  It is
*not* a good substitute for attending lecture, but may provide a
useful study guide.

## Lecture 1 (Feb 3)

* Overview and syllabus
* Finite-difference approximations: notes and demos (to be linked).

Brief overview of the huge field of numerical methods, and outline of the small portion that this course will cover. Key new concerns in numerical analysis, are (i) performance (traditionally, arithmetic counts, but now memory access often dominates) and (ii) accuracy (both floating-point roundoff errors and also convergence of intrinsic approximations in the algorithms).  In contrast, the more pure, abstract mathematics of continuity is called "analysis", and is mainly concerned with (ii) but not (i): they are happy to prove that limits converge, but don't care too much how *quickly* they converge.  Whereas traditional discrete computer science is concerned with mainly with (i) but not (ii): they care about performance and resource usage, but traditional algorithms like sorting are either right or wrong, never approximate.

As a starting example, considered the the convergence of **finite-difference approximations** to derivatives df/dx of given functions f(x), which appear in many areas of numerical analysis (such as solving differential equations) and are also closely tied to *polynomial approximation and interpolation*.   By examining the errors in the finite-difference approximation, we immediately see *two* competing sources of error: *truncation* error from the non-infinitesimal Δx, and *roundoff* error from the finite precision of the arithmetic.  Understanding these two errors will be the gateway to many other subjects in numerical methods.

**Further reading**: [FNC book: Finite differences](https://fncbook.com/finitediffs).  There is a lot of information online on [finite difference approximations](https://en.wikipedia.org/wiki/Finite_difference),  [these 18.303 notes](https://github.com/mitmath/18303/blob/fall16/difference-approx.pdf), or [Section 5.7 of *Numerical Recipes*](http://www.it.uom.gr/teaching/linearalgebra/NumericalRecipiesInC/c5-7.pdf).   The Julia [FiniteDifferences.jl package](https://github.com/JuliaDiff/FiniteDifferences.jl) provides lots of algorithms to compute finite-difference approximations; a particularly robust and powerful way to obtain high accuracy is to employ [Richardson extrapolation](https://github.com/JuliaDiff/FiniteDifferences.jl#richardson-extrapolation) to smaller and smaller δx.  If you make δx too small, the finite precision (#digits) of [floating-point arithmetic](https://en.wikipedia.org/wiki/Floating-point_arithmetic) leads to [catastrophic cancellation errors](https://en.wikipedia.org/wiki/Catastrophic_cancellation).

## Lecture 2 (Feb 5)

* [Floating-point introduction](https://github.com/mitmath/18065/blob/e4ff01607ef3997ba6ac7fc95c00af0a6e8903df/notes/Floating-Point-Intro.ipynb)

One of the most basic sources of computational error is that **computer arithmetic is generally inexact**, leading to [roundoff errors](https://en.wikipedia.org/wiki/Round-off_error).  The reason for this is simple: computers can only work with numbers having a **finite number of digits**, so they **cannot even store** arbitrary real numbers.  Only a _finite subset_ of the real numbers can be represented using a particular number of "bits", and the question becomes _which subset_ to store, how arithmetic on this subset is defined, and how to analyze the errors compared to theoretical exact arithmetic on real numbers.

In **floating-point** arithmetic, we store both an integer coefficient and an exponent in some base: essentially, scientific notation. This allows large dynamic range and fixed _relative_ accuracy: if fl(x) is the closest floating-point number to any real x, then |fl(x)-x| < ε|x| where ε is the _machine precision_. This makes error analysis much easier and makes algorithms mostly insensitive to overall scaling or units, but has the disadvantage that it requires specialized floating-point hardware to be fast. Nowadays, all general-purpose computers, and even many little computers like your cell phones, have floating-point units.

Went through some simple definitions and examples in Julia (see notebook above), illustrating the basic ideas and a few interesting tidbits.  In particular, we looked at **error accumulation** during long calculations (e.g. summation), as well as examples of [catastrophic cancellation](https://en.wikipedia.org/wiki/Loss_of_significance) and how it can sometimes be avoided by rearranging a calculation.

**Further reading:** [FNC book: Floating-poing numbers](https://fncbook.com/floating-point).  [Trefethen & Bau's *Numerical Linear Algebra*](https://people.maths.ox.ac.uk/trefethen/text.html), lecture 13. [What Every Computer Scientist Should Know About Floating Point Arithmetic](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.22.6768) (David Goldberg, ACM 1991). William Kahan, [How Java's floating-point hurts everyone everywhere](http://www.cs.berkeley.edu/~wkahan/JAVAhurt.pdf) (2004): contains a nice discussion of floating-point myths and misconceptions.   A brief but useful summary can be found in [this Julia-focused floating-point overview](https://discourse.julialang.org/t/psa-floating-point-arithmetic/8678) by Prof. John Gibson. Because many programmers never learn how floating-point arithmetic actually works, there are [many common myths](https://github.com/mitmath/18335/blob/spring21/notes/fp-myths.pdf) about its behavior.   (An infamous example is `0.1 + 0.2` giving `0.30000000000000004`, which people are puzzled by so frequently it has led to a web site [https://0.30000000000000004.com/](https://0.30000000000000004.com/)!)

## Lecture 3 (Feb 7)

* Interpolation
