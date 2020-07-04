# SMASH hadronic transport hands-on session

Add your name to the [table](https://docs.google.com/spreadsheets/d/1f1M4vro1lFZnp80Dy0bE_XjYxMQid9oG9BgEMiO10-o/edit?usp=sharing) to mark your progress.
Then follow the steps below.

<details><summary> My personal docker cheat sheet </summary>
<p>

I'm very new with docker, so here I assemble commands that were useful for me:

```
  docker container ls -a        # List all containers
  docker system prune           # Remove all the stopped containers
  docker start -ai myJetscape   # Start JETSCAPE docker again after exiting

  # run JETSCAPE container on linux
  docker run -it -v ~/jetscape-docker:/home/jetscape-user --name myJetscape --user $(id -u):$(id -g) jetscape/base:v1.4

  # run JETSCAPE container on MAC
  docker run -it -v ~/jetscape-docker:/home/jetscape-user --name myJetscape jetscape/base:v1.4
```

</p>
</details>


<details><summary><b> 1. What is SMASH </b></summary>
<p>

SMASH is a hadronic transport code. In JETSCAPE it simulates multiple hadron-hadron scatterings in the final dilute stage of the fireball evolution.
Look at the visualization at the [official SMASH webpage](https://smash-transport.github.io/). At the end of our session we will be able
to create similar visualizations, configure SMASH and analyze its output. All this will be done without writing any code at all, or with simple one-liners.

</p>
</details>







<details><summary><b> 2. Making sure prequisites are ready </b></summary>
<p>

I assume that you have followed the [general school instructions](https://github.com/JETSCAPE/SummerSchool2020/blob/master/README.md) and have
docker and ROOT installed. Having [ROOT](https://root.cern/install/) installed on your computer (not in docker space) is really important for this tutorial.

Try to run

```
root -l
```

You should see a root command line. Try to type

```
new TBrowser
```

into this line and make sure you havd a ROOT browser opening.



#### Installing paraview
----

For creating nice SMASH visualizations we will use paraview.


<details><summary> MAC  </summary>
<p>

```
brew install paraview
```

</p>
</details>

<details><summary> Ubuntu or other linux </summary>
<p>

```
sudo apt-get install -y paraview
```

With other linux distributives you may use *yum* instead of *apt-get*.

</p>
</details>

<details><summary> Windows </summary>
<p>

[Download](https://www.paraview.org/download/) and execute the .exe installer for Windows.

</p>
</details>

If you have some fancy operating system you are probably screwed. Give it up.
If you are at the session and didn't manage to install paraview for more than 10 minutes, give it up
and proceed further. Paraview is nice to have, but not critical for us.


</p>
</details>




<details><summary><b> 3. Installing SMASH in docker environment </b></summary>
<p>

Installing SMASH:

```
cd jetscape-docker/JETSCAPE/external_packages
./get_smash.sh
cd smash/smash_code/build
make smash
```

Now let's try to run SMASH. Starting default smash run:

```
./smash
```

Print out SMASH version:

```
./smash --version
```

Prints the list of all SMASH command line options

```
./smash --help
```

</p>
</details>



<details><summary><b> 4. Configuring SMASH </b></summary>
<p>
  What SMASH is going to simulate depends on what you ask it.
  By default it simulates a Au+Au collision at 1.23 GeV per nucleon in the lab frame.
  In the end we want to use SMASH as a hadronic afterburner, so let's learn to configure it.
  You can learn how to do it by yourself from the detailed [SMASH user guide](http://theory.gsi.de/~smash/userguide/1.8/),
  but this tutorial is intended to make your life a bit simpler. So let's go
  step by step.

  SMASH is controlled in two ways:

  - By configuration file\
    By default this file is called config.yaml. Let's copy
    it to JETSCAPE_school.yaml and make smash read configuration from it:

    ```
      cp config.yaml JETSCAPE_school.yaml
      ./smash --inputfile JETSCAPE_school.yaml
    ```

  - By command-line options\
    They can overrule the options in the file. For example,

    ```
      ./smash --inputfile JETSCAPE_school.yaml --config "General: {End_Time: 40.0}"
    ```
    will change the simulation end time from the 200 fm/c in the config to 40 fm/c.

  Now let us look inside the `JETSCAPE_school.yaml`. For now let's focus
  on the Output section:

  ```
    Output:
        Output_Interval: 10.0
        Particles:
            Format:          ["Oscar2013"]
  ```

  This means that SMASH is going to print out all the particles in
  Oscar2013 format (a simple human readable text), and if it is required to
  print out particles in the middle of the simulation, it will do so every 10.0 fm/c.
  By default SMASH will print out only particles in the end of the simulation.
  To make it actually print out particles every 10 fm/c we need to supply our config with
  an additional `Only_Final: False` option.

  ```
    Output:
        Output_Interval: 10.0
        Particles:
            Format:          ["Oscar2013"]
            Only_Final:      False
  ```

  **Let's look at the results of our simulations.**
  By default SMASH output will be in the folders `data/0`, `data/1`, etc.
  Open the latest `data/?` folder and look at the files there.
  There is config.yaml there, it is just a full copy of SMASH configuration
  to keep record of what was done. And there is a `particle_lists.oscar` file. This is the one we want to look at.
  It contains the particles that SMASH generated. Open it and you should see something like this:

  ```
   #!OSCAR2013 particle_lists t x y z mass p0 px py pz pdg ID charge
   # Units: fm fm fm fm GeV GeV GeV GeV GeV none none e
   # SMASH-1.8
   # event 1 out 470
   200 -106.204 58.1653 -14.4014 0.938 1.26645138 -0.746754441 0.397353787 -0.092319454 2112 2364 0
   200 104.02 39.1754 98.0998 0.938 1.4867404 0.782602686 0.334508298 0.778582208 2212 907 1
   200 15.7665 -21.8512 -137.847 0.938 1.34280422 0.101448745 -0.118439561 -0.948134694 2212 2344 1
   ...
  ```

  You can analyse these results already using your favourite way to write scripts, but at this tutorial I want to show some
  convenient approaches to perform quick analysis without writing code.
  For this we actually want output in some different formats: Root and VTK.
  We will use Root output for analysis and VTK output for visualization. Let's add them to configuration.
  Let's also ask for as much information as possible to be written out (`Extended: True`).

  ```
    Output:
        Output_Interval: 1.0
        Particles:
            Format:          ["Oscar2013", "Root", "VTK"]
            Only_Final:      False
            Extended:        True
  ```

  This is going to generate a lot of output, so let's change the time of simulation to 40 fm/c instead of 200 fm/c:

  ```
    General:
        ...
        End_Time:    40.0   # 200.0
        ...
  ```

  Next we will look at Root and VTK outputs.

</p>
</details>


<details><summary><b> 5. Easy SMASH results visualization: looking at VTK output with paraview </b></summary>
<p>

  If you didn't manage to install paraview, skip this section. It's pretty and fun, but not critical for us.

  Let us open the vtk files that we generated in previous section. For this start `paraview`, press `File -> Open`
  and open our vtk files. Remember, that by default SMASH output will be in the folders `data/0`, `data/1`, etc.
  Open the latest `data/?` folder and look at the files there.

  Press a large green
  ```
    Apply
  ```
  button and you should be able to see some small dots on the display. Those are our particles.
  Let's make them look bigger. Change:

  ```
    Representation: Surface -> 3D Glyphs
    Glyph Type: Arrow -> Sphere
  ```

  Now use the green `Next Frame` and `Previous Frame` buttons on the top to play the movie.

  ### Challenge 1

  Experiment with paraview capabilities. You can change the color of spheres depending on their momenta,
  particle type, etc. You can add arrows to particles to show their momenta.

  Try to visualize a particles in a box simulation instead of collider. To run a box simulation
  change

  ```
    General:
        Modus:  Box  # previously it was Collider
  ```

  and set up the box configuration you like, see [the documentation](http://theory.gsi.de/~smash/userguide/1.8/input_modi_box_.html).
  I don't reveal all the details, you have to find them yourselves. That's why it's called *challenge*.

</p>
</details>

<details><summary><b> 6. Scriptless quick analysis with ROOT output </b></summary>
<p>

Run ROOT on your computer (not in docker enveroment).

```
  root -l
```

If it doesn't run because you forgot to install it, then you are in trouble.
Go to the official [Root installation guide](https://root.cern/install/) and try your luck. But many school
participants are experimentalists, so you guys should have ROOT on your computers and be more fluent with it than I am.

In root environment type

```
  new TBrowser
```

This should open a browser. Use it to open the Root file `Particles.root` you generated previously from SMASH simulation.
In the left panel of the browser you should see a tree called `particles`. Double-click on it and you will see many
leaves. Double-click on a leaf shows a histogram. In this way you can see a distribution of x, y, z coordinates,
times of output, particle energies p0, and momenta px, py, pz.

Let's do something more practical. Let's generate 50 events (set this yourself in the configuration)
and compare pion to proton rapidity distributions. Open the newly generated `Particles.root` in your TBrowser.
In the `Command(local)` panel enter:

```
  particles->Draw("0.5 * log((p0-pz)/(p0+pz))","pdgcode == 2212", "E");
```

Now left-click on the histogram to update it. Here
 - `particles` is the name of the tree
 - `0.5 * log((p0-pz)/(p0+pz))` is the first parameter of the [Draw](https://root.cern.ch/root/html524/TTree.html#TTree:Draw) function.\
    It is a variable to be histogrammed. As you see, ROOT allows to put formulas there,
    which use leaves like `p0` and `pz`.
 - `pdgcode == 2212` is the second parameter of the [Draw](https://root.cern.ch/root/html524/TTree.html#TTree:Draw) function. It defines
   a cut. Here we cut on particle type, `2212` is a [PDG code](http://pdg.lbl.gov/2019/reviews/rpp2019-rev-monte-carlo-numbering.pdf) of protons.
   It is possible to combine cuts, for example `pdgcode == 2212 && sqrt(px*px + py*py) > 0.2 && t == 200.0`.
 - `E` is the third parameter of [Draw](https://root.cern.ch/root/html524/TTree.html#TTree:Draw). It is a plotting option that
   asks ROOT to show error bars.

Let's now plot a rapidity distribution for pions (plotting option `same` puts this histogram above the previous one):

```
  particles->Draw("0.5 * log((p0-pz)/(p0+pz))","pdgcode == 211 || pdgcode == 111 || pdgcode == -211", "E same");
```

Now both proton and pion histograms have the same color and you can't distinguish them. Right-click on the points and change the color:

```
  Right-click -> SetLineAttributes
```

Now you have an ugly, but very quick and functional way to analyze SMASH output. Let's look at particles in spatial coordinates.
You cannot do it in experiment, but it is easy in SMASH:

```
  particles->Draw("x:y:z","pdgcode == 2212");

  particles->Draw("x:y","t==20");
  particles->Draw("x:y","t==30");
  particles->Draw("x:y","t==40");
```


</p>
</details>


<details><summary><b> 7. Observe chemical and kinetic freeze-out </b></summary>
<p>
</p>
</details>
