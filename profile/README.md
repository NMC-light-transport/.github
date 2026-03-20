# NMC — multimodal photon transport Monte Carlo simulations in turbid media

> Developed at Victoria University of Wellington, New Zealand.

---

## What is NMC?

NMC is a power-efficient, open-source Monte Carlo (MC) framework purpose-built for simulating **optical effects** in turbid biological media — *subsurface scattering, propagation of polirized light, spontaneous and stimulated Raman scattering (SRS), etc*.

While classical MC codes (MCML, MCX, mcxyz.c) model linear photon transport — absorption, elastic scattering, and fluorescence — NMC extends the photon physics engine. This includes:

- 🔬 **Polirised light scattering** in tissue-like turbid media
- ⚡ **Spontaneous and Stimulated Raman scattering (SRS)** — coherent, intensity-dependent processes
- 🧠 **Time-resolved transport** with full spectral tracking
- 🍎 **Apple M-series (ARM) optimisation** for energy-efficient high-throughput simulation

NMC is led by [Alexander Doronin](https://github.com/aledoronin) at Victoria University of Wellington, New Zealand.

---

## Core Team

| Name | Role | Affiliation |
|---|---|---|
| **Alexander Doronin** | PI, lead developer | Victoria University of Wellington |
| **Ilya Vladyko** | PhD student, Raman MC engine | Victoria University of Wellington |
| **Vladislav V. Yakovlev** | Collaborator, nonlinear optics | Texas A&M University |

---

## Key Publications

- **Vladyko, Yakovlev, Doronin** (2025) — *"[A Power-Efficient Monte Carlo Framework for Nonlinear Light-Matter Interactions: In Silico Modeling of Spontaneous and Stimulated Raman Scattering](https://www.authorea.com/users/966458/articles/1334839-a-power-efficient-monte-carlo-framework-for-nonlinear-light-matter-interactions-in-silico-modeling-of-spontaneous-and-stimulated-raman-scattering)"*
- **Clennell, Nguyen, Yakovlev, Doronin** (2023) — *"[Neu(t)ralMC: energy-efficient open-source MC algorithm for photon transport](https://doi.org/10.1364/oe.496516)"*, Opt. Express 31(19)
- **Doronin & Meglinski** (2012) — *"[Peer-to-peer Monte Carlo simulation of photon migration](https://doi.org/10.1117/1.jbo.17.9.090504)"*, J. Biomed. Opt. 17(9)
- **Doronin & Meglinski** (2011) — *"[Online object-oriented Monte Carlo computational tool for biomedical optics](https://doi.org/10.1364/BOE.2.002461)"*, Biomed. Opt. Express 2

---

## Project Context: Where NMC Sits in the Field

The field of biomedical Monte Carlo simulation has a rich 40-year lineage. NMC occupies a **specific and novel position** at the frontier of nonlinear transport modelling. To understand what makes NMC distinct, see:

📄 **[`docs/mc-landscape.md`](../docs/mc-landscape.md)** — A comprehensive map of the MC light transport field: founders, active groups, platforms, and where NMC fits among them.

🌐 **[`docs/mc-landscape.html`](../docs/mc-landscape.html)** — Interactive visual version of the same landscape (open in any browser).

---

## Licence

Code released under the [MIT Licence](LICENSE). Documentation under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

---

<sub>Victoria University of Wellington · School of Engineering and Computer Science · Wellington, New Zealand</sub>
