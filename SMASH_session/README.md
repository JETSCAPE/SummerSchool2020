# SMASH hadronic transport hands-on session

Add your name to the [table](https://docs.google.com/spreadsheets/d/1f1M4vro1lFZnp80Dy0bE_XjYxMQid9oG9BgEMiO10-o/edit#gid=0) to mark your progress.
Then follow the steps below.

<details><summary>## 1. What is SMASH </summary>
<p>

SMASH is a hadronic transport code. In JETSCAPE it simulates multiple hadron-hadron scatterings in the final dilute stage of the fireball evolution.
Look at the visualization at the [official SMASH webpage](https://smash-transport.github.io/). At the end of our session we will be able
to create similar visualizations, configure SMASH and analyze its output. All this will be done without writing any code at all, or with simple one-liners.

</p>
</details>







<details><summary>## 2. Making sure prequisites are ready </summary>
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



For creating nice SMASH visualizations we will use paraview.

#### Installing paraview

<details><summary> MAC </summary>
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

With other linux distributives you may use *yum* instead of *apt_get*.

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




<details><summary>## 3. Installing SMASH in docker environment </summary>
<p>

Installing SMASH:

```
cd jetscape-docker/JETSCAPE/external_packages
./get_smash.sh
cd smash/smash_code/build
make -j4 smash
```

Try to run SMASH:


```
./smash --version
```
Prints smash version

```
./smash --help
```
Prints list of SMASH command line options

```
./smash
```
Starts a default SMASH run

</p>
</details>

