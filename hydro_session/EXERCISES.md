# HOMEWORK

## 1. Study the viscous effects on hydrodynamic evolution

Simulate a Pb+Pb collision in 20-30% centrality at 5.02 TeV. In order to
compare simulations with different viscosity, we need to fixed the
random seed. You can choose your favorate number for the seed. (42?)

* Compare the averaged temperature evolution with two choices of
the specific bulk viscosity $\zeta/s(T)$

* Compare the development of averaged radial with and without the specific
bulk viscosity

* Compare the development of flow anisotropy as a function of proper time
with two different shear viscosities


## 2. Produce animation for the temperature and flow velocity profiles in the transverse plane

* Pick your favorate collision system (colliding nuclei, collision energy,
and centrality) and generage hydrodynamic evolution file.

You can try different color maps and even define your own to make the
animation pretty.

Please send your best animation to chunshen@wayne.edu.
We will select the most impressive ones and post them on the school website.


## 3. [Bonus] Compute particle spectra and flow anisotropic flow Qn vectors from the event-by-event simulations for one heavy-ion collison system.

To accumlate statistic, you can set `<nEvents>` to 5000 and
`<nReuseHydro>` to 5000 in the xml file to avoid running 5000 hydrodynamic
simulations. With the generated `test_out.dat` file, apply `FinalStateHardon`
and analysis the output to get $p_T$-spectra for charged hadrons and their
flow anisotropy coefficients.
