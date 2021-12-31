# AP Python Cookiecutter

This is a project template powered by [Cookiecutter](https://github.com/cookiecutter/cookiecutter) for use with [datakit-project](https://github.com/associatedpress/datakit-project/).

**Structure**

```
.
├── README.md
├── analysis
│   └── archive
├── data
│   ├── documentation
│   ├── html_reports
│   ├── manual
│   ├── processed
│   ├── public
│   └── source
├── etl
├── publish
└── scratch
```

- `README.md`
  - Project-specific readme with boilerplate for data projects.
- `analysis`
  - This is where we keep all of our jupyter ipython notebooks that contain analysis for the project.
    - Notebooks in this folder can ingest data from either `data/source` (if that data comes from the source in a workable format) or `data/processed` (if the data required some prep).
    - Dataframes from analysis notebooks should be written out to `data/processed`
  - `analysis/archive`: Notebooks that leave the scope of the project but should also remain in the project history will be placed here.
  - Note that only `.Rmd` linked to `.ipynb` via `Jupytext` are commited, `.ipynb` are in the `.gitignore` because `.ipynb` metadata frequently disrupts version control whenever a notebook is opened or interacted with, while `.Rmd` files only keep track of code.
- `data`
  - This is the directory used with our `datakit-data` plugin.
  - `data/documentation`
    - Documentation on data files should go here - data dictionaries, manuals, interview notes.
  - `data/html_reports`
    - Contains rendered html of our analysis notebooks, the results of calling `pipenv run export_rmarkdown` on a notebook.
  - `data/manual`
    - Contains data that has been manually altered (e.g. excel workbooks with inconsistent string errors requiring eyes on every row).
  - `data/processed`
    - Contains data that has either been transformed from an `etl` script or output from an `analysis` jupyter notebook.
    - Data that has been transformed from an `etl` script will follow a naming convention: `etl_{file_name}.[csv,json...]`
  - `data/public`
    - Public-facing data files go here - data files which are 'live'.
  - `data/source`: contains raw, untouched data.
- `etl`
  - This is where we keep python scripts involved with collecting data and prepping it for analysis.
  - These files should be scripts, they should not be jupyter notebooks.
- `publish`
  - This directory holds all the documents in the project that will be public facing (e.g. data.world documents).
- `scratch`
  - This directory contains output that will not be used in the project in its final form.
  - Common cases are filtered tables or quick visualizations for reporters
  - This directory is not git tracked.

**Our `.gitignore`**

```
.DS_Store

.ipynb_checkpoints
*.ipynb

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
git clone git@github.com/associatedpress/cookiecutter-python-project
```

- Now, when starting a new project with `datakit-project`, reference the cookiecutter in your filesystem. This creates a `pipenv` virtual environment and a ipython kernel for jupyter notebooks that will have the name of the `project_slug`.

```
datakit project create --template path/to/.cookiecutters/cookiecutter-python-project`
```

If you'd like to avoid specifying the template each time, you can edit `~/.datakit/plugins/datakit-project/config.json` to use this template by default:

```
 {"default_template": "/path/to/.cookiecutters/cookiecutter-python-project"}
```

This template generates R style html reports because interfacing with `pandas` dataframes via the interactive `rtables` in R-style html reports is easier compared to tables in html reports generated by `nbconvert`. Thus, when working with jupyter notebooks in `analysis`, link the `.ipynb` to a `.Rmd` (file -> Jupytext -> Pair Notebook with R Markdown). Include the following meta data in the top of the notebook:

```
{r, setup, echo=F, include=F}
knitr::opts_chunk$set(echo=FALSE)
library(DT)
library(reticulate)
```

To generate the html report from the `.Rmd`:
```
pipenv run export_rmarkdown path/to/analysis_notebook.Rmd
```

## Configuration

You can set the default name, email, etc. for a project in the `cookiecutter.json` file.
