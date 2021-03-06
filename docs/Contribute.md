# Contribute

Intel Owl was designed to ease the addition of new analyzers. With a simple python script you can integrate your own engine or integrate an external service in a short time.


## How to add a new analyzer
You may want to look at the following analyzer example to start to build a new one: [Example](https://github.com/certego/IntelOwl/blob/master/api_app/script_analyzers/analyzer_template.py)

After having written the new python module, you have to remember to:
* Put the module in the `file_analyzers` or `observable_analyzers` directory based on what it can analyze
* Add the new module as a celery task in [tasks.py](https://github.com/certego/IntelOwl/blob/master/intel_owl/tasks.py)
* Add a new entry in the [analyzer configuration](https://github.com/certego/IntelOwl/blob/master/configuration/analyzer_config.json):
  
  Example 
  ```
    "Analyzer_Name": {
        "type": "file",
        "external_service": true,
        "leaks_info": true,
        "run_hash": true,
        "supported_filetypes": ["application/javascript"],
        "python_module": "haget_run",
        "additional_config_params": {
            "custom_required_param": "ANALYZER_SPECIAL_KEY"
        }
    },
  ```
  
  Remember to set at least:
  * `type`: can be `file` or `observable`. It specifies what the analyzer should analyze
  * `python_module`: name of the task that the analyzer must launch
  
  The `additional_config_params` can be used in case the new analyzer requires additional configuration.
  In that way you can create more than one analyzer for a specific python module, each one based on different configurations.
  MISP and Yara Analyzers are a good example of this use case: for instance, you can use different analyzers for different MISP instances.

* If possible, add a new basic unit test in [tests.py](https://github.com/certego/IntelOwl/blob/master/api_app/tests.py)
 
  Then follow the [Test](https://github.com/certego/IntelOwl/blob/master/docs/Tests.md) guide to start testing.

* Finally, add the new analyzer/s in the list in the doc: [Usage](https://github.com/certego/IntelOwl/blob/master/docs/Usage.md)

If everything is working, you can submit your pull request!