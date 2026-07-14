# externals
Code used to compute radiative and Coulomb corrections during the 6GeV era. The paper detailing how it was used for CLAS is [here](https://journals.aps.org/prc/abstract/10.1103/PhysRevC.110.025202).

## Structure of the EXTERNALS code:
The files for carbon for the CLAS12 RG-E experiment are used here as an example.

1. [INP/RGE_C.inp](https://github.com/rymilton/externals/blob/main/INP/RGE_C.inp)  - this file is the main input file that points to the other required inputs.

2. [RUNPLAN/clas_kin.inp](https://github.com/rymilton/externals/blob/main/RUNPLAN/clas_kin.inp) - this file contains the kinematics at which to calculate the cross section and RC. Note that it's pretty sensitive to formatting. The contents of the file in this repository are calculated using the x-Q2 bin centers used in the RG-E inclusive DIS analysis.

3. [TARG/targ.C_RGE](github.com/rymilton/externals/blob/main/TARG/targ.C_RGE) - this file contains a lot of info about the target (Z,A, geometry, model to use, etc.). The things you should adjust are the Z and A values and the thicknesses. For the thicknesses (in radiation lengths), `target` is the scattering target, `walls` is any material around the scattering target, `pre-target` is material before the scattering target, `post-target` is material after the scattering target. For RG-E, which uses a dual target of LD2 followed by a solid target, the LD2 thickness is placed in `pre-target` while all other materials are included in `walls`. To get the corrections for deuterium, use [INP/RGE_D2_C.inp](https://github.com/rymilton/externals/blob/main/INP/RGE_D2_C.inp) instead.

4. [runout/RGE_C.out](https://github.com/rymilton/externals/blob/main/runout/RGE_C.out) - Contains the corrections for each of the kinematic points from [RUNPLAN/clas_kin.inp](https://github.com/rymilton/externals/blob/main/RUNPLAN/clas_kin.inp). 

5. OUT/RGE_C.out  - this gives more detailed output than the summary table above. Note this file isn't in the repository as it can get pretty large.

6. To run, you just use the little script (run_extern) in the top level directory.
The usage is: "./run_extern <input-file>", leave the ".inp" off the input file. 

## Compiling and running EXTERNALS:
The code is currently set up to run on ifarm (JLab computing cluster). 
To compile the code, do:
```
  source set_env.sh
  make
```

To run the code, do:
`./run_extern RGE_C` (change to your target)


Output files are in `OUT/` and `runout/`

## Adding a target for Coulomb corrections:
The code will use a default calculation for the Coulomb corrections if the target is not in `externals_all.f`. To add the target, add `deltae_cc` to [this block](https://github.com/rymilton/externals/blob/main/externals_all.f#L403-L427)  and [this block](https://github.com/rymilton/externals/blob/main/externals_all.f#L2084-L2107). `deltae_cc` can be calculated using the equations here [https://github.com/rymilton/externals/blob/main/externals_all.f#L398-L401](https://github.com/rymilton/externals/blob/main/externals_all.f#L398-L401). Note that the number should be in GeV!!

## Common errors
- If you get the error, `Fortran runtime error: Cannot open file 'OUT/RGE_C_details.out': No such file or directory`, you need to create the directory `OUT`.
