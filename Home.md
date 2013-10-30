# About

The **mpt3lowcom** package for [MPT3](http://control.ee.ethz.ch/~mpt/3/) contains methods for generating low-complexity explicit MPC controllers.

Currently implemented methods:

* clipping-based complexity reduction ([paper](http://ieeexplore.ieee.org/xpl/articleDetails.jsp?arnumber=6099563))
* optimal PWA fitting scheme
* separation-based complexity reduction ([paper](http://www.sciencedirect.com/science/article/pii/S0005109813001076))
* minimum-time control ([paper](http://www.sciencedirect.com/science/article/pii/S0005109805001482))

Note that this package contains experimental code. Do expect some limitations. Use generated controllers at your own risk.

## Installation

1. Install MPT3 per [these instructions](http://control.ee.ethz.ch/~mpt/3/Main/Installation)
2. Install the **mpt3lowcom** module by running `tbxmanager install mpt3lowcom` in Matlab

## Feedback

Send questions/comments to `mpt@control.ee.ethz.ch`