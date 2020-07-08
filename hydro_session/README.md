# JETSCAPE Hydrodynamics Session

## Goals

- Understand how to run JETSCAPE with a few default Fluid dynamics modules
and set/change their relevant parameters, such as transport coefficients

- Using a realistic hydrodynamic module, students will be able to output
hydrodynamic evolution profile and analyze the evolution of temperature
and the development of flow velocity with various settings

-Simulate event-by-event bulk dynamics for Au+Au @ 200 GeV and
Pb+Pb @ 5020 GeV with a realistic hydrodynamics code, such as MUSIC.


## Setup a docker container

In this session, we need to launch a docker container that supports jupyter
notebook. Please use the following command:

**macOS:**
```
docker run -it --rm -p 8888:8888 -v ~/jetscape-docker:/home/jetscape-user --name myJetscape jetscape/base:v1.4
```

**linux:**
```
docker run -it --rm -p 8888:8888 -v ~/jetscape-docker:/home/jetscape-user --name myJetscape --user $(id -u):$(id -g) jetscape/base:v1.4
```

- `--rm` This option will delete the current docker container at exit.
(If you want to keep the container, you can omit this option.)
- `-p 8888:8888` This option creates a port for your web browser outside
the docker container to load a jupyter notebook.
All the python packages are in the docker container.

Under Linux, if you encounter an error about `permission denied`,
you can use `sudo` in front of the docker run command.

## Build and run JETSCAPE with MUSIC and iSS

We will do all of our exercises in the `JETSCAPE/build` directory:

When you enter the docker container, type the following commands to go to the
correct working directory,

```
cd JETSCAPE
cd build
cmake .. -DUSE_MUSIC=ON -DUSE_ISS=ON
make
```

Copy the hand-on materials of this session to the build folder,

```
cp -r ../../SummerSchool/hydro_session .
```

Then, run JETSCAPE with the example configuration file,

```
./runJetscape hydro_session/jetscape_user_MUSICrun.xml
```

## Study the hydrodynamic evolution

### Physics Background

The JETSCAPE framework employs the Trento model to generate event-by-event
initial energy density profile. The energy density profile is then passed to
the hydrodynamics module (MUSIC), which will evolve the collision system from
a hot QGP phase to hadron gas phase. Optionally, pre-equilibrium dynamics
modeled by free-streaming can be included between the Trento and hydrodynamics.
In the dilute hadronic phase, fluid cells will convert to individual
hadrons. This process is denoted as the particlization. The JETSCAPE framework
uses iSS to perform particlization. The produced hadrons can be
fed to a hadronic transport model (SMASH) which accounts for scattering
processes among hadrons and decays of excited resonance states.


### Play with model parameters

- Types of collision systems
    * Collision Energy
    * Colliding Nuclei
    * Centrality

In the user configuration file `hydro_session/jetscape_user_MUSICrun.xml`,
one can modify model parameters for the initial state module to simulate
his/her desired collision system. In between `<Trento>` and `<\Trento>`,
we can specify the type of colliding nucleus, collision energy, centrality,
and longitudinal fluctuations.

Initial state module, between `<IS>` and `</IS>` also define the 3D grid
that we would like to simulate hydrodynamic evolution. If one set
`grid_max_z` to 0, the JETSCAPE framework will perform a (2+1) hydrodynamic
simulations assuming longitudinal boost-invariant.

Finally, the user can specify a random seed for the entire simulation.
This is specified inside the block `<Random>` in the xml file. If the
`<seed>` parameter is set to 0, then random seed will be determined by
the system time. If `<seed>` is set to any positive number, the JETSCAPE
will perform simulation with the given positive number is the random
seed for all its modules. A fix seed simulation will be handy when we
study the effect of viscosity during the hydrodynamic evolution.


- QGP viscosity
    * Specific shear viscosity
    * Specific bulk viscosity

Using a realistic hydrodynamic module inside the JETSCAPE, the users have
freedom to change a few physical parameters for the hydrodynamic simulations.
The most interesting ones are the specific shear and bulk viscosity.
All the relevant parameters are under the block `<hydro>` `<MUSIC>`.
The user can set the a constant specific shear viscosity by changing the
value for the parameter `<shear_viscosity_eta_over_s>`. A physical $\eta/s$
needs to be a positive value. We recommend the users to try any real value
between 0 and 0.3 in the exercies.
One can further try a temperature dependent $(\eta/s)(T)$ by setting
the variable `<T_dependent_Shear_to_S_ratio>` to **3**. Once this parameter
is set to 3, the previous parameter `<shear_viscosity_eta_over_s>` will be
ineffective. The users needs to further specify the following four parameters

    1. <eta_over_s_T_kink_in_GeV>
    2. <eta_over_s_low_T_slope_in_GeV>
    3. <eta_over_s_high_T_slope_in_GeV>
    4. <eta_over_s_at_kink>`

to characterize the temperature dependence of $(\eta/s)(T)$.

Moreover, the users can include a non-zero bulk viscosity in the hydrodynamic
simulations. Because the QCD bulk viscosity is related to the breaking of
conformal symmetry, we expect the specific bulk viscosity to depend on
temperature, $(\zeta/s)(T)$. Siimilar to the case for $(\eta/s)(T)$, the
uses can set the parameter `<temperature_dependent_bulk_viscosity>` to **3**
in the xml file to include a temperature dependent $(\zeta/s)(T)$. Setting
`<temperature_dependent_bulk_viscosity>` to **0** will set $\zeta/s = 0$.
With `<temperature_dependent_bulk_viscosity>` set to 3, the users needs to
further provide four additional parameters to characterize the shape of
$(\zeta/s)(T)$. They are as follows,

    1. <zeta_over_s_max>
    2. <zeta_over_s_T_peak_in_GeV>
    3. <zeta_over_s_width_in_GeV>
    4. <zeta_over_s_lambda_asymm>

In addition to specific shear and bulk viscosity, the users have freedom to
change the starting time of hydrodynamics, `<Initial_time_tau_0>` (0.2-1.0 fm)
, whether to include second order transport coefficients,
`<Include_second_order_terms>` (0 or 1), and particlization temperature,
`<freezeout_temperature>` (0.13 to 0.16 GeV)

After each run, please create a result folder with the viscosity information
and move the following three files into the result folder,

    * eccentricities_evo_eta_-0.5_0.5.dat
    This file records the evolution of spacial eccentricity of the fireball
    * momentum_anisotropy_eta_-0.5_0.5.dat
    This file has the evolution information about the momentum anisotropy,
    average velocity, and average temperature.
    * evolution_for_movie_xyeta_MUSIC.dat
    This file contains evolution history of fluid cells above T = 130 MeV.

### Visualization with Jupyter Notebook

Launch jupyter notebook inside the docker contain with the following command,

```
jupyter notebook --ip 0.0.0.0 --no-browser > notebook.log 2>&1 &
```

Once the jupyter notebook is running in the background, the user can open
the notebook.log with `cat notebook.log`, then the user can click on the
link with holding the `ctrl` key. After the click, your web browser should
be launched in the current directory.


### Averaged temperature and flow velocity evoltion

In your browser, we first go into the `hydro_session` folder. To make
comparison, we open the notebook `hydro_evo.ipynb` by click on it inside
the browser. Once the notebook is opened, the user can execute every cell
in this notebook one-by-one. Press `shift+enter` to execute the cell block
in in the notebook. Please note to change the result folder name to the ones
you simulate on your computer and change the label text accrodingly.


### Animation of energy density evolution

Similar to the previous exercies, this time we will open the second notebook
under `hydro_session` folder, `hydro_movie.ipynb`. After it is opened inside
your browser, you need to modify the path to where you store the hydrodynamic
evolution file `evolution_for_movie_xyeta_MUSIC.dat`. Then you can execute
the code cells one by one to generate 2D color contour plots as well as
animation for the temperature and fluid velocity evolution.


## Produce hadrons from hydrodynamics [Bonus]

In JETSCAPE, a third party particle sampler iSpectraSampler (iSS) is employed
to convert fluid cells to particles. The iSS produces Monte-Carlo particles
from the hydrodynamic hyper-surface. The spatial and momentum distributions
of particles follow the Cooper-Frye Formula.

The sampled hadrons are output in the `test_out.dat`. One can run another
code script `FinalStateHadrons` to extract the final state hadron list from
the `test_out.dat` file. The users need to type in the following command at
the build folder,

```
./FinalStateHadrons test_out.dat hadron_list.dat
```

With the produced `hadron_list.dat` file, the users can apply their own
analysis script to compute particle spectra, mean $p_T$, and
anisotropic flow coefficients $v_n$. An example of the analysis code is
`analyze_particle_spectra_and_vn.ipynb`. 


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

To accumlate statistic, you can set `<nEvents>` to 50 and
`<nReuseHydro>` to 50 in the xml file to avoid running 50 hydrodynamic
simulations. With the generated `test_out.dat` file, apply `FinalStateHardon`
and analysis the output to get $p_T$-spectra for charged hadrons and their
flow anisotropy coefficients.
