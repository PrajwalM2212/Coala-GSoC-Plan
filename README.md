# Coala Gsoc Plan


## Lifecycle of Configuration

The entire configuration lifecycle resides in `gather_configuration` function. This function returns `sections, local_bears, global_bears and targets`. This is the first important point for this project as this defines the boundary and makes the project well defined.

Firstly it calls `load_configuration` function which returns `sections` and `targets`. It also calls a number of other functions `_set_section_language` which sets the language for all the sections, `aspectize_sections` which handles aspect based settings, `fill_settings` which returns `local_bears` and `global_bears` and `save_sections` which writes back to the desired config file. Essentially it returns all the data required to run coala. 

The point of interest for this project is the `load_configuration` and `save_sections` functions. The others functions called by `gather_configuration` process upon the data returned by `load_configuration`. This marks the second important point. The project now narrows down to two places of interest `load_configuration` and `save_sections` functions. Now the project has a more well defined boundary. As a matter of fact this project may as well be equally divided between these two functions.

### `load_configuration`
In gist this function gets sections from all sources (`.coafile`, `.coarc`, `system_caofile`, `cli`) and merges them to give the final sections and targets. The real point of interest in this function is `load_config_file` function that gets sections from all the different config files. Getting into `load_config_file`, the point of interest changes to `ConfParser`. This is where the first part of the project resides in. 
`ConfParser` inturn depends upon the `LineParser`, but not of much interest at this time. But the output of `LineParser` is important. It gives `(section_name, keys, value, append, comment)` tuple for each line. 

```python
('LineCounting', [], '', False, '')
('', [('', 'enabled')], 'False', False, '')
('', [], '', False, '')
('', [('', 'bears')], 'LineCountBear', False, '')
('', [('', 'max_lines_per_file')], '1000', False, '')
('', [], '', False, '')
```
This individual line data is used to generates settings and ultimately sections. 

This is also how this project will have to work on. But instead of `LineParser`, the project has `toml.load(f, _dict=dict)` function. If the output generated by `LineParser` is a tuple for each line, the output given by `toml.load` is a dictionary.

The major benifit of using `TOML` apparent here, the `LineParser` is no more needed and the cumbersome work required to join those lines into sections is also no more needed. Custom sub-level parsing which is a tough task for INI style config is can be handled with ease. The benifit of this new liberty allows for implementing complex functionalities and also gives the freedom to prototype and experiment.

So the task now points to having to parse the dictionary provided by `toml.load` to generate sections. This is one of the tasks for this project. 
To write code to generate sections from the dictionary. The code has to accomodate for all the features of the current configuration system like inheritance. Also the data from the dictionary must be converted to the format that `Section` and `Setting` understands. 
Since TOML does not understand config like `bears = SpaceConsistencyBear, LineLengthBear` definite mappings have to be decided upon. `bears = SpaceConsistencyBear, LineLengthBear` has to be `bears = [ SpaceConsistencyBear, LineLengthBear ]` in TOML. This works to remove ambiguity which gives another benifit of using TOML. But challenges do arise , what does `files += app.py` map into in TOML ?

But the end goal is to get the sections.


