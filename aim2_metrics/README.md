# AIM 2 Metrics

AIM 2 Metrics is a subproject of Aalto Interface Metrics (AIM). The subproject contains old and new metrics that are compatible with Python 3 and have been rigorously tested. The metrics will be incorporated into the next major release of AIM (version 2), to be published in 2021.


## Structure

The structure of this subproject is as follows:
```
.
├── aim                          : AIM source codes
│   ├── core                     : Core functions
│   ├── evaluators               : GUI design evaluators
│   ├── exceptions               : Custom exceptions and errors
│   ├── metrics                  : AIM metrics
│   │   ├── interfaces.py        : AIM metric interfaces
│   │   ├── m1_png_file_size.py  : AIM metric 1
│   │   └── ...
├── data                         : Data directory
│   ├── inputs                   : Input datasets
│   ├── outputs                  : Evaluation results
│   └── tests                    : Test files
├── tests                        : Unit tests
│   ├── core                     : Tests for core functions
│   └── metrics                  : Tests for AIM metrics
│   │   ├── test_m1.py           : Tests for AIM metric 1
│   │   └── ...
├── gui_designs_evaluator.py     : GUI design evaluator application
├── ...
.
```


## Requirements

You need [Python 3.7](https://www.python.org/) (or later), [pip](https://pypi.org/project/pip/), and [git](https://git-scm.com/) to work on this subproject.

In addition, it is highly recommended to install [virtualenv](https://pypi.org/project/virtualenv/) or [Pipenv](https://pypi.org/project/pipenv/) to create a dedicated Python virtual environment. You can find more information on this below.


## Installation

Clone the AIM git repository and head over to the [aim2_metrics](./) directory (this subproject):
```
git clone https://github.com/aalto-ui/aim.git && cd aim/aim2_metrics
```

### Working with `virtualenv`

Create a new virtual environment:
```
virtualenv ../venv
```

Activate the virtual environment:
```
source ../venv/bin/activate
```

Install all dependencies, including development packages:
```
pip install -r requirements.txt
```

Re-activate the virtual environment to update paths (see [Stack Overflow](https://stackoverflow.com/questions/35045038/how-do-i-use-pytest-with-virtualenv) for details):
```
deactivate && source ../venv/bin/activate
```

To deactivate the virtual environment, run:
```
deactivate
```

### Working with `Pipenv`

Install all dependencies, including development packages:
```
pipenv install --dev
```

Activate your Pipenv environment:
```
pipenv shell
```

To deactivate your Pipenv environment, run:
```
exit
```


## Tests <a name="tests"></a>

This subproject uses [pytest](https://pypi.org/project/pytest/), a Python testing framework to run tests on source code, including metrics.

### Configuration

Configure pytest, if needed:
```
nano pytest.ini
```

### Usage

Run all tests:
```
pytest .
```

Run a specific test file:
```
pytest [FILEPATH]
```


## Evaluating GUI Designs <a name="evaluation"></a>

We provide a utility application to evaluate GUI designs (web page screenshots) against metrics. The application generates a CSV file with evaluation results, and optionally plot figures for each metric.

### Configuration

Configure [Loguru](https://pypi.org/project/loguru/), if needed:
```
nano loguru.ini
```

Edit [evaluators.py](./aim/evaluators/evaluators.py) (see private constants) to control which metrics are used in GUI design evaluation:
```
nano aim/evaluators/evaluators.py
```

### Datasets

[Alexa Top Global Sites](https://www.alexa.com/topsites) (ALEXA_50) currently serves as our default input dataset. Additional datasets can be downloaded, for instance, from https://doi.org/10.7910/DVN/XEYNYW.

### Usage

Show the help message:
```
python gui_designs_evaluator.py -h
```

Evaluate GUI designs:
```
python gui_designs_evaluator.py -i data/inputs/ALEXA_TOP_50/ -o data/outputs/ -p
```


## Adding New Metrics

1. For your metric, create a new Python file within the [AIM metrics](./aim/metrics/) directory. For example, `m7_your_metric_name.py`.
2. Implement a metric class within the file. The class must implement the `AIMMetricInterface`, specified in the [interfaces.py](./aim/metrics/interfaces.py) file. The easiest way to get started is to copy paste the code from an existing metric file (e.g., `m1_png_file_size.py`) and modify it for your needs. **Note:** Remember to include extensive documentation to your metric.
3. For testing your metric, create a new Python file within the [unit tests](./tests/metrics/) directory. For example, `test_m7.py`.
4. Implement a unit test within the file. Again, the easiest way to get started is to copy paste the code from an existing unit test file (e.g., `test_m1.py`) and modify it for your needs. **Note:** You might need to include metric-specific test files within the [test files](./data/tests/) directory.
5. Run the test file (see [Tests](#tests)).
6. Edit [evaluators.py](./aim/evaluators/evaluators.py) (see private constants) to include your metric and its results as part of the GUI designs evaluation.
7. Run the GUI designs evaluator application (see [Evaluating GUI Designs](#evaluation)).


## Utility Tools

This subproject supports the following utility tools to (i) ease development and (ii) improve code quality. Their installation and use is optional, but highly recommended.

- **isort.** Python utility to automatically sort imports. https://pypi.org/project/isort/
- **Black.** Python code formatter. https://pypi.org/project/black/
- **Mypy.** Static type checker for Python. https://pypi.org/project/mypy/
- **Flake8.** Python tool for style guide enforcement. https://pypi.org/project/flake8/
- **pre-commit.** Package manager for pre-commit hooks. https://pypi.org/project/pre-commit/

### Installation

Install pre-commit into your git hooks:
```
pre-commit install --install-hooks --overwrite
```

To uninstall pre-commit from your git hooks, run:
```
pre-commit uninstall
```

### Configuration

Configure isort, if needed:
```
nano .isort.cfg
```

Configure Black, if needed:
```
nano pyproject.toml
```

Configure mypy, if needed:
```
nano mypy.ini
```

Configure flake8, if needed:
```
nano .flake8
```

Configure pre-commit, if needed:
```
nano .pre-commit-config.yaml
```

### Usage

Sort imports:
```
isort .
```

Format code:
```
black .
```

Type check code:
```
mypy .
``` 

Lint code:
```
flake8 .
```

Run all pre-commit hooks against currently staged files:
```
pre-commit run
```

Run a single pre-commit hook against currently staged files:
```
pre-commit run [HOOK ID]
```

Run all pre-commit hooks against all files:
```
pre-commit run --all-files
```

Run all pre-commit hooks against specific files:
```
pre-commit run --files [FILES [FILES ...]]
```


## Troubleshooting

1. **PROBLEM:** Running `mypy` fails and the following error message is shown "./venv/lib/python3.7/site-packages is in the PYTHONPATH. Please change directory so it is not." **SOLUTION:** Create the virtual environment outside of the current subproject directory. For example, `virtualenv ../venv` Mypy 0.810 will include the `--exclude` argument to address this issue (see [pull request #9992](https://github.com/python/mypy/pull/9992)).


## License

Copyright (c) 2018-present, [User Interfaces group](https://userinterfaces.aalto.fi/), [Aalto University](https://www.aalto.fi/), Finland

This software is distributed under the terms of the [MIT License](https://opensource.org/licenses/MIT). See [LICENSE.txt](../LICENSE.txt) for details.
