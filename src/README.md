# The latent logarithm
This folder contains the source code for the latent logarithm, or lag. `demo_lag.m` contains all of the code used to perform analyses presented in the paper (including figure generation). 

## Usage
The two functions of primary interest are `lag.m` and `lag_plus_learn_prior.m`. Both functions require a vector of non-negative data to be modeled (`t`) and a vector of offsets/confidences/exposures/sampling depths (`o`). `lag.m` additionally requires specification of the prior mean and variance, wherease `lag_plus_learn_prior.m` learns these values. 

Both output the log-latent rates, `z`. 
* `z` should be approximately equal to `log(t./o)` (will have to add a pseudocount to avoid `-Inf`). This is nlag in the paper.
* `z + log(o)` should be approximately equal to `log(t)` (will have to add a pseudocount to avoid `-Inf`). This is lag in the paper.

An example of their use is given below:

```
% Set up a clean environment
clear;
rng('default');
cd('~/GitHub/latent_log'); % Change accordingly
path(genpath(pwd), path);

% Load the data used in the paper:
% t, t2, and o into memory. 
% t and t2 are the TPM abundances for the 'rare' and 'abundant' gene respectively.
% o contains the TPM offsets.
load('AT2G43386_AT1G14630.mat'); 

% First set a  prior mean and variance and let lag infer the log latent TPMs (rates)
prior.mu = 0.25;
prior.sig2 = 0.05;
z1 = lag(t, o, prior);

% Now also learn the prior in addition to the log latent TPMs (rates)
z2 = lag_plus_learn_prior(t, o);

```

Send questions/comments/suspected bugs to surojitbiswas@g.harvard.edu

