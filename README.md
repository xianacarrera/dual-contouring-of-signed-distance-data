# Dual Contouring of Signed Distance Data

This repository contains the code for the SIGGRAPH 2026 paper [Dual Contouring of Signed Distance Data](https://gatc.cs.columbia.edu/projects/dual-contouring-of-signed-distance-data.html), by Xiana Carrera, Ningna Wang, Christopher Batty, Oded Stein, and Silvia Sellán.

> [!CAUTION]
> This code was tested on macOS only. If you encounter issues with other platforms, please contact [x.carrera@columbia.edu](mailto:x.carrera@columbia.edu).


We propose an algorithm to reconstruct explicit polygonal meshes from discretely sampled Signed Distance Function (SDF) data, which is especially effective at recovering sharp features.Building on the traditional Dual Contouring of Hermite Data method, we design and solve a quadratic optimization problem to decide the optimal placement of the mesh's vertices within each cell of a regular grid. Critically, this optimization relies solely on discretely sampled SDF data, without requiring arbitrary access to the function, gradient information, or training on large-scale datasets. Our method sets a new state of the art in surface reconstruction from SDFs at medium and high resolutions, and opens the door for applications in 3D modeling and design.

![Teaser](images/teaser.png)


## Installation

### 1. Clone with submodules

```bash
git clone --recursive https://github.com/xianacarrera/dcsdd.git
cd dcsdd
```

### 2. Set up a Python environment

We recommend [conda](https://docs.conda.io/) with Python 3.13:

```bash
conda create -n dcsdd python=3.13 pip -y
conda activate dcsdd
pip install -r requirements.txt
```

### 3. Build the C++ library and Python bindings

```bash
mkdir -p build
cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
make -j$(sysctl -n hw.ncpu 2>/dev/null || nproc)
cd ..
```

This compiles the core C++ library and places the Python extension module at `src/python/contouring/_contouring_cpp_module*.so`.

### 4. Verify the installation

```bash
python -c "import sys; sys.path.insert(0, '.'); import src.python.contouring as contouring; print('OK')"
```

## Usage

All scripts should be run from the **repository root**.

For convenience, `scripts/run_ours.py` provides a ready-to-use script that runs our method, Dual Contouring, Marching Cubes, RFTA and Kohlbrenner and Alexa [2025a, 2025b] on a given mesh. The script also saves the resulting meshes and prints the runtime for each method.

The main code for our method can be found under `src/`. The figures in the paper can be reproduced by running the scripts in `scripts/`, which will save their outputs in the corresponding subfolders of `results/`. For convenience, these subfolders already contain `.zip` archives with the precomputed results of the scripts.

### Key parameters (`ContouringOptions`)

| Parameter | Default | Description |
|---|---|---|
| `outer_iters` | 100 | Number of iterations in the outer loop |
| `inner_iters` | 100 | Number of iterations in the inner loop (local energy minimization) |
| `hermite_update` | `True` | Whether to refine Hermite positions from the mesh |
| `mu` | 0.1 | Regularization weight |
| `dc_weight` | 0.02 | Weight of the Dual Contouring (Hermite) energy term |
| `new_hermite_pos_weight` | 0.2 | Blend weight for updating Hermite positions |
| `new_hermite_normal_weight` | 0.2 | Blend weight for updating Hermite normals |
| `new_face_pos_weight` | 0.2 | Blend weight for updating face positions |
| `batch_size` | 200000 | Number of SDF grid points processed per batch |
| `verbose` | `False` | Print per-iteration energy values |


## Citation

If you use this code in your research, please cite:

```bibtex
@inproceedings{Carrera2026DCSDD,
  author = {Carrera, Xiana and Wang, Ningna and Batty, Christopher and Stein, Oded and Sell\'{a}n, Silvia},
  title = {Dual Contouring of Signed Distance Data},
  year = {2026},
  isbn = {9798400725548},
  publisher = {Association for Computing Machinery},
  address = {New York, NY, USA},
  url = {https://doi.org/10.1145/3799902.3811116},
  doi = {10.1145/3799902.3811116},
  booktitle = {Proceedings of the Special Interest Group on Computer Graphics and Interactive Techniques Conference Conference Papers},
  articleno = {38},
  numpages = {12},
  series = {SIGGRAPH Conference Papers '26}
}
```

## Issues

Please [email us](mailto:x.carrera@columbia.edu) if you have any questions or issues related to this project.

## Ackwnoledgements

The Geometry and the City lab at Columbia University is supported by generous gifts from nTop, Adobe, Dandy, and Braid Technologies, as well as by a sponsored research project from Dreamsports and the Columbia Engineering Interdisciplinary Research Fund. Christopher Batty acknowledges the generous support from the Natural Sciences and Engineering Research Council of Canada (Grant RGPIN-2021-02524). Oded Stein acknowledges the generous support from the National Science Foundation (award #2335493) and a gift from Adobe.
