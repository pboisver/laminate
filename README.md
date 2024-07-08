# laminate

Demonstrates a couple of things...

### Basic setup for Heroku using Python and FastAPI.
Heroku is an example of an enviroment we might typically target for a web application. Out of the box, the standard Heroku builders expect a pretty vanilla app, with the python version indicated, dependencies resolved using `pip`, and a run command specified.

* Procfile with the worker type and run command. The Procfile with [uvicorn](Procfile) is the default and a [gunicorn](Procfile-gunicorn) example is given
* `requirements.txt` (incl. the project package itself; see below)
* A `runtime.txt` with a Python version string (e.g. `python-3.12.4`)

### How to generate a `requirements.txt` file
Poetry is useful for development and leaves the dependencies "loosely" specified; at runtime; at runtime all dependencies should be fixed. 
*  The scripted generation uses `poetry`, `poethepoet`, and a script command in `pyproject.toml`. The command is:
    ```zsh 
    poe reqs
    ```
*  The `requirements.txt` can/should be in source control and versioned with the deployable project (esp. at release)
*  This also allows the project to be run without poetry by using `venv` and `pip`:
    ```bash
    # Create a virtual environment
    python3 -m venv .venv # using a local virual environment
    source .venv/bin/activate
    which python # will include the .venv evironment created above
    
    # install required packages
    python3 -m pip install -r requirements.txt

    ```
    or in windows:
    ```bash
    # Create a virtual environment
    py -m venv .venv
    .venv\Scripts\activate
    where python

    # install required packages
    python3 -m pip install -r requirements.txt    
    ```
    Upgrading and recreating the `requirements.txt` file directly probably doesne't make sense; let Poetry generate it.
    
### How to use a `src` layout (e.g. in a poetry project)
`src` layouts are recommended to help runtime runtime code in a python project separate from non-code artifacts or code artifacts  used in a different part of the life-cycle, e.g. `tests`. This more easily allows packages to be built with only what is needed to run.

* In our case, since it needs to run on Heroku (where the default image builder uses `requirements.txt`), if we don't specify the project package, it can be tricky to refer to the entry point module (`laminate.main:app`) in the run command.
* By adding a `.` package (the current working directory) to `requirements.txt`, the install process (e.g. on Heroku will add the project package (`laminate`) to the enviroment.
    ```
* The projects main package (`laminate` here) is specified in `pyproject.toml` to be used by `pip` to add the packages to the runtime environment.

