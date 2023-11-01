# using ASE
## molecular dynamics 
```python
from ase.build import molecule
from ase.calculators.nwchem import NWChem
from ase.md.verlet import VelocityVerlet
from ase.md.nvtberendsen import NVTBerendsen
from ase import units

water = molecule("H2O")
calc = NWChem(xc="b3lyp")
water.set_calculators(calc)

dyn = VelocityVerlet(water, timestep=1.0*units.fs, trajectory="md.traj")
# dyn = NVTBerendsen(water, timestep=1.0*units.fs, temperature=500, trajectory="md.traj", taut=0.5*1000*units.fs)
dyn.run(100)
```