# JETSCAPE Hydrodynamics Session

## Setup docker container

In this session, we need to launch a docker container that supports jupyter notebook. Please use the following command:

**macOS:**
```
docker run -it --rm -p 8888:8888 -v ~/jetscape-docker:/home/jetscape-user --name myJetscape jetscape/base:v1.4
```

**linux:**
```
docker run -it --rm -p 8888:8888 -v ~/jetscape-docker:/home/jetscape-user --name myJetscape --user $(id -u):$(id -g) jetscape/base:v1.4
```

- `--rm` will delete the current docker container at exit
- `-p 8888:8888` creates a port for the web browser to load jupyter notebook inside the docker container
Under Linux, if an error "permission denied" is encountered, one can use "sudo" in front of the docker run command.

## Run hydrodynamic simulations from JETSCAPE

We start our exercise in the `build` directory:
```
cd JETSCAPE
cd build
```
Copy the example 
