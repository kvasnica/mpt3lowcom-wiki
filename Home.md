## About

The **mpt3lowcom** package extends the [Multi-Parametric Toolbox 3](http://control.ee.ethz.ch/~mpt/3/) by adding methods for generating low-complexity explicit MPC controllers.

Currently implemented methods:

* [Clipping-based complexity reduction](Clipping).
* [Optimal PWA fitting scheme](Fitting).
* [Separation-based complexity reduction](Separation).
* [Minimum-time control](Mintime).

Note that this package contains experimental code. Do expect some limitations. Use generated controllers at your own risk.

## Users guide

More information about low-complexity explicit MPC synthesis in MPT3 can be found in our [technical report](http://www.kirp.chtf.stuba.sk/publication_info.php?id_pub=1581).

## Installation

1. Install MPT3 per [these instructions](http://control.ee.ethz.ch/~mpt/3/Main/Installation)
2. Install the **mpt3lowcom** module by running `tbxmanager install mpt3lowcom` in Matlab

## Feedback

Send questions/comments/bug reports to `mpt@control.ee.ethz.ch`