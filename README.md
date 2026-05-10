# CICE Skill

Progressive-disclosure skill for the [CICE Consortium](https://github.com/CICE-Consortium/CICE) sea-ice model, the de facto sea-ice component of CESM, E3SM, NorESM, UFS, and many other coupled systems.

> **Skill author:** Koutian Wu (ktwu01@gmail.com)
> **Skill version:** 0.1.0-scaffold

## What This Is

A guide to standalone and embedded use of CICE. Covers the `cice.setup` case-creation tool, the `Icepack` column-physics submodule, dynamics options (EVP/EAP/VP rheology, incremental-remap advection), the `ice_in` / `icepack_in` namelist split, and the regression test suite.

## Status

Scaffold. Layout and submodule conventions verified against the cloned tree. Operational depth being filled in.

## Related skills in this org

- [cesm-skill](https://github.com/Earth-Space-Modeling-skills/cesm-skill)
- [mom6-skill](https://github.com/Earth-Space-Modeling-skills/mom6-skill)

## License

MIT (skill content). CICE has its own consortium license; see `LICENSE.pdf` and `DistributionPolicy.pdf` in the upstream repo.
