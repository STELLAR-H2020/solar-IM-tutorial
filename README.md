# Interferometry imaging for solar radio data

## software

`DP3` [doc](https://dp3.readthedocs.io/en/latest/) for raw data handling, calibration, column operation

`wsclean` [doc](https://wsclean.readthedocs.io/en/latest/) for creating image from interferometry visibility (MS)

`lofar-sun-tools` [doc](https://lofar-sun-tools.readthedocs.io/) tools for dynamic spectrum imaging post processing 

## Using contianer

Singularity (and docker) can provide a relatively isolated env with all software installed, fully functional, plugin and play.

simple example:

(1) start a interactive shell from a image
```bash
singularity shell /dir/to/your-image.sif
```

(2) run a command, to print 'haha' to commmand line output
```bash
singularity exec /dir/to/your-image.sif echo 'haha'
```

### Contianers for solar data processing

----------
**LINC** for MS data inspection, preprocessing, flagging, calibration, and imaging

installed packages: `DP3`, `losoto`, `wsclean`

----------
**lofarsunDocker** for post processing after the imaging is done

installed packages: (python3.10) `lofar-sun-tools`


### run contianer 

to start a interactive shell:

```bash
singularity shell --bind $PWD:/home/runtime -C {$SG_DIR_SIF}  --user lofarsun
```

to start a jupyter lab session:
```bash
singularity exec --bind $PWD:/home/runtime -C {$SG_DIR_SIF}  jupyter lab --notebook-dir=/home/lofarsun --no-browser --port={$SG_PORT_NUM} --ip=0.0.0.0  --user lofarsun
```

`SG_DIR_SIF` is the directory to the singularity image.
`SG_PORT_NUM` is the port number to connect to the jupyter-lab