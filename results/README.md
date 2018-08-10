# JSON files

The JSON files in this folder contain model parameters.
They can optionally contain an entry `bounds` with parameter bounds
(used for optimization/fitting) and `fit_name` that is a string
indicating the "name" of the optimization run.

## Name conventions

- d17 parameters start with `d17_`. When not specified, the parameters are for the d7 sample. 
- Next, the filename has the model_Ns, where
  * model can be G (Gaussian), wlc or others
  * N is the number of states
- `_D_` means with diffusion, i.e. intramolecular diffusion on the same timescale as lifetime
- `_noD_` means no diffusion, i.e. intramolecular diffusion on timescales longer than lifetime
- `_B_` means that background is included in the simulation
- `_offset_` means that the offset parameter in the WLC is not 0
- `_opt_` means that params are results of optimization
- after `_opt_` there is a string indicating the loss function:
    * unspecified implied E histogram loss function
    * E, nanot, FCS indicate combination of E histogram, lifetime decays and FCS
    * the number after that is the number of iterations in the optimization
    
# Checkpoint files

The checkpoint files save the a snapshot of the optimizer
during the search of optimal model parameter.

There are two kind of checkpoint files .csv (comma-separated-values) and .pkl (python pickle). 
The .pkl are not crucial as most information can be reloaded and the fit restarted using
only the CSV files.

The CSV files contain a table with parameter values and the corresponding value 
of the loss function for each iteration.

Since the loss function evolves in steps, the optimized parameters are the first
row where the loss function reaches its minimum.
The string after checkpoint in the file name refers to the item 'fit_name' in the JSON
file.

