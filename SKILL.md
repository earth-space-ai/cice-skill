---
name: cice
description: >
  Progressive-disclosure skill for the CICE Consortium sea-ice model,
  the de facto sea-ice component of CESM, E3SM, NorESM, UFS, and many
  other coupled climate systems. Covers the cice.setup case-control
  interface, the configuration/ tree, the bundled Icepack column
  physics submodule, history files, namelists, the test suite, and
  the contribution model.
version: 0.1.0-scaffold
tags:
  - earth-science
  - sea-ice
  - climate-model
  - cice
  - icepack
  - cice-consortium
  - fortran
  - mpi
---

# CICE Sea-Ice Model Guide

> **CICE** = Sea-ice model maintained by the CICE Consortium.
> Maintainer: CICE Consortium (multi-institutional)
> Source: https://github.com/CICE-Consortium/CICE
> Column physics submodule: https://github.com/CICE-Consortium/Icepack
> Docs: https://cice-consortium-cice.readthedocs.io
> Forum: https://xenforo.cgd.ucar.edu/cesm/forums/cice-consortium.146/
> Skill author: Koutian Wu (ktwu01@gmail.com)
> Skill version: 0.1.0-scaffold

**What CICE does:** Simulates the growth, melting, advection, and deformation of polar sea ice on a curvilinear grid. Couples to atmosphere and ocean components; provides ice fraction, thickness, surface temperature, ocean–ice fluxes, and atmosphere–ice fluxes. Used in: CESM (via CESM_CICE wrapper), E3SM, NorESM, UFS, GFDL, and many regional configurations.

Used in CESM, E3SM, NorESM, UFS, and other coupled systems. (Note: GFDL uses its own SIS2 sea-ice model, not CICE.)

**Architecture:** CICE is a **two-repo** system. This repo (`CICE`) holds the dynamical/transport core and the case-control machinery. Column-physics (thermodynamics, melt ponds, snow, brine, biogeochemistry) lives in **Icepack**, included as a Git submodule under `icepack/`.

**Coupling note:** standalone CICE includes a driver that reads atm/ocn forcing directly from NetCDF (CORE2, JRA55-do, etc.). CICE does **not** require an external coupler to run. When embedded in CESM/E3SM/etc., the host coupler (CMEPS, MCT) drives it via the standard flux interface.

**Who this skill is for:** Researchers running CICE standalone (often via a host coupler), CICE developers, and people who need to understand how CICE is configured inside CESM/E3SM/NorESM.

---

## Quick Decision Tree

```
"What do I need?"
│
├─ 🆕 What is CICE, what is Icepack, how do they relate?
│  └─ Read: reference/overview.md
│
├─ 🚀 Set up and run a CICE case via cice.setup
│  └─ Read: reference/case-setup.md
│
├─ ❄️ Column physics (Icepack): thermodynamics, melt ponds, snow
│  └─ Read: reference/icepack.md
│
├─ 🌐 Dynamics: advection (incremental remap), rheology (EVP, EAP, VP)
│  └─ Read: reference/dynamics.md
│
├─ 📝 Namelists (ice_in, icepack_in)
│  └─ Read: reference/namelists.md
│
├─ ✅ Test suite (cice.setup --test)
│  └─ Read: reference/testing.md
│
└─ 🐛 Common build/run errors
   └─ Read: reference/debugging.md
```

---

## Repo Layout (verified from clone)

```
CICE/
├── cice.setup           # Top-level case-creation script (Bash)
├── cicecore/            # CICE dycore: dynamics, transport, drivers
├── configuration/       # Build/run configs (machines, environments, namelists)
├── doc/                 # Sphinx documentation source
├── icepack/             # Icepack column physics (Git submodule)
├── COPYRIGHT.pdf
├── LICENSE.pdf          # CICE Consortium license
├── DistributionPolicy.pdf
└── README.md
```

**Important:** if you `git clone` without `--recurse-submodules`, `icepack/` is empty and CICE will not build. Use:

```bash
git clone --recurse-submodules https://github.com/CICE-Consortium/CICE.git
# or, after cloning:
git submodule update --init --recursive
```

---

## Critical Rules

1. **`cice.setup` is the canonical way to build cases.** It scaffolds a case directory with environment, namelist, and run script. Don't invoke compilers directly.
2. **Icepack is a submodule.** Treat it as a separate piece of code with its own version. CICE pins to a specific Icepack commit.
3. **Two namelists:** `ice_in` (CICE dynamics, I/O, forcing) and `icepack_in` (column physics defaults). Both are written by `cice.setup`.
4. **CICE does not couple itself.** It exposes a standardized flux interface that the host coupler (CMEPS in CESM, MCT in older CESM, etc.) drives.
5. **License is bundled as PDF.** See `LICENSE.pdf` and `DistributionPolicy.pdf`. Read before redistributing.

---

## Reference Index

| File | Topic |
|---|---|
| reference/overview.md | CICE + Icepack relationship |
| reference/case-setup.md | cice.setup, environment, machines |
| reference/icepack.md | Column physics, melt ponds, snow |
| reference/dynamics.md | Advection, EVP/EAP/VP rheology |
| reference/namelists.md | ice_in, icepack_in |
| reference/testing.md | cice.setup --test, regression |
| reference/debugging.md | Common errors |

## Critical agent gotchas (Gemini-reviewed)

- **This skill targets CICE6.** Older CICE5 had a different layout and was usually built through the host model (e.g., CESM); the `cice.setup` workflow described here is CICE6+.
- **`cice.setup` requires `-p` (PE count) and `-m` (machine).** Without them the case scaffolding will not produce a runnable script.
- **Block decomposition is configurable.** `block_size_x` and `block_size_y` in the namelist set the per-block size; bad values cause poor scaling or load imbalance.
- **`ncat` (number of ice thickness categories)** is the defining ITD parameter. Changing it can require namelist changes elsewhere; check defaults per CICE tag.
- **Grid type matters.** CICE6 supports both **B-grid** (legacy) and **C-grid** (newer default in some configurations). The `kdyn` and grid input files must agree.
- **Restart pointers.** The model uses an `ice.restart_file` pointer; mismanaging the pointer (binary vs NetCDF format mismatch, missing file) is the single most common cause of automated chained-job failures.
- **Rheology selection in practice.** EVP and mEVP are the workhorses; EAP is research-grade; VP is rarely used at production scale due to scalability.

## Status

Scaffold (v0.1.0-scaffold). Layout, submodule structure, and case-setup model verified against the cloned tree, with Gemini critique pass on 2026-05-09 to remove the GFDL claim and add coupling and decomposition notes. Operational depth being filled in.
