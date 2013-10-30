# Clipping-based complexity reduction

## Literature

Kvasnica, M. and Fikar, M.: [Clipping-Based Complexity Reduction in Explicit MPC](http://ieeexplore.ieee.org/xpl/articleDetails.jsp?arnumber=6099563). IEEE Transactions on Automatic Control, no. 7, vol. 57, pp. 1878-1883, 2012.

## Principle

Regions of a PWA feedback where the control action is saturated are completely eliminated and replaced by extensions of unsaturated regions.

## Prerequisites

Continuous PWA feedback law.

## Implementation

See `help ClippingController`

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
    
    % reduce complexity using clipping
    clipped = ClippingController(ctrl);
    
    % number of regions of the original controller
    ctrl.nr
    
    ans =
    
        33
        
    % number of regions of the clipping controller
    clipped.nr
    
    ans =
    
        9
        
    % evaluate the controller for a particular state
    x0 = [2; 0.9];
    u = clipped.evaluate(x0)
    
    u =
    
        -1