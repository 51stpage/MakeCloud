# MakeCloud

## Package Requirements
Requires the following packages:
- numpy
- scipy
- h5py
- docopt

```shell
pip install -r requirements.txt
```

## Options
 
   -h --help            Show this screen.

   ### File Parameters
   --filename=<name>    Name of the IC file to be generated
   --glass_path=<name>  Contains the root path of the glass ic [default: /home1/03532/mgrudic/glass_orig.npy] See note in [Glassy Conditions](#glassy-conditions)
   --turb_path=<name>   Path to store turbulent velocity fields so that we only need to generate them once [default: /home1/03532/mgrudic/turb]
   --param_only         Just makes the parameters file, not the IC
   --nsnap=<N>          Number of snapshots per freefall time [default: 150]
   --localdir           Changes directory defaults assuming all files are used from local directory.

   ### Box Parameters
   --boxsize=<f>        Simulation box size
   --makebox            Creates a second box IC of equivalent volume and mass to the cloud. Cannot be used with `sinkbox` or `makecylinder`. 
   --makecylinder       Creates a third, cylindrical IC of equivalent volume and mass to the cloud. Cannot be used with `sinkbox` or `makebox`.
   --sinkbox=<f>        Setup for light seeds in a turbulent box problem - parameter is the maximum seed mass in solar [default: 0.0]. Cannot be used with `makecylinder` or `makebox`.
   --cyl_aspect_ratio=<f>   Sets the aspect ratio of the cylinder, i.e. Length/Diameter [default: 10]
   --N=<N>              Number of gas particles [default: 125000]
   --length_unit=<pc>   Unit of length in pc [default: 1]

   ### Cloud Parameters
   --R=<pc>             Outer radius of the cloud in pc [default: 20.0]
   --M=<msun>           Mass of the cloud in msun [default: 1e5]
   --MBH=<msun>         Mass of the central black hole [default: 0.0]
   --Z=<solar>          Metallicity of the cloud in Solar units (just for params file) [valid:0 =< Z < 1]
   --central_SN         Sets the central sink (if MBH>0) to go supernova
   --central_star       Sets the central sink (if MBH>0) to be a ZAMS star
   --spin=<f>           Spin parameter: fraction of binding energy in solid-body rotation [default: 0.0]


   ### Physics Parameters
   --B_unit=<gauss>     Unit of magnetic field in gauss [default: 1e4]
   --G=<f>              Gravitational constant in code units [default: 4300.71]
   --ISRF=<solar>          Interstellar radiation background of the cloud in Solar neighborhood units (just for params file) [default: 1.0]
   --density_exponent=<f>   Power law exponent of the density profile [default: 0.0]
   --mass_unit=<msun>   Unit of mass in M_sun [default: 1]
   --minmode=<N>        Minimum populated turbulent wavenumber for Gaussian initial velocity field, in units of pi/R [default: 2]
   --phimode=<f>        Relative amplitude of m=2 density perturbation (e.g. for Boss-Bodenheimer test) [default: 0.0]
   --warmgas            Add warm ISM envelope in pressure equilibrium that fills the box with uniform density.
   --fixed_ncrit=<f>    Fixes ncrit to a specific value [default: 0.0]
   --alpha_turb=<f>     Turbulent virial parameter (BM92 convention: 2Eturb/|Egrav|) [default: 2.]
   --bfixed=<f>         Magnetic field in magnitude in code units, used instead of bturb if not set to zero [default: 0]

   ### Turbulence Parameters
   --turb_seed=<N>      Random seed for turbulence initialization [default: 42]
   --turb_sol=<f>       Fraction of turbulence in solenoidal modes, used when turb_type is 'gaussian' [default: 0.5]
   --turb_type=<s>      Type of initial turbulent velocity (and possibly density field): 'gaussian' or 'full' [default: gaussian, valid:'full','gaussian']
   --bturb=<f>          Magnetic energy as a fraction of the binding energy [default: 0.1]

   ### Impact Parameters
   --impact_axis=<x>    Axis along which collision occurs (z is along magnetic field lines) [default: x]
   --impact_dist=<b>    Initial separation between cloud centers of mass in units of the cloud radius. Only enabled if impact_axis > 0  (0 is no cloud-cloud collision) [default: 0.0]
   --impact_param=<b>   Impact parameter of cloud-cloud collision in units of the cloud radius [default: 0.0]
   --v_impact=<v>       Impact velocity, in units of the cloud's RMS turbulent velocity [default: 1.0]
   --v_unit=<m/s>       Unit of velocity in m/s [default: 1]


   --tmax=<N>           Maximum time to run the simulation to, in units of the freefall time [default: 5]
   --derefinement       Apply radial derefinement to ambient cells outside 3* cloud radius




## Glassy Conditions
If you want to use glassy initial conditions, you will need <a href=http://www.tapir.caltech.edu/~mgrudich/glass_orig.npy>this file</a>, and you should point the script to it via the --glass_path option.

## Turbulence Storage
By default, we generate turbulent velocity fields on the fly but then store that data somewhere, so we don't have to do all those FFTs again. You'll have to specify the path where the files get stored via the --turb_path option.

## Usage

Run `python MakeCloud.py -h` for instructions

e.g. if I wanted a 10^6 solar mass cloud of radius 100pc, resolved in 10^6 gas cells, I would do:
```shell
python MakeCloud.py --M=1e6 --R=100 --N=1e6
```

