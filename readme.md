# Comparison of Julia's Turing and PYMC for ODE inference

* Goal is to compare the same ODE parameter inference problem implemented in TuringLang and in PYMC (using sunode) 

* The model chosen is the Lotka-Volterra model, which is a simple predator-prey model. I use simulated data, but the parameters are chosen to reproduce qualitatively the data used in the [Sunode documentation](https://sunode.readthedocs.io/en/latest/quickstart_pymc.html). I believe the data is from the Hudson Bay Company, but I am not sure.

* `turing_test.ipynb` is the Turing version, and also generates the simulated data used for both. 

*  `sunode_test.ipynb` is the PYMC version, which uses the sunode library to solve the ODEs as recommended by the [PYMC Developers](https://www.pymc.io/projects/docs/en/stable/api/ode.html).

* This was just a quick look, I didn't try to be careful with the timings, as I only want to compare order of magnitudes.    On my M3 Macbook Pro,  The pymc model took 2 minutes to sample 4 chains of 1000, while the turing model took 2 minutes as well, however the Turing model was doing the chains in serial not parallel as in the pymc model. (TODO: use MCMCThreads or MCMCDistributed). 

* On windows, the pymc model took 2.5 minutes per chain, that is 10 minutes total. This is because you cannot do parallel chains on windows with Sunode yet. The Turning model took 2.25 minutes for all 4 chains.  Again these are not scientific comparisons, but the Turing model is certainly not slower than the PYMC model!

* As for comparing the code, the PYMC code requires understanding PyTensor, Sunode as well as PYMC APIs, which all have their quirks. For example, currently the 'extra' parameters in the Sunode wrapper call are required to make it work. This is a known issue for some time. Although I did not need this here, you might also need SymPy when dealing with Sunode. The Julia code seems much cleaner and more straightforward. 

* Another issue I found is that dealing with PyTensor and such can be a pain. For example, in Julia, to implement a "log if positive" function, I could just define the function in teh most straightforward way. In PYMC, I had to use special PyTensor functions to make this happen. On the other hand, it might not have been needed for the PYMC version.


* In `sunode_test.ipynb` I also tested `pymc.ode` which is *much* slower (about 25 minutes *per* chain on my windows pc). However it does support multiple cores. It is still faster to run 4 chains in series using Sunode, by a factor of about 2, but the pymc.ode implementation is a bit cleaner *looking*.  