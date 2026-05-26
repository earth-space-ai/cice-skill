# CICE Skill

Progressive-disclosure skill for the [CICE Consortium](https://github.com/CICE-Consortium/CICE) sea-ice model, the de facto sea-ice component of CESM, E3SM, NorESM, UFS, and many other coupled systems.

> **Skill author:** Koutian Wu (ktwu01@gmail.com)
> **Skill version:** 0.1.0-scaffold

> ⚠️ **Disclaimer — please read before using this skill.**
> This skill is **not a gold-standard reference**. It is a helper that lowers
> the barrier for new users to **get their hands dirty** with the model. AI
> agents (and the humans drafting this material) make mistakes; commands, file
> paths, namelist options, and physics explanations here can be wrong,
> incomplete, or out of date. **Always cross-check with the official model
> documentation, the source code, and a human expert before trusting any
> output for research, publication, or operational use.**

## What This Is

A guide to standalone and embedded use of CICE. Covers the `cice.setup` case-creation tool, the `Icepack` column-physics submodule, dynamics options (EVP/EAP/VP rheology, incremental-remap advection), the `ice_in` / `icepack_in` namelist split, and the regression test suite.

## Status

Scaffold. Layout and submodule conventions verified against the cloned tree. Operational depth being filled in.

## Related skills in this org

- [cesm-skill](https://github.com/earth-space-ai/cesm-skill)
- [mom6-skill](https://github.com/earth-space-ai/mom6-skill)

## License

MIT (skill content). CICE has its own consortium license; see `LICENSE.pdf` and `DistributionPolicy.pdf` in the upstream repo.
