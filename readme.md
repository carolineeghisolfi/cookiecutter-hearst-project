# Hearst Python Cookiecutter

This is a project template powered by [Cookiecutter](https://github.com/cookiecutter/cookiecutter) for use with [datakit-project](https://github.com/associatedpress/datakit-project/).

**Structure**

```
.
├── README.md
├── code
│   └── analysis
│   └── etl
│   └── scratch
├── data
│   ├── source
│   ├── processed
│   ├── public
│   └── archive
├── docs
└── viz
```

- `README.md`
  - Project-specific readme with boilerplate for data projects.
- `code`
  - `code/analysis`: This is where we keep all of our jupyter ipython notebooks that contain analysis for the project.
    - Notebooks in this folder can ingest data from either `data/source` (if that data comes from the source in a workable format) or `data/processed` (if the data required some prep).
    - Dataframes from analysis notebooks are written out to `data/processed`
  - `code/etl`
    - This is where we keep python scripts involved with collecting data and prepping it for analysis.
    - These files should be scripts, they should not be jupyter notebooks.
  - `scratch`: This directory contains output that will not be used in the project in its final form.
  - Note that only `.Rmd` linked to `.ipynb` via `Jupytext` are commited, `.ipynb` are in the `.gitignore` because `.ipynb` metadata frequently disrupts version control whenever a notebook is opened or interacted with, while `.Rmd` files only keep track of code.
- `data`
  - This is the directory used with our `datakit-data` plugin.
  - `data/source`: contains raw, untouched data.
  - `data/processed`
    - Contains data that has either been transformed from an `etl` script or output from an `analysis` jupyter notebook.
    - Data that has been transformed from an `etl` script follow a naming convention: `etl_{file_name}.[csv,json...]`
  - `data/public`
    - Public-facing data files go here - data files which are 'live'.
  - `data/archive`: Data files that leave the scope of the project but should also remain in the project history are placed here.
- `docs`
  - Documentation on data files goes here - data dictionaries, manuals, interview notes.
- `viz`
  - This directory holds visualizations made for the project. 

**Our `.gitignore`**

```
*.vim
.env
.Renviron
.venv
.DS_Store
.ipynb_checkpoints

analysis/*.ipynb
analysis/archive/*.ipynb
!analysis/notebook_templates/*.ipynb

data/
!data/source/.gitkeep
!data/manual/.gitkeep
!data/processed/.gitkeep
!data/html_reports/.gitkeep
!data/public/.gitkeep
!data/documentation/.gitkeep

scratch/
!scratch/.gitkeep
```

## Usage

These steps assume configuration for [datakit-project](https://github.com/associatedpress/datakit-project) are complete.

- If you'd like to keep a local version of this template on your computer, git clone this repository to where your cookiecutters live:

```
cd path/to/.cookiecutters
git clone git@github.com:carolineeghisolfi/cookiecutter-hearst-project.git
```

- Now, when starting a new project with `datakit-project`, reference the cookiecutter in your filesystem. This creates a `pipenv` virtual environment and a ipython kernel for jupyter notebooks that will have the name of the `project_slug`.

```
datakit project create --template path/to/.cookiecutters/cookiecutter-hearst-project`
```

If you'd like to avoid specifying the template each time, you can edit `~/.datakit/plugins/datakit-project/config.json` to use this template by default:

```
 {"default_template": "/path/to/.cookiecutters/cookiecutter-hearst-project"}
```

### Full virtual environment setup. From package management to rendering analyses.

This python template should get data journalists set up quickly with a virtual environment, allowing them to clone a project and quickly install all the packages required to run ETL and analysis files. 

**Setup**

*This is the required setup to get the full python package management functionality provided by this template:*

- [Pyenv](https://github.com/pyenv/pyenv) to manage our python installations. `brew install pyenv`

  - We need to install a python with shared libraries via `pyenv` using the option `--enable-shared`. This gives us the ability to interact with our R install, should we ever wish to write R code in an R cell in Jupyter, or use R from a python instance using the python library `rpy2`. If we were to install version 3.9.13, for example: `env PYTHON_CONFIGURE_OPTS="--enable-shared" pyenv install 3.9.13`.

- [Pipenv](https://pipenv.pypa.io/en/latest/) to manage the python packages necessary for our project. We switch to our python with shared libraries we installed earlier (in this case version 3.9.13) with `pyenv global 3.9.13` and then pip install pipenv `python -m pip install pipenv`. It is possible to brew install pipenv, but those who maintain pipenv do not maintain that brew install of the software. They suggest pip installing.

**Workflow**

*Starting a new project*
- `datakit project create` will kick off the typical datakit cookiecutter project creation, but this template runs an additional script after constructing the analysis folder tree. Briefly, this script sets up the project for pipenv and installs our typical analysis packages. You can find this script in your project: `.first_install.py`. A more detailed description for this script will come with an update to the README.

- Once the project is created we `cd` into it and run `pipenv shell` Before running `jupyter lab`. Or, we run `pipenv run jupyter lab`. It's up to you which commands to use here. Some people like to have a subshell running via `pipenv shell`, knowing that any command they run in that open subshell will make use of the pipenv environment. Other people like to type in the command every time they want to use the virtual environment with `pipenv run [terminal command]`.

- Whenever we need to install a package, we use `pipenv install [some_package]`.

- We don't git track `.ipynb` notebooks. Instead, we use [Jupytext](https://jupytext.readthedocs.io/en/latest/) to link our `.ipynb` files to git-tracked `.Rmd` files. This makes `git diff`s much more useful. `git status` shouldn't say our analysis changed because we ran a cell again. This makes sure it doesn't.

**Cloning a project**

- When you're in the directory where you keep your analysis projects, clone the python project: `git clone git@some.git.domain:path/to/git_project.git`
- `cd` into the project and run `python .first_install.py`
  - This step will create the projects virtual environment, install all necessary packages included in the `Pipfile` using the major python version defined in the `Pipfile`, and use the `.Rmd` files in the project to generate `.ipynb` files to work with. 


## Configuration

You can set the default name, email, etc. for a project in the `cookiecutter.json` file.
