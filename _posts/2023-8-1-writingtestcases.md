---
layout: post
title: Writing Test Cases
---

For testing specturm produce using vaex and pandas for non-equilibrium calculations are same , the code similar to equilibrium calculations is used 

```
from radis import calc_spectrum

import time
t0=time.time()

s, factory_s = calc_spectrum(1800, 1820,         # cm-1
                      molecule='CO',
                      isotope='1',
                      pressure=1.01325,   # bar
                      Tgas=700,           # K
                      Tvib=710,
                      Trot=710,
                      mole_fraction=0.1,
                      wstep='auto',
                      path_length=1,      # cm
                      databank='hitemp',  # or 'hitemp', 'geisa', 'exomol'
                      optimization=None,
                      engine='vaex',
                      verbose=3,
                      return_factory=True,
                      )

s.apply_slit(0.5, 'nm')       # simulate an experimental slit

t1=time.time()
print('Time taken : '+str(t1 - t0))

t0=time.time()

s1, factory_s1 = calc_spectrum(1800, 1820,         # cm-1
                      molecule='CO',
                      isotope='1',
                      pressure=1.01325,   # bar
                      Tgas=700,           # K
                      Tvib=710,
                      Trot=710,
                      mole_fraction=0.1,
                      wstep='auto',
                      path_length=1,      # cm
                      databank='hitemp',  # or 'hitemp', 'geisa', 'exomol'
                      engine='pandas',
                      verbose=3,
                      return_factory=True,
                      )

s.apply_slit(0.5, 'nm')       # simulate an experimental slit

t1=time.time()
print(s.get("absorbance"))
s.plot('radiance_noslit')
print('Time taken : '+str(t1 - t0))

import numpy as np
print(np.allclose(s.get("absorbance"), s1.get("absorbance")))

for column in factory_s1.df1.columns:
    assert np.all(factory_s1.df1[column] == factory_s.df1[column].to_numpy())
```

I will add more test cases .