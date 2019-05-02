```python
from aguaclara.core.units import unit_registry as u
import aguaclara.research.environmental_processes_analysis as epa
import aguaclara.core.physchem as pc
import aguaclara.core.utility as ut
import numpy as np
import matplotlib.pyplot as plt
from scipy import stats

Column_D = 1 * u.inch
Column_A = pc.area_circle(Column_D)
# Column_L = 15.2 * u.cm\Ct]=[4e c0sx6g]-olumn_V = Column_A * Column_L
Porosity = .4

SaltyWater = Porosity*3*(Column_V).to(u.mL)
SaltyWater
```
