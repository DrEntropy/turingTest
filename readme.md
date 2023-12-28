# Comparison of Julia's Turing and PYMC for ODE inference

* Goal is to compare the same ODE parameter inference problem implemented in TuringLang and in PYMC (using sunode) 

* For the Julia part, don't forget to `activate turingTest` and `instantiate` (https://pkgdocs.julialang.org/v1/environments/)

* For now I have compared two approaches with simulated data for the LV model. This was just a quick look, I didn't try to carefully time things.  On my M3 Macbook Pro,  The PYMC model took 2 minutes to sample 4 chains of 1000, while the turing model took 2 minutes as well, however the Turing model was doing the chains in serial not parallel as in the pymc mode. (TODO: use MCMCThreads or MCMCDistributed). On windows, the pymc model took 2.5 minutes per chain (you cannot do parallel chains on windows with sunode yet) and the Turning model took 2.25 minutes for all 4 chains.  Again these are not scientific comparisons, but the Turing model is certainly not slower than the PYMC model!

* As for comparing the code, the PYMC code requires understanding PyTensor, Sunode as well as PYMC APIs, which all have their quirks. For example, currently the 'extra' parameters in the Sunode wrapper call are required to make it work. This is a known issue for some time.  The Julia code seems much cleaner and straightforward. Its all Julia.

* Another issue I found is that dealing with pytensor and such can be a pain. For example, in Julia, to implement a "log if positive" function, I could just define the function in teh most straightforward way. In PYMC, I had to use special PyTensor functions to make this happen. On the other hand, it might not have been for the PYMC version.