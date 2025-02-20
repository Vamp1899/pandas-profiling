======================================
Dataset metadata and data dictionaries
======================================

Dataset metadata
----------------
When sharing reports with coworkers or publishing online, it might be important to include metadata of the dataset, such as author, copyright holder or descriptions. ``pandas-profiling`` allows complementing a report with that information. Inspired by `schema.org's Dataset <https://schema.org/Dataset>`_, the currently supported properties are *description*, *creator*, *author*, *url*, *copyright_year* and *copyright_holder*.

The following example shows how to generate a report with a *description*, *copyright_holder* *copyright_year*, *creator* and *url*. In the generated report, these properties are found in the *Overview*, under *About*.

.. code-block:: python

    report = df.profile_report(
        title="Masked data",
        dataset={
            "description": "This profiling report was generated using a sample of 5% of the original dataset.",
            "copyright_holder": "StataCorp LLC",
            "copyright_year": 2020,
            "url": "http://www.stata-press.com/data/r15/auto2.dta",
        },
    )

    report.to_file(Path("stata_auto_report.html"))

Column descriptions
-------------------

In addition to providing dataset details, often users want to include column-specific descriptions when sharing reports with team members and stakeholders. ``pandas-profiling`` supports creating these descriptions, so that the report includes a built-in data dictionary. By default, the descriptions are presented in the *Overview* section of the report, next to each variable.

.. code-block:: python
    :caption: Generate a report with per-variable descriptions


    profile = df.profile_report(
        variables={
            "descriptions": {
                "files": "Files in the filesystem, # variable name: variable description",
                "datec": "Creation date",
                "datem": "Modification date",
            }
        }
    )

    profile.to_file(report.html)


Alternatively, column descriptions can be loaded from a JSON file: 

.. code-block:: json
     :caption: dataset_column_definition.json

        {
            column name 1: column 1 definition,
            column name 2: column 2 definition
        }

.. code-block:: python
        :caption: Generate a report with descriptions per variable from a JSON definitions file


        import json
        import pandas as pd
        import pandas_profiling

        definition_file = dataset_column_definition.json

        # Read the variable descriptions
        with open(definition_file, r) as f:
            definitions = json.load(f)

        # By default, the descriptions are presented in the Overview section, next to each variable
        report = df.profile_report(variable={"descriptions": definitions})

        # We can disable showing the descriptions next to each variable
        report = df.profile_report(
            variable={"descriptions": definitions}, show_variable_description=False
        )

        report.to_file('report.html')