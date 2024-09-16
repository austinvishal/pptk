# pptk - Point Processing Toolkit

Copyright (C) 2011-2018 HERE Europe B.V.

The Point Processing Toolkit (pptk) is a Python package for visualizing and processing 2-d/3-d point clouds.

At present, pptk consists of the following features.

* A 3-d point cloud viewer that
  - accepts any 3-column numpy array as input,
  - renders tens of millions of points interactively using an octree-based level of detail mechanism,
  - supports point selection for inspecting and annotating point data.
* A fully parallelized point k-d tree that supports k-nearest neighbor queries and r-near range queries
  (both build and queries have been parallelized).
* A normal estimation routine based on principal component analysis of point cloud neighborhoods.

[Homepage](https://heremaps.github.io/pptk/index.html)

![pptk screenshots](/docs/source/tutorials/viewer/images/tutorial_banner.png)

The screenshots above show various point datasets visualized using pptk.
The `bildstein1` Lidar point cloud from Semantic3D (left),
Beijing GPS trajectories from Geolife (middle left),
`DistrictofColumbia.geojson` 2-d polygons from US building footprints (middle right),
and a Mobius strip (right).
For details, see the [tutorials](https://heremaps.github.io/pptk/tutorial.html).

## License

Unless otherwise noted in `LICENSE` files for specific files or directories,
the [LICENSE](LICENSE) in the root applies to all content in this repository.

## Install

One can either install pptk directly from PyPI

```
>> pip install pptk
```

or from the .whl file that results from [building pptk from source](#build).

```
>> pip install <.whl file>
```

## Quickstart

In Python, generate 100 random 3-d points.

```
>> import numpy as np
>> x = np.random.rand(100, 3)
```

Visualize.

```
>> import pptk
>> v = pptk.viewer(x)
```

Set point size to 0.01.

```
>> v.set(point_size=0.01)
```

For more advanced examples, see [tutorials](https://heremaps.github.io/pptk/tutorial.html).

## Build

We provide CMake scripts for automating most of the build process, but ask the
user to manually prepare [dependencies](#requirements) and record their paths
in the following CMake cache variables.

* `Numpy_INCLUDE_DIR`
* `PYTHON_INCLUDE_DIR`
* `PYTHON_LIBRARY`
* `Eigen_INCLUDE_DIR`
* `TBB_INCLUDE_DIR`
* `TBB_tbb_LIBRARY`
* `TBB_tbb_RUNTIME`
* `TBB_tbbmalloc_LIBRARY`
* `TBB_tbbmalloc_RUNTIME`
* `Qt5_DIR`

To set these variables, either use one of CMake's GUIs (ccmake or cmake-gui),
or provide an initial CMakeCache.txt in the target build folder
(for examples of initial cache files, see the CMakeCache.<platform>.txt files)

##### Requirements

Listed are versions of libraries used to develop pptk, though earlier versions
of these libraries may also work.

* [QT](https://www.qt.io/) 5.4
* [TBB](https://www.threadingbuildingblocks.org/) 4.3
* [Eigen](http://eigen.tuxfamily.org) 3.2.9
* [Python](https://www.python.org/) 2.7+ or 3.6+
* [Numpy](http://www.numpy.org/) 1.13

##### Windows

1. Create an empty build folder

```
>> mkdir <build_folder>
```

2. Create an initial CMakeCache.txt under <build_folder> and use it to provide
values for the CMake cache variables listed above. (e.g. see CMakeCache.win.txt)

3. Type the following...

```
>> cd <build_folder>
>> cmake -G "NMake Makefiles" <source_folder>
>> nmake
>> python setup.py bdist_wheel
>> pip install dist\<.whl file>
```

##### Linux

Similar to building on Windows.

For Conda Users with Python 3.9 and Ubuntu 22.04 (To be precise Miniconda 24.7.1, Python 3.9.18, Ubuntu 22.04.4). Given below are the instructions



1. Download the zip file from first comment [45](https://github.com/heremaps/pptk/pull/45) in the issue
extract it.
2. Rename 38 to 39 for your python3.9 version inside the extracted folder \
 or Here is the wheel for convenience, just download, extract and proceed to step 3:\
 [pptkcp39](https://github.com/austinvishal/pptk/blob/pptk_view/resources/pptk-0.1.1-cp39-none-manylinux2014_x86_64.zip) \
3. For installation inside conda environment
activate your conda environment, go to the downloaded folder where extracted file is and 
```
pip3 install  ./pptk-0.1.0-cp39-none-manylinux1_x86_64.whl
```
will install the necessary libraries libraries

4. Run an example given in the github as python file from terminal

# Issues faced

Issue 1: ImportError: libpython3.8.so.1.0: cannot open shared object file: No such file or directory \
Solution: \
a) Create sym link to have libpython3.8 library, for this create a new conda env withpy3.8 first if you don't have python 3.8 \
b) Go to conda/envs/envname/lib/python3.9/site-packages/pptk/libs/libz.so and rename the libz.so file or cut it and paste it somewhere safe, here we / will use the symbolic link of libpython3.8.so file \
then   \
```
ln -s /home/{user_name}/anaconda3/envs/{env_name}/lib/libz.so.1 /home/{user_name}/anaconda3/envs/{env_name}/lib/python3.7/site-packages/pptk/libs/libz.so.1
```
Example in this case:
```
ln -s /home/vishal/.miniconda3/envs/legion/lib/libz.so /home/vishal/.miniconda3/envs/mujoco_py/lib/python3.9/site-packages/pptk/libs/libz.so
```
```
ln -s /home/vishal/.miniconda3/envs/legion/lib/libpython3.8.so.1.0 /home/vishal/.miniconda3/envs/mujoco_py/lib/python3.9/site-packages/pptk/libs/libpython3.8.so.1.0
```

Based on:  [3](https://github.com/heremaps/pptk/issues/3) and [4](https://github.com/heremaps/pptk/issues/4 ) 

c) Now use the example given in the github and save it as pptkexample1.py to view 100 points as point cloud in the viewer and run the example in the terminal
```
python pptkexample1.py
```
Issue 2: The viewer pops up and closes \
Solution:  \
After line 52 in viewer.py, set debug=True  (temporary fix) based on, [24](https://github.com/heremaps/pptk/issues/24) 

##### Mac

Similar to building on Windows.
