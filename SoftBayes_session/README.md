# TRENTo/Bayesian Analysis for the [JETSCAPE Online Summer School 2020](https://indico.bnl.gov/event/8660/).

## Prerequisite and Installation

These examples run with `python3` on a `Jupyter Notebook`. It *does not* require docker.
We provided two options to either install the dependencies using [Anaconda](https://www.anaconda.com/) (recommended) or simply run the code from an online session supported by [Binder](https://mybinder.org/).

### Option 1. Conda Virtual Environment (Recommended)

1. Download and install `Conda`

    1.1. Download `Miniconda` (a free minimal version of Anaconda) for Python 3.7 from [here](https://docs.conda.io/en/latest/miniconda.html). Remember to choose `Python3` (3.7) and the corrected installer for your operating system and architecture (for my system which is Linux x86_64, the installer that I downloadeded is `Miniconda3-latest-Linux-x86_64.sh`). 

    1.2. Execute the downloaded installer (for example on Linux, `sudo bash <name-of-the-installation-script>`), and follow the instructions to install.
It will ask you where to put conda (Default is `$HOME/` by press `Enter`). After the installation, `conda` command can be called in shell (for Mac / Linux). For Windows, you can find `anaconda prompt` from your start menu to open a shell environment.

2. Clone the repository and download data files

    2.1. If you already download the code from `git pull` in the JETSCAPE summer school repo, you can skip this step. Clone the current repository to a directory, for example `$HOME/JS2020`.
    ```
    mkdir $HOME/JS2020 && cd $HOME/JS2020 git clone https://github.com/keweiyao/JETSCAPE2020-TRENTO-BAYES.git 
    ```

    2.2. Using the script `postBuild` to down the data. Note that this script uses `wget` to download the data file. (On Mac, you may need to do `brew install wget` to intall `wget`, or you can simply go to [here](https://webhome.phy.duke.edu/~wk42/jetscape/data.tar.gz) to download the `data.tar.gz` and unzip to the working directory)
    ```
    cd <path-to-code-folder>
    bash postBuild 
    ```
    Now `ls` and you should see the downloaded folder `ModelData/`.

3. Create conda virtual environment and test the notebook

    3.1. Create conda virtual environment.
    ```
    conda env create -f environment.yml 
    ```
    This should create a virtual environment called `trento-bayes`. You can check if it exists and its location by `conda env list`. The required libraries are installed in this environment (these libraries are listed in `environment.yml`, except for `emcee` package, which is listed in the `pip` `requirements.txt`). If you want to delete this environment after the lecture, you can do `conda env remove -n <name-of-environment>`.

    3.2. To activate the environment 
    ```
    conda activate trento-bayes 
    ```
    To deactivate the environment 
    ```
    conda deactivate 
    ```

    3.3. Within the folder that contains the examples, open jupyter notebook, 
    ```
    jupyter notebook 
    ```
    From the prompt of the browser, click and open the file `trento-bayes.ipynb`

    3.4. Move to the first block, press `Shift+Enter` to see if you can run the first two or three blocks successfully. 
Then you are good to go!

### Option 2. Use Online Binder Environment

This Binder environment holds online session of interactive `Jupyter notebook`. 
Simple check the following link and wait for the environment to build and load.
[Binder link](https://mybinder.org/v2/gh/keweiyao/JETSCAPE2020-TRENTO-BAYES/master ).
The advantage is that you don't need to install the dependence on your computer. 
The disadvantages: 
1. The initial build of the image make take ~ 5 mins.
2. Require fast and stable internet connection. Once you lost connection, you will need to restart.
3. The computational power of the binder server is limited. So for some examples with significant amount of computation, the computing time can be too long for the lecture. Therefore, I also uploaded pre-calculated results for these examples that you can simply load and study.

## Materials from Previous JETSCAPE Schools.

If you have time, you are encouraged to checkout materials from eariler JETSCAPE schools.

1. JETSCAPE Winter School & Workshop 2018.
[Lecture-1](https://indico.bnl.gov/event/3958/contributions/12067/attachments/10945/13352/WS_Statistics_Theory.pdf) and 
[Lecture-2](https://indico.bnl.gov/event/3958/contributions/12064/attachments/10943/13350/WS_Exercise_Pres.pdf).
[Hands-on](https://sites.google.com/a/lbl.gov/jetscape2018/home/school-material/school-preparation).

2. 2nd JETSCAPE Winter School and Workshop 2019
[Lecture](https://indico.bnl.gov/event/5031/contributions/25828/attachments/21430/29220/WS_Theory_Exercises.pdf).
[Hands-on](https://indico.bnl.gov/event/5031/page/115-school-material) under section `School preparation - Statistical Analysis`.
