# Reflexer Digital Twin

## Introduction

The Reflexer Digital Twin is a comprehensive modular toolkit for performing automated routine tests and system predictions that are aware of the controller fundamentals as well as the available live data.

The goal is to be a decision-oriented and data-driven computational
representation of RAI, while retaining the properties of being easy-to-consume,
easy-to-modify and provenance keeping.

The backtesting and extrapolation components are powered by cadCAD, a framework for generalized dynamical systems that allows for expressing the behavioural and logical mechanisms behind crypto-economic systems.

![RAI Digital Twin Components Diagram](assets/dt-components.png)

Specifically, it accomplishes the following functions:

- Data Interface: The DT has integration with the same live data that the RAI controller, as well as integrations with Data Lakes for exporting result sets.
- Backtesting: The DT is able to verify the past controller behaviour to make sure that it is working as intended.
- System Identification: The DT is able to identify and quantify past patterns for usage in extrapolation and scientific contexts.
- Future State Extrapolation: The DT is able to make use of data-driven mechanisms in order to extrapolate and predict the system trajectory over the future.
- Report Generation: The DT is able to generate diagnostics and rich visualizations that informs about the state of the system with a acessible focus.

## Usage 

### Installation

The Reflexer Digital Twin requires **Python 3.9** and all the dependencies at `requirements.txt` installed. 

On a linux machine, this is achievable by (thanks @bkellerman)

```
$ python -V
Python 3.9.6

$ python -m venv venv
$ source venv/bin/activate
$ python -m pip install -r requirements.txt 
```

The setup can be tested by making an restricted run of the digital twin:

`python -m rai_digital_twin -l -e 5`

### Execution

The standard execution flow can be invoked by passing `python -m rai_digital_twin`. Custom execution arguments are available, with an description given by the `--help` option.

The standard execution flow will retrieve, prepare, backtest, fit and extrapolate over the existing data.

This will retrieve, prepare, backtest, fit and extrapolate over the existing 
data.

As of now, it is possible to configure the DT parameters by directly modified 
the `rai_digital_twin/__main__.py` call on the `extrapolation_cycle()` call. 
Options can include:

- Number of Monte Carlo runs for the USD/ETH price
- Interval for retrieving and backtesting data
- Re-utilize existing past data rather than retrieving

### Result Analysis

The generated data will be located at `data/runs`, while any reports will be 
located at `reports/`

### Testing

The Reflexer Digital Twin uses `pytest` for unit and integration testing. 
In order to make use of it, just pass:

``python -m pytest``
## Components

The RAI Digital Twin is made of several semi-independent components that act 
together for making a decision-oriented data-driven representation of RAI.

Everything is glued together by the notion of an 'Extrapolation Cycle',
which is encoded at `rai_digital_twin/execution_logic.py`, on which the
core interfaces and functions are executed while using the cadCAD model as a 
basis for embedding the context-dependent knowledge for the controller and
the behavioural user actions.

### Reports

The Digital Twin generates interactive HTML reports by using `nbconvert`
together with parametrized notebooks which are located on the
 `rai_digital_twin/templates` folder. The default output path is located
 on the `reports` folder.

Those are generated by invoking the command line on 
`rai_digital_twin/execution_logic.py` and it is possible to modify the existing
templates and/or to add more execution points on the extrapolation cycle itself 
depending on the use-cases.

### Data interface

The data retrieval is handled by the `rai_digital_twin/retrieve_data`, on which
historical series is collected from The Graph on the RAI Subgraph. 

Currently, it works by retrieving all the available subgraph hourly statistics
and using them as reference points for getting the system state and the
SAFEs history on given block heights.

After this is done, pre-processing is done at the 
`rai_digital_twin/prepare_data.py` file, where the tabular datasets are
instantiated into relevant objects as described in `rai_digital_twin/types.py`.

### Backtesting

Backtesting consists in providing the cadCAD model representation the same
information that is available to the real controller so that the modelled
action is computed and compared against the real action.

The goal is to be able to measure how descriptive the model is against the
objective reality. This is done numerically by using the controller outputs
as validation metrics on which loss functions can be defined, aggregated
and mixed. This is done on `rai_digital_twin/backtesting.py`
### System Identification

System Identification is to identify and generate required priors for performing
extrapolation. Generically, this is done by estimating parameters associated
with behavioural models and by identifying stochastic processes that are 
descriptive of exogenous signals, like the ETH / USD price.

As of the current implementation, this is done in two steps:

1. Exogenous signals are statistically fitted and extrapolated before the 
Future State Extrapolation, so that are used as a input.
2. Behavioural models are fitted on-the-go during the simulation timesteps, as
the current behavioural models are auto-regressive in nature.
### Future State Extrapolation

Future State Extrapolation is about making inferences and predictions over 
the time evolution. This consists in using behavorial models and stochastic
process for doing projections while using the encoded knowledge about RAI as
constraints.

By doing a range of parameter sweeps and Monte Carlo runs, it is possible to
understand what are the expected future scenarios under a variety of conditions
and probabilities.


## Notebooks
* [data_acquisition.ipynb](notebooks/data_acquisition.ipynb) shows how the data was obtained and from which sources
* [Systems_Identification_Fitting.ipynb](notebooks/Systems_Identification_Fitting.ipynb) documents the iterative process of constructing the systems identification model at the heart of the Digital Twin
* [VAR_vs_VARMAX_evaluation.ipynb](notebooks/VAR_vs_VARMAX_evaluation.ipynb) is an experimental notebook determining if VAR or VARMAX was a better fit


## References

- Video Walkthrough: https://youtu.be/CLOr-xVePJ0