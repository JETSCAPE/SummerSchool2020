# JETSCAPE Hydrodynamics Session

## Goals

- Understand how to run JETSCAPE with a few default Fluid dynamics modules
and set/change their relevant parameters, such as transport coefficients

- Using a realistic hydrodynamic module, students will be able to output
hydrodynamic evolution profile and analyze the evolution of temperature
and the development of flow velocity with various settings

-Simulate event-by-event bulk dynamics for Au+Au @ 200 GeV and
Pb+Pb @ 5020 GeV with a realistic hydrodynamics code, such as MUSIC.


## Setup docker container

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

- `--rm` will delete the current docker container at exit
- `-p 8888:8888` creates a port for the web browser to load jupyter notebook
inside the docker container

Under Linux, if an error `permission denied` is encountered,
one can use `sudo` in front of the docker run command.

## Build JETSCAPE with MUSIC, iSS, and SMASH

We start our exercise in the `build` directory:
```
cd JETSCAPE
cd build
cmake .. -DUSE_MUSIC=ON -DUSE_ISS=ON -DUSE_SMASH=ON
make
```
Copy the example input file
`/SummerSchool/hydro_session/jetscape_user_MUSICrun.xml` to the build folder

run JETSCAPE with
```
./runJetscape jetscape_user_MUSICrun.xml
```

## Study hydrodynamic evolution

### Physics Background

The JETSCAPE framework employs the Trento model to generate event-by-event
initial energy density profile. The energy density profile is passed to the
hydrodynamics module (MUSIC), which will evolve the collision system from a
hot QGP phase to hadron gas phase. Optionally, pre-equilibrium dynamics
modeled by free-streaming can be included between the Trento and hydrodynamics.
In the dilute hadronic phase, fluid cells will convert to individual
hadrons, which is denoted as the particlization. The produced hadrons can be
feed to a hadronic transport model which accounts for scattering processes
among hadrons and decays of excited resonance states.


### Play with Parameters

- Type of collision system
    * Collision Energy
    * Colliding Nuclei
    * Centrality
- QGP viscosity
    * Specific shear viscosity
    * Specific bulk viscosity


### Visualization with Jupyter Notebook

Launch jupyter notebook inside the docker contain with the following command,
```
jupyter notebook --ip 0.0.0.0 --no-browser > notebook.log 2>&1 &
```

### Averaged temperature and flow velocity evoltion


### Animation of energy density evolution



## Produce hadrons from hydrodynamics

In JETSCAPE, a third party particle sampler iSpectraSampler (iSS) is employed
to convert fluid cells to particles. The iSS produces Monte-Carlo particles
from the hydrodynamic hyper-surface. The spatial and momentum distributions
of particles follow the Cooper-Frye Formula.

