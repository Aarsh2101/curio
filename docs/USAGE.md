# Usage

## Table of content
1. [Installation](#installation)
2. [Quick use](#quick-use)

## Installation

Because Curio is integrated with UTK it is necessary to add it as a submodule.

```console
git clone git@github.com:urban-toolkit/curio.git  
git submodule init
git submodule update --remote --merge
```

Curio is divided into three components: backend (provenance and database management), Python sandbox (to run Python code), and the frontend. All components need to be running.

Curio was tested on Windows 11 and MacOS Sonoma 14.5. **Python >= 3.9 & < 3.12 is needed.**

### 1. Curio Backend

The backend source code is available on the `backend` folder. It is recommended to install its requirements on a virtual environment such as [Anaconda](https://anaconda.org). Inside the `backend` folder:

```console
pip install -r requirements.txt
```

Once the requirements are installed, we have to create a SQLite database for provenance.

```console
python create_provenance_db.py
```

Now the backend server can be started.

```console
python server.py
```

The backend is also responsible for user authentication. In order to use Curio's functionalities, you will need authentication. To do so, upgrade the database by applying migrations (see below for steps).

#### Apply migrations

You need to run this command before you start using Curio:

```console
# run this to apply any migration that hasn't run yet
FLASK_APP=server.py flask db upgrade
```

If the environment variable FLASK_APP does not work with the command above, set the environnment variable in your terminal.

#### Create migration

```console
# after updating any model, run this to generate a new migration
FLASK_APP=server.py flask db migrate -m "Migration Name"
```


### 2. Python sandbox

Since modules on Curio can run Python code, it is necessary to run a Python sandbox on the `sandbox` folder.

**To run without Docker (Anaconda environment recommended):**

```console
pip install -r requirements.txt
```

Install UTK's backend module to have access to the sandbox:

```console
pip install utk
```

Run the server:

```console
python server.py
```

**If you prefer to use Docker (but you won't be able to use GPU for Ray Tracing):**

```console
docker-compose up
```

### 3. Curio frontend

Because Curio also uses UTK's frontend it is necessary to compile the UTK submodule. On the `utk-workflow` folder:

```console
cd src/utk-ts
```

NodeJS is needed to build the frontend:

```console
conda install nodejs
npm install
npm run build 
```

To start Curio's frontend, simply go to `urban-workflows` and run:

```console
npm install
npm run build
npm run start
```

To verify the code syntax and style, you can run the following:

```console
npm run lint
npm run format-check
```

To fix the code syntax and style, you can run the following:

```console
npm run lint -- --fix
npm run format
```

To run End-to-End tests with Pytest and Playwright

```shell
# after everything is setup, you can run the E2E tests
python -m pytest tests/test_frontend
```

To Format the E2E tests go to `backend/` and run:

```shell
#
python -m black tests/test_frontend
```


### Ray tracing

To use Ray Tracing, please see UTK's [requirements](https://github.com/urban-toolkit/utk).

## Quick use

For a simple introductory example check [this](https://github.com/urban-toolkit/curio/blob/main/docs/QUICK-USE.md) tutorial.  

![Tutorial](https://github.com/urban-toolkit/curio/blob/main/images/final_result.png?raw=true)


