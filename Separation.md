# Separation-based complexity reduction

## Literature

Kvasnica, M., Hledik, J., Rauova, I., Fikar, M.: [Complexity reduction of explicit model predictive control via separation](http://www.sciencedirect.com/science/article/pii/S0005109813001076). Automatica, no. 6, vol. 49, pp. 1776-1781, 2013.

## Principle

Regions in which the optimal control action is saturated are completely removed. To restore equivalence, a separation function is employed.

## Prerequisites

Continuous PWA feedback law.

## Implementation

See `help SeparationController`

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
    
    % reduce complexity using separation
    separated = SeparationController(ctrl);
    
    % number of regions of the original controller
    ctrl.nr
    
    ans =
    
        33
        
    % number of regions of the separation-based controller
    separated.nr
    
    ans =
    
        9
        
    % evaluate both controllers for a particular state
    x0 = [2; 0.9];
    uopt = ctrl.evaluate(x0);
    ufit = separated.evaluate(x0);
    [uopt, ufit]
    
    ans =
    
        -1.0000   -1.0000