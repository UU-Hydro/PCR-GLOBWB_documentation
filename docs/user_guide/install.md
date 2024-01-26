# How to install

PCR-GLOBWB is developed in Python and uses various supporting packages (e.g. pcraster, numpy and netcdf4). Therefore, beside the PCR-GLOBWB model code, you will need a working Python package environment to install the PCR-GLOBWB model. Here we provide a short guide to installing PCR-GLOBWB. *Note that in this installation guide we assume you work on a Linux operating system*.

## Python package environment

Please follow the following steps to setup a PCR-GLOBWB environment:

1. To create a Python package environment, we recommend to install Miniconda, particularly for Python 3. Follow the Miniconda install instructions given [here](https://docs.conda.io/en/latest/miniconda.html). A user guide and short reference on the conda package manager can be found [here](https://docs.conda.io/projects/conda/en/latest/user-guide/cheatsheet.html).

2. Now that Miniconda is installed, you can use it to make a package environment. To install the correct packages and their versions, we have created an environment file on [our GitHub repository](https://github.com/UU-Hydro/PCR-GLOBWB_model/blob/master/conda_env/pcrglobwb_py3.yml). Use the environment file to install all required packages to a conda environment:

    `conda env create --name pcrglobwb_python3 --file pcrglobwb_py3.yml`

    This will create a environment named *pcrglobwb_python3*.

3. To activate the PCR-GLOBWB environment:

    `conda activate pcrglobwb_python3`

    This will set the current environment to the *pcrglobwb_python3* environment

## PCR-GLOBWB model code

Please follow the following steps to get the PCR-GLOBWB model code:

1. To save the model code, we recommend to install Git. Follow the Git install instructions [here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git). A user guide and short reference on the git version control system can be found [here](https://training.github.com/downloads/github-git-cheat-sheet/).

2. Now that Git is installed, you can use it to clone and track the PCR-GLOBWB model code from the the main branch of [our GitHub repository](https://github.com/UU-Hydro/PCR-GLOBWB_model):

    `git clone https://github.com/UU-Hydro/PCR-GLOBWB_model.git`

    This will clone PCR-GLOBWB, as well as the models version history, into the current working directory.

3. To run the PCR-GLOBWB model please checkout the [configure section](../configure) and [run section](../run) of this guide.

## In short

```bash
conda env create --name pcrglobwb_python3 --file pcrglobwb_py3.yml
conda activate pcrglobwb_python3
git clone https://github.com/UU-Hydro/PCR-GLOBWB_model.git
```
