# Comparison of Julia's Turing and PYMC for ODE inference

* Goal is to compare the same ODE parameter inference problem implemented in TuringLang and in PYMC (using sunode) 

* For the Julia part, don't forget to `activate turingTest` and `instantiate` (https://pkgdocs.julialang.org/v1/environments/)

* For now I have compared two approaches with a more direct parameterization of the LV model. The PYMC model took 2.5 minutes to sample 4 chains of 1000, while the turing model took 1m20s, however the turing mode was doing the chains in serial not parallel as in the pymc mode.

* Another issue i found is that dealing with pytensor and such can be a pain. For example, in Julia, to implement a "log if positive" function, I could just define the function in teh most straightforward way. In PYMC, I had to use special PyTensor functions to make this happen. On the other hand, it might not have been for the PYMC version.