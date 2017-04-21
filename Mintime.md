# Minimum-time control

## Literature

Grieder, P., Kvasnica, M., Baotic, M., Morari, M.: [Stabilizing low complexity feedback control of constrained piecewise affine systems](http://www.sciencedirect.com/science/article/pii/S0005109805001482). Automatica, no. 10, vol. 41, pp. 1683–1694, 2005.

## Principle

Complexity is reduced by solving a time-optimal control problem where the objective is to reach a given terminal set in a minimal number of time steps.

## Prerequisites

LTI or PWA prediction model.

## Implementation

See `help EMinTimeController`

## Example

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
    
    % add a terminal set constraint
    model.x.with('terminalSet');
    model.x.terminalSet = model.LQRSet();

    % construct the minimum-time MPC controller
    mintime = EMinTimeController(model)
    
    % obtain time-optimal control action for a particular state
    x0 = [2; 0.9];
    u = mintime.evaluate(x0)
    
    u =

        -1