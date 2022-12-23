Some Notes
----------

BLAS64 Convention
^^^^^^^^^^^^^^^^^

https://lists.debian.org/debian-devel/2018/10/msg00339.html

The preliminary decision is "blas64" instead of "blas-ilp64".
reference: libpcre*, BLIS, dsfmt API/ABI changed but API is compatible.

Benchmarks
^^^^^^^^^^

https://openbenchmarking.org/result/1802101-FO-THREADCOR29

Julia Specific Notes
^^^^^^^^^^^^^^^^^^^^

* upstream about to drop openlibm
  https://github.com/JuliaLang/julia/issues/26434

* external julia packages?
  https://discourse.julialang.org/t/binary-dependencies/11623

* https://github.com/JuliaLang/PackageCompiler.jl

* https://discourse.julialang.org/t/creating-a-registry/12094

* Static and Ahead of Time (AOT) Compiled Julia
  https://juliacomputing.com/blog/2016/02/09/static-julia.html

* We can write an optimized BLAS library in pure Julia
  https://discourse.julialang.org/t/we-can-write-an-optimized-blas-library-in-pure-julia-please-skip-op-and-jump-to-post-4/11634
