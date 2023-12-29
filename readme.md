# Comparison of Julia's Turing and PYMC for ODE inference

Dec 2023

* Goal is to compare the same ODE parameter inference problem implemented in TuringLang and in PYMC (using sunode) 

* The model chosen is the Lotka-Volterra model, which is a simple predator-prey model. I use simulated data, but the parameters are chosen to reproduce qualitatively the data used in the [sunode documentation](https://sunode.readthedocs.io/en/latest/quickstart_pymc.html). I believe the data is from the Hudson Bay Company, but I am not sure.

* `turing_test.ipynb` is the Turing Language version, and also generates the simulated data used for both. 

*  `sunode_test.ipynb` is the PyMC version, which uses the sunode library to solve the ODEs as recommended by the [PyMC Developers](https://www.pymc.io/projects/docs/en/stable/api/ode.html).

* This was just a quick look, I didn't try to be careful with the timings, as I only want to compare order of magnitudes.    On my M3 Macbook Pro,  The PyMC model took 2 minutes to sample 4 chains of 1000, while the Turing model took 2 minutes as well, however the Turing model was doing the chains in serial not parallel as in the PyMC model.  It is possible to do the Turing sampling in parallel but not straightforward.

* On windows, the PyMC model took 2.5 minutes per chain, that is 10 minutes total. This is because you cannot do parallel chains on windows with sunode yet. The Turning model took 2.25 minutes for all 4 chains.  Again these are not scientific comparisons, but the Turing model is certainly not slower than the PYMC model and could be up to 4 times faster.

* IMHO the Julia Turing code *looks* cleaner and easier to understand. The PyMC code requires understanding the PyTensor, sunode, PyMC and ArviZ APIs.  Each of these API's have there little quirks you have to learn. For example, currently the 'extra' parameters in the sunode wrapper call are required to make it work. This is a known issue for some time.  

* Another issue I found is that dealing with PyTensor can be a pain. For example, in Julia, to implement a "log if positive" function, I could just define the function in the most straightforward way. In PyMC, I had to use special PyTensor functions to make this happen. On the other hand, it is not really necessary for the PYMC version.


* In `sunode_test.ipynb` I also tested `pymc.ode` which is *much* slower (about 25 minutes *per* chain on my windows pc). However it does support multiple cores. It is still faster to run 4 chains in series using Sunode, by a factor of about 2, but the pymc.ode implementation is a bit cleaner *looking*.  