# {{ cookiecutter.project_name }}
{% if cookiecutter.project_short_description %}
{{ cookiecutter.project_short_description }}
{% endif %}
{% if cookiecutter.full_name %}

*Created by {{ cookiecutter.full_name }} (<{{ cookiecutter.email }}>)*
{% endif %}


### Project structure

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


### Project notes

To replicate the code in this repo, run the project's `/code/etl` and `/code/analysis` files in the given order. 


### Data sources

Run `datakit data pull` to retrieve the data files. You can find more information about each file below. 

`/source/my-awesome-data-file.csv`
- <u>Source info</u> (name, url and contact info): ...
- <u>Data info</u> (geography, timeframe, etc.): ...
- <u>Key data fields</u>: ...
- <u>Data quality notes</u> (caveats, limitations, nulls, outliers, etc.): ...

...