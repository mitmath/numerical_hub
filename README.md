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

**Lectures**: MWF10 in 2-142 (Feb 2 – Mar 20), slides and notes posted below.

**Homework and grading**: 5 weekly psets, due Wednesdays at midnight (Feb. 11, 18, 25; Mar. 11, 18).  Students will have oral check-ins on 3 psets (randomly selected) where they have to explain their work (pass/fail per problem). One in-class exam, March 6. For accommodations, speak with [S3](https://studentlife.mit.edu/s3) and have them contact the instructors.   Grade percentages 40% psets, 30% check-ins, 30% exam.

* Homework assignments will require some programming — you can use either **Julia or Python** (your choice; instruction and examples will use a mix of languages).

* Submit your homework *electronically* via [Gradescope on Canvas](https://canvas.mit.edu/courses/36170) as a *PDF* containing code and results (e.g., from a Jupyter notebook) and a scan of any handwritten solutions.

* **Collaboration policy:** Talk to anyone you want to and read anything you want to, with two caveats: First, make a solid effort to solve a problem on your own before discussing it with classmates or googling. Second, no matter whom you talk to or what you read, write up the solution on your own, without having their answer in front of you (this includes ChatGPT and similar). (You can use [psetpartners.mit.edu](https://psetpartners.mit.edu/) to find problem-set partners.)

**Office Hours**: TBD

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
