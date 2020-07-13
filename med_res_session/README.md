# Medium Excitation by Jets [Hands-on Session]

## Materials

Please make sure you have the following folders in `~/jetscape-docker`,

* JETSCAPE
* SummerSchool2020/med\_res_session

## Start a docker container

Start the JETSCAPE docker container,

**macOS:**
```
docker run -it -p 8888:8888 -v ~/jetscape-docker:/home/jetscape-user --name myJSMedResSession jetscape/base:v1.4
```

**Linux:**
```
docker run -it -p 8888:8888 -v ~/jetscape-docker:/home/jetscape-user --name myJSMedResSession --user $(id -u):$(id -g) jetscape/base:v1.4
```

The option `-p 8888:8888` is necessary to creates a port to access the jupyter notebook, which we use in this hands-on session, from your local web browser.


## Build JETSCAPE with LBT-tables, MUSIC and iSS

Please make sure all the external code packages (LBT-tables, MUSIC and iSS) have been
downloaded. You can check this by the following commands,

```
cd ~/JETSCAPE/external_packages
ls
```

Please check the folder `LBT-tables`, `music` and `iSS` are present.
If not, please get them with the following commands,

```
./get_lbtTab.sh
./get_music.sh
./get_iSS.sh
```

Setup and build JETSCAPE from inside the docker container:

```
cd ~/JETSCAPE
mkdir -p build
cd build
cmake .. -DUSE_MUSIC=ON -DUSE_ISS=ON
make -j4
cp -r ../../SummerSchool2020/med_res_session .
```

The materials used in this session are copied to the working folder by the last command.

## Run Twostage-hydro of JETSCAPE

To perform a simulation with hydrodynamic medium response (in `~/JETSCAPE/build`), 

```
cd ~/JETSCAPE/build
./runJetscape med_res_session/jetscape_user_twostagehydro_summer_school_2020.xml
```


## Visualization with Jupyter Notebook

Launch jupyter notebook inside the docker contain with the following command, 

```
jupyter-notebook --ip 0.0.0.0 --no-browser > notebook.log 2>&1 &
cat notebook.log
```
Open the displayed　address `http://127.0.0.1:8888/?token=...` in your browser. 
Then please go to the folder `med_res_session`, open `hydro_movie-medium_response.ipynb`, and follow the instructions. 



