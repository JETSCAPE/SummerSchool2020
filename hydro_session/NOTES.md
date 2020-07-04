# JETSCAPE Hydrodynamics Session

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

### Averaged temperature and flow velocity evoltion


### Animation of energy density evolution


## Produce hadrons from hydrodynamics

In JETSCAPE, a third party particle sampler iSpectraSampler (iSS) is employed
to convert fluid cells to particles. The iSS produces Monte-Carlo particles
from the hydrodynamic hyper-surface. The spatial and momentum distributions
of particles follow the Cooper-Frye Formula.

