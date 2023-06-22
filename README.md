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

#### Regularly used args

* `--bind` or `-B` 

binding directory, expose local directories to the contianer.

For example, binding your current directory to a direcotry named `/home/secret/files/`, the command would be: 

```bash
singularity shell -B $PWD:/home/secret/files/ your-image.sif
```

then you can access the files in host:$PWD at `/home/secret/files/` from inside the contianer.

* `-C`

fully contianed, isolate the /bin and /lib, recommend to use for not mixing the enviroment commands


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

## Calibration

TBD

## Imaging

Go to the location where the data is stored, start linc.sif singularity:

```bash
singularity shell -B $PWD:/run/mnt /data/containers/linc.sif
```

then run imaging with wsclean:

```bash
wsclean -j 2 -mem 10 -size 512 512 -scale 12asec -pol I -data-column CORRECTED_DATA -niter 1000 -intervals-out 2 -interval 5010 5020 -name SB150 L242126_SB150_uv.dppp.MS
```