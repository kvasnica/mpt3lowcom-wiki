# Optimal PWA fitting scheme

## Literature

Takacs, B., Holaza, J., Kvasnica, M., Di Cairano, S.: [Nearly-Optimal Simple Explicit MPC Regulators with Recursive Feasibility Guarantees](http://www.kirp.chtf.stuba.sk/assets/publication_info.php?id_pub=1469). In IEEE Conference on Decision and Control, Florence, Italy, pp. 7089–7094, 2013.

## Principle

Given PWA feedback law is approximated by a different PWA function of lower complexity such that the approximation error is minimized.

## Prerequisites

Continuous or discontinuous PWA feedback law.

## Implementation

See `help FittingController`

## Example

    % double integrator dynamics
    model = LTISystem('A', [1 1; 0 1], 'B', [1; 0.5]);
    
    % state constraints
    model.x.min = [-10; -5];
    model.x.max = [10; 5];
    
    % input constraints
    model.u.min = -1;
    model.u.max = 1;
    
    % state and input penalties (regulation to the origin)
    model.x.penalty = QuadFunction(eye(2));
    model.u.penalty = QuadFunction(1);
    
    % prediction horizon
    N = 5;
    
    % construct the explicit MPC controller
    ctrl = MPCController(model, N).toExplicit();
    
    % reduce complexity using fitting
    fitted = FittingController(ctrl);
    
    % number of regions of the original controller
    ctrl.nr
    
    ans =
    
        33
        
    % number of regions of the fitted controller
    fitted.nr
    
    ans =
    
        13
        
    % evaluate both controllers for a particular state
    x0 = [2; 0.9];
    uopt = ctrl.evaluate(x0);
    ufit = fitted.evaluate(x0);
    [uopt, ufit]
    
    ans =
    
        -1.0000   -0.3666

## Optimal fitting of arbitrary PWA functions

Sometimes you would like to optimally approximate a generic PWA function over a specific partition. The example below shows how to achieve that, under the assumption that the PWA function to be approximated was generated by the `Opt/solve()` method:

    % formulate an optimization problem (see "help Opt")
    %    min u'*u + x'*u
    %   s.t. -10 <= u + x <= 10
    %        -1 <= u <= 1
    %        -10 <= x <= 10
    %
    H = 1; % penalty in u'*H*u
    F = 1; % penalty in x'*F*u
    % constraints in the form A*u <= b + E*x
    A = [1; -1; 1; -1; 0; 0];
    b = [10; 10; 1; 1; 10; 10];
    E = [-1; 1; 0; 0; -1; 1];
    % formulate the parametric optimization problem
    problem = Opt('H', 1, 'pF', 1, 'A', A, 'b', b, 'pB', E);
    % generate the PWA solution
    solution = problem.solve(); 
    
    % specify the partition of the simple optimizer
    % (in our case we will use triangulation of the domain)
    T = solution.xopt.Domain.triangulate();

    % partition of the simple optimizer must be a PolyUnion object
    new_partition = PolyUnion(T);

    % prepare data for the fitting procedure
    nx = solution.xopt.Dim; % number of parameters
    nz = solution.xopt.Set(1).Functions('primal').R; % number of optimization variables
    zmin = -Inf(nz, 1); % lower limit on the approximate value
    zmax = Inf(nz, 1); % upper limit on the approximate value

    % perform the optimal fitting
    fitted = FittingController.refine(solution.xopt, ...
        new_partition, ...
        zmin, ...
        zmax, ...
        Polyhedron.unitBox(nx), ...
        {}, ...
        {}, ...
        struct('invariance', false))

    % plot the original and the simple optimizer
    solution.xopt.fplot('primal');
    figure; fitted.fplot('primal');

## Optimal fitting of PWA functions obtained by YALMIP's `solvemp`

Simply convert the YALMIP solution, generated by `solvemp`, to MPT3's `PolyUnion` object:

    solution = mpt_mpsol2pu(yalmip_sol)

and then use the procedure reported above.