<html lang="en">

<head>
<link rel="stylesheet" href="lib/bootstrap.min.css">
<script src="lib/jquery-3.3.1.slim.min.js"></script>
<script src="lib/popper.min.js"></script>
<script src="lib/bootstrap.min.js"></script>

<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>

<meta name="viewport" content="width=device-width, initial-scale=1">
<meta charset="utf-8">
<title>M. Zhou / Debian</title>
</head>

<body>
<div class='container'>
<div class="card">
<h5 class="card-header">Math::Optimization</h5>
<div class="card-body">

A simple note about optimization, using Julia language.

## Terminations

* **LP**: Linear Programming
* **QP**: Quadratic Programming
* **MILP**: Mixed-Integer Linear Programming
* **NLP**: Non-Linear Programming
* **MINLP**: Mixed-Integer Non-Linear Programming (Better avoid this case)
* **SDP**: Semidefinite programming

## A Simple LP Problem

\\[
\\begin{align}
\\max \\qquad & 5x+3y \\\\
s.t.\\qquad & 1x + 5y \\leq 3 \\\\
& 0 \\leq x \\leq 2 \\\\
& 0 \\leq y \\leq 30
\\end{align}
\\]

```julia
julia> using JuMP, GLPK

julia> M = Model(with_optimizer(GLPK.Optimizer))
A JuMP Model
Feasibility problem with:
Variables: 0
Model mode: AUTOMATIC
CachingOptimizer state: EMPTY_OPTIMIZER
Solver name: GLPK

julia> @variable(M, 0 <= x <= 2)
x

julia> @variable(M, 0 <= y <= 30)
y

julia> @constraint(M, 1x + 5y <= 3.0)
x + 5 y ≤ 3.0

julia> @objective(M, Max, 5x+3y)
5 x + 3 y

julia> optimize!(M)

julia> value.([x, y])
2-element Array{Float64,1}:
 2.0
 0.2
```

# Knapsack: An ILP problem

Maximize total price under the weight constraint.

\\[
\\begin{align}
\\max\\qquad & \\sum\_j p\_j x\_j \\\\
s.t.\\qquad & \\sum\_j w\_j x\_j \\\\
& \\forall j, x\_j \\in {0,1}
\\end{align}
\\]

```julia
julia> using JuMP, GLPK

julia> M = Model(with_optimizer(GLPK.Optimizer))
A JuMP Model
Feasibility problem with:
Variables: 0
Model mode: AUTOMATIC
CachingOptimizer state: EMPTY_OPTIMIZER
Solver name: GLPK

julia> profit = [5, 3, 2, 7, 4];

julia> weight = [2, 8, 4, 2, 5];

julia> @variable(M, x[1:5], Bin);

julia> capacity = 10;

julia> @constraint(M, weight' * x <= capacity)
2 x[1] + 8 x[2] + 4 x[3] + 2 x[4] + 5 x[5] ≤ 10.0

julia> @objective(M, Max, profit' * x)
5 x[1] + 3 x[2] + 2 x[3] + 7 x[4] + 4 x[5]

julia> optimize!(M)

julia> value.(M[:x])
5-element Array{Float64,1}:
 1.0
 0.0
 0.0
 1.0
 1.0

julia> value.(M[:x])' * profit
16.0
```

## Sudoku: An ILP problem

TODO

## Linearization of an NL problem

A toy problem.

\\[
\\begin{align}
\\max\\qquad & \\sum_i x_i \\\\
s.t.\\qquad& \\prod_i x_i = 0
\\end{align}
\\]

[Linearization](https://math.stackexchange.com/questions/1798077/how-to-convert-a-non-linear-constraint-to-a-linear-constraint-for-integer-progra):

\\[
\\begin{align}
y &= x_1 x_2 x_3 \\\\
y &\\leq x_i, \\forall i \\\\
y &\\geq (\\sum_i x_i ) - (3 - 1) \\\\
y &\\in \\{ 0, 1\\}
\\end{align}
\\]

```julia
julia> using JuMP, GLPK

julia> M = Model(with_optimizer(GLPK.Optimizer))
A JuMP Model
Feasibility problem with:
Variables: 0
Model mode: AUTOMATIC
CachingOptimizer state: EMPTY_OPTIMIZER
Solver name: GLPK

julia> @variable(M, x[1:3], Bin)
3-element Array{VariableRef,1}:
 x[1]
 x[2]
 x[3]

julia> @objective(M, Max, sum(x))
x[1] + x[2] + x[3]

julia> @variable(M, y, Bin)
y

julia> @constraints(M, begin
       y == 0
       y <= x[1]
       y <= x[2]
       y <= x[3]
       y >= sum(x) -2
       end)

julia> optimize!(M)

julia> value.(x)
3-element Array{Float64,1}:
 1.0
 1.0
 0.0

julia> value.(y)
0.0
```

## References

1. http://www.juliaopt.org/JuMP.jl/v0.20/installation/
2. https://github.com/JuliaOpt/JuMP.jl/blob/master/examples/
3. https://ww2.mathworks.cn/help/optim/linear-programming-and-mixed-integer-linear-programming.html?s_tid=CRUX_lftnav
4. https://ww2.mathworks.cn/help/optim/ug/intlinprog.html
5. https://math.stackexchange.com/questions/1798077/how-to-convert-a-non-linear-constraint-to-a-linear-constraint-for-integer-progra
6. https://www.researchgate.net/post/How_can_we_convert_the_non-linear_expression_to_the_linear_constraints
7. https://www.researchgate.net/post/How_to_convert_a_problem_that_contains_a_binary_state_variable_in_one_of_its_constraints_into_a_linear_problem
8. MIT Integer Programming http://web.mit.edu/15.053/www/AMP-Chapter-09.pdf

</div>
</div>
</div>
</body>
</html>
