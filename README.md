# DYNAMIC Fire risk indicator implementation


This repository contains the implementation of the dynamic fire risk indicator described in the submitted paper:

R.D: Strand and L.M. Kristensen. *An implementation, evaluation and validation of a dynamic fire and conflagration risk indicator for wooden homes*. Submitted for review.

which uses forecast and weather data observation for computing fire risk indication in the form of time-to-flash-over (ttf) for wooden houses. 

# Installation

The project is based on using the [Poetry package manager](https://python-poetry.org/).

Start by installing Poetry for your platform using the [installation instructions](https://python-poetry.org/docs/#installation)

To install the required Python packages use:

```
poetry install
```
# Overview

The implementation is organised into the following main folders:

- `datamodel` - contains an implementation of the data model used for weather data and fire risk indications
- `weatherdata`  contains an client implementations and interfaces for fetching weather data from cloud services.
- `fireriskmodel` contains an implementation of the underlying fire risk model

The main API for the implementation is in the file `frcapi.py`

# Weather Data Sources

The implementation has been designed to be independent of any particular cloud-based weather data service. 

This library contains an implementation that use the weather data services provided by the Norwegian Meteorological Institute (MET)

Specifically, the files `src/frcm/weatherdata/client_met` and `src/frcm/weatherdata/extractor_met.py` shows how to implement client and extractors using

- MET Frost API for weather data observations: https://frost.met.no/index.html
- MET Forecasting API for weather data forecasts: https://api.met.no/weatherapi/locationforecast/2.0/documentation 

To use these pre-implemented clients a file name `.env` must be put into the `src/frcm/weatherdata` folder having the following content:

```
MET_CLIENT_ID = '<INSERT CLIENT ID HERE>'
MET_CLIENT_SECRET = '<INSERT CLIENT SECRET HERE>'
```

Credentials for using the MET APIs can be obtained via: https://frost.met.no/auth/requestCredentials.html

Please make sure that you conform to the terms of service which includes restrictions on the number of API calls.

# Testing

The folder `src/tests` contains a number of unit tests that can be used to test the implementation. 

To execute the tests enter the `src/tests` folder and execute

```
pytest
```

You may also execute a specific test-file in the test-suite using, e.g., 

```
pytest test_datamodel.py
```

or running a specific test within a test-file using, e.g., 

```
pytest test_datamodel.py::TestDataModel::test_validate
```

# Application integration

The file `src/main.py` provides sample code on how to compute fire risk indications and how to integration the implementation of fire risk indications into applications.

Running the command

```
python3 main.py
```

should result in similar output as below 

```
...

FireRiskPrediction[latitude=60.383 longitude=5.3327]
FireRisks[2024-01-13 00:00:00+00:00 6.072481167177002]
FireRisks[2024-01-13 01:00:00+00:00 6.084901639685655]
FireRisks[2024-01-13 02:00:00+00:00 6.115931368998181]
FireRisks[2024-01-13 03:00:00+00:00 6.146731614820376]
FireRisks[2024-01-13 04:00:00+00:00 6.157474811242355]
FireRisks[2024-01-13 05:00:00+00:00 6.148031627082924]
FireRisks[2024-01-13 06:00:00+00:00 6.135871365180064]

...

FireRisks[2024-01-24 23:00:00+00:00 5.782145706635683]
FireRisks[2024-01-25 00:00:00+00:00 5.789519566277565]
FireRisks[2024-01-25 01:00:00+00:00 5.796865122491866]
FireRisks[2024-01-25 02:00:00+00:00 5.80463250152655]
FireRisks[2024-01-25 03:00:00+00:00 5.812626103446193]
FireRisks[2024-01-25 04:00:00+00:00 5.820770098316547]
FireRisks[2024-01-25 05:00:00+00:00 5.829024065716487]
FireRisks[2024-01-25 06:00:00+00:00 5.837363568856687]
```

showing hourly computed fire risks for the given location.

