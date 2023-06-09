# hagn2hdf
by Seyoung Jeon (syj3514@yonsei.ac.kr)  
The `hagn2hdf` reads Horizon-AGN raw output and converts to HDF5 which is compatible with `SimSpin`.  

---

## Preparation
Install `rur` from https://github.com/sanhancluster/rur  
Also, you should have proper RAMSES output and `GalaxyMaker` data  

---

## Install
Download the code from github repository  
```bash
~$ git clone https://github.com/syj3514/hagn2hdf.git
```

Add path:
```bash
~$ conda develop ~/hagn2hdf
```

If you want to use fortran subroutine as python function, run `f2py.sh`:
```bash
~/hagn2hdf$ ./f2py.sh
```
:warning: **Please confirm your fortran compiler before f2py**

---

## Parameter setting
Set parameters in `hparams.py`
### Simulation output
Desired RAMSES output data file structure is like:
```plain
base
|---snapshots
|   |---output_00001
|   |   |   info_00001.txt
|   |   |   amr_00001.out00001
|   |   |   amr_00001.out00002
|   |   |   ...
|   |   |   part_00001.out04096
|   |   
|   |---output_00002
|   |   ...
|   |---output_00782
|
|---galaxy
|   |   tree_bricks00001
|   |   tree_bricks00002
|   |   ...
|   |   tree_bricks00782
|
|---halo
|   |   tree_bricks00001
|   |   tree_bricks00002
|   |   ...
|   |   tree_bricks00782

```
Following the structure, set `base`, `snaprepo`, `halorepo`, `full_path` properly.  
`ramsesver` indiciates the ramses code version.  
`Horizon-AGN` and `NewHorizon` is `Ra3`, and `NewHorizon2` and `NewCluster` is `Ra4`.  
`Ra3` classifies particles in `part` file using the sign of ID and epoch of particles.  
`Ra4` does it using `family`.  

### HAGN to hdf5 params
See https://kateharborne.github.io/SimSpin/

### Running params
The result hdf5 files are saved in `savedir`.  
`verbose` for debugging level.  
`nthread` is used only for fortran mode as `omp_num_threads`.  
If memory usage exceeds `maxGB`, cached data will be deallocated.  

---

## Run 
```bash
~/hagn2hdf$ python3 HAGN2HDF.py <iout>
```
You can see the log, `hagn2hdf_{iout:03d}.log`, in `savedir`.  
Output file will saved as `{savedir}/{iout:03d}/HAGN_{iout:03d}_MAGPI_{Galaxy_ID:06d}.hdf5`

---

## HDF5 format
See https://kateharborne.github.io/SimSpin/examples/generating_hdf5.html  
or See `save_hdf` function in `hfunc.py`
