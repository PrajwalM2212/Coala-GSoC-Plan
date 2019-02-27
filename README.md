# Coala Gsoc Plan


## Lifecycle of Configuration

The entire configuration lifecycle resides in `gather_configuration` function. This function returns `sections, local_bears, global_bears and targets`. This is the first important point for this project as this defines the boundary and makes the project well defined.

Firstly it calls `load_configuration` function which returns `sections` and `targets`. It also calls a number of other functions `_set_section_language` which sets the language for all the sections, `aspectize_sections` which handles aspect based settings, `fill_settings` which returns `local_bears` and `global_bears` and `save_sections` which writes back to the desired config file. Essentially it returns all the data required to run coala. 

The point of interest for this project is the `load_configuration` and `save_sections` functions. The others functions called by `gather_configuration` process upon the data returned by `load_configuration`. This marks the second important point. The project now narrows down to two places of interest `load_configuration` and `save_sections` functions. Now the project has a more well defined boundary.

###
