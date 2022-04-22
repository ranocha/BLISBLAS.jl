# BLISBLAS.jl

BLISBLAS.jl is a Julia package that allows users to use the [BLIS](https://github.com/flame/blis) library for Julia's underlying BLAS. Note that OpenBLAS, which Julia ships by default, will still be used for LAPACK functionality (BLIS only provides BLAS!).

This package is based on [libblastrampoline](https://github.com/JuliaLinearAlgebra/libblastrampoline), which enables picking a BLAS (and/or LAPACK) library at runtime, and **requires Julia 1.7+**.

## Installation

```
] add https://github.com/carstenbauer/BLISBLAS.jl
```

## Usage

Simply `using BLISBLAS` is enough:
```julia
julia> using LinearAlgebra

julia> BLAS.get_config()
LinearAlgebra.BLAS.LBTConfig
Libraries: 
└ [ILP64] libopenblas64_.so

julia> using BLISBLAS

julia> BLAS.get_config()
LinearAlgebra.BLAS.LBTConfig
Libraries: 
├ [ILP64] libopenblas64_.so
└ [ILP64] libblis.so
```

The following simple example is run with `OPENBLAS_NUM_THREADS=64` and `BLIS_NUM_THREADS=64` on a 64-core AMD EPYC 7763 (Milan) CPU.

```julia
julia> using BenchmarkTools

julia> A = rand(1000,1000); B = rand(1000,1000);

julia> @btime $A * $B;
  3.927 ms (2 allocations: 7.63 MiB)

julia> using BLISBLAS

julia> @btime $A * $B;
  2.729 ms (2 allocations: 7.63 MiB)
```

## Related packages

* A more manual approach to accessing BLIS is provided by [BLIS.jl](https://github.com/JuliaLinearAlgebra/BLIS.jl)
* [MKL.jl](https://github.com/JuliaLinearAlgebra/MKL.jl) for Intel's MKL