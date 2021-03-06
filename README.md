# IUCN Extinction Simulator

A program for estimating extinction probabilities and dates for a given set of species, based on IUCN threat assessments.

[![downloads](https://anaconda.org/bioconda/iucn_sim/badges/downloads.svg)](https://anaconda.org/bioconda/iucn_sim)
[![updated](https://anaconda.org/bioconda/iucn_sim/badges/latest_release_date.svg)](https://anaconda.org/bioconda/iucn_sim)
[![conda](https://anaconda.org/bioconda/iucn_sim/badges/installer/conda.svg)](https://anaconda.org/bioconda/iucn_sim)
[![conda](https://anaconda.org/bioconda/iucn_sim/badges/license.svg)](https://anaconda.org/bioconda/iucn_sim) 


## Installation

`iucn_sim` is available as a conda package, which helps installing all required Python and R dependencies, without you having to worry about taking care of this yourself.
The conda package manager creates an extremely light-weight virtual environment that enables anybody to run `iucn_sim` on their computer, independently of the operating system and independently of any previous installations of Python and R.

1. Download [miniconda](https://docs.conda.io/en/latest/miniconda.html) for your operating system.

2. Once miniconda is installed, open a command line terminal (e.g. `Terminal` on macOS). Windows users will need to open the freshly installed **Anaconda Powershell Prompt** instead of the regular Command Prompt for this purpose.

3. Add the conda-forge and bioconda channels to conda, where some of the needed packages are hosted:
	- `conda config --add channels conda-forge`
	- `conda config --add channels bioconda`

4. Install `iucn_sim` and all it's dependencies by typing the following into the command line, but see the tip below before you do so!
	- `conda install iucn_sim`

	> Tip: It is recommendable to create your own environment for `iucn_sim`. This will ensure that downloading the required R or Python version for `iucn_sim` will not affect any existing setup on your computer. In order to install `iucn_sim` into a new environment, simply type `conda create -n iucn_sim_env iucn_sim`.
	Now, everytime you want to use the software, you first need to connect to the environment you just created, by typing `conda activate iucn_sim_env`. Now you can use `iucn_sim`, while connected to your specially created environment. Disconnect from the environment by typing `conda deactivate` while connected.

5. Test if installation worked by typing `iucn_sim -h`. This should show an overview of the available arguments of `iucn_sim`.

6. If step 4 caused an error something went wrong along the way. If you are a Linux or Mac user, you can instead install `iucn_sim` by [downloading this GitHub repo](https://github.com/tobiashofmann88/iucn_extinction_simulator/archive/master.zip) and building the software by typing `python setup.py install`. You will need to make sure yourself that Python3 and R are installed, including the R package `rredlist`.

## Running `iucn_sim`

Once installed, `iucn_sim` will be installed in your standard path, so you can simply type `iucn_sim` in your command line (use **Anaconda Powershell Prompt** if you are a Windows user) to call the program.

- `iucn_sim -h` --> Open help page showing available functions

- `iucn_sim get_rates -h` --> Open help page for specific function

The -h command will show and explain all available flags for each specific `iucn_sim` function. An example command could look like this:


#### Estimate status transition rates and extinction probabilities for all taxa:

`iucn_sim get_rates --input_data data/example_data/gl_data_carnivora.txt --reference_group Mammalia --reference_rank class --outdir data/example_data/carnivora_output/transition_rates/`

#### Run simulations for future

`iucn_sim run_sim --indir data/example_data/carnivora_output/transition_rates/ --outdir data/example_data/carnivora_output/future_simulations/ --n_years 100`

See below for further explanation of the required input.

## Required input data:

For running `iucn_sim` you will need to provide the following input:

- Species list, preferably containing **generation length (GL)** data for all taxa (scaled in years and tab-separated). Can contain multiple GL values per species, representing indpendent samples from the uncertainty surrounding the GL estimate. See example at `data/example_data/gl_data_all_mammals.txt`.
- Setting a reference group: Name of a taxon group (e.g. `'Mammalia'`) or alternatively path to text file containing list of species, to be used for estimating rates of change between different IUCN categories (should be sufficiently large group, we recommend > 1,000 species).


## Apply for IUCN API token

To use the full functionality of `iucn_sim` you will have to apply for an [IUCN API token](https://apiv3.iucnredlist.org/api/v3/token). This key is necessary to download data from IUCN, which is done internally in `iucn_sim`. It is easy o apply for an API key, just [follow this link](https://apiv3.iucnredlist.org/api/v3/token), it will then take a couple of days before you receive your API key. Once you have received your IUCN token, provide it when using the `get_rates` function with the `--iucn_key` flag.

However, if for some reason you have problems obtaining an IUCN token or don't want to wait until receiving it, you can run the `get_rates` function without donwloading data from IUCN. In that case you will need to provide an extra file containing the current IUCN status of all your input species (`--status_list`, but see **Note** below) and you will further need to choose a reference group (`--reference_group`) for which the IUCN history is already pre-compiled ([see available precompiled groups here](https://github.com/tobiashofmann88/iucn_extinction_simulator/tree/master/data/precompiled/iucn_history)).

**Note:** If all species from the `--input_data` file are present in one of the pre-compiled IUCN history files in the [GitHub repo](https://github.com/tobiashofmann88/iucn_extinction_simulator/tree/master/data/precompiled/iucn_history), it is **not necessary** to provide an additional file containing the current IUCN status of all input species (`--status_list`). E.g. in the code example above for running the `get_rates` function, it is technically not necessary to provide an IUCN key, since all species in the input Carnivora file are present in the precompiled IUCN history file for Mammalia. However, due to the ever-changing taxonomy it is not a given that all your species are contained in the precompiled file, even if you have a list of mammal names and use the precompiled Mammalia reference group. Long story short: Make your life easier and [apply for an IUCN token](https://apiv3.iucnredlist.org/api/v3/token)!

## Other user settings:

- `--n_reps` Number of simulation replicates, e.g. `100` will lead to 100 independent simulation replicates. If `N` different  GL values are provided per species, the simulator will by default run `N` simulation replicates, using a different GL value for each replicate.
- `--n_years` How many years to simulate into the future, e.g. `80` if the next 80 years (starting at present) should be simulated.



