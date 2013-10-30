# Optimal PWA fitting scheme

## Literature

Takacs, B., Holaza, J., Kvasnica, M., Di Cairano, S.: Nearly-Optimal Simple Explicit MPC Regulators with Recursive Feasibility Guarantees. To appear at CDC'13.

## Principle

Given PWA feedback law is approximated by a different PWA function of lower complexity such that the approximation error is minimized.

## Prerequisites

Continuous or discontinuous PWA feedback law.

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