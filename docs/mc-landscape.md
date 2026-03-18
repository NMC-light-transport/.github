# Monte Carlo Light Transport in Biological Tissue — Field Map

> *A guide to the research groups, software platforms, and scientific lineage of Monte Carlo photon transport simulation — and where the NMC project fits within this landscape.*

---

## Contents

1. [Why Monte Carlo?](#why-monte-carlo)
2. [The Founding Lineage: MCML (1985–1995)](#the-founding-lineage-mcml-19851995)
3. [Generation 2: GPU Acceleration and 3D Voxel Codes (2002–2014)](#generation-2-gpu-acceleration-and-3d-voxel-codes-20022014)
4. [Active Platforms Today](#active-platforms-today)
5. [Online and Cloud-Based Simulators](#online-and-cloud-based-simulators)
6. [The Nonlinear Frontier: Raman and Beyond](#the-nonlinear-frontier-raman-and-beyond)
7. [NMC: Position and Contribution](#nmc-position-and-contribution)
8. [Lineage Tree (Text)](#lineage-tree-text)
9. [Key References](#key-references)

---

## Why Monte Carlo?

Biological tissue is an extraordinarily complex optical medium. Photons launched into skin, brain, or other tissue undergo millions of scattering events before being absorbed, re-emitted, or escaping. Analytical solutions — such as the diffusion approximation or the Kubelka–Munk model — work only in specific regimes (e.g., thick, highly scattering tissue with no sharp boundaries).

**Monte Carlo (MC) simulation** treats photon transport as a statistical sampling problem. Each photon packet is tracked step by step: it travels a random path length, scatters at a random angle, and deposits a fraction of its weight as it goes. By simulating millions of such packets, accurate fluence distributions, reflectance, and transmittance can be obtained for *any* geometry and *any* set of optical properties.

The trade-off is computational cost — which is why the field's history is, in large part, a story of **making MC fast enough to be useful** (GPU acceleration, parallel computing, smart variance reduction), and **making MC general enough** to model new physics.

---

## The Founding Lineage: MCML (1985–1995)

The canonical biomedical MC code — **mcml.c** — emerged from a series of contributions between 1985 and 1995. This history is documented in the primary source: [Jacques (2022), J. Biomed. Opt. 27(8)](https://doi.org/10.1117/1.JBO.27.8.083002).

### Steven L. Jacques — The First Code (1985)

In 1985, **Steven L. Jacques** wrote the first MC code for tissue optics while working at the **Wellman Center for Photomedicine, Massachusetts General Hospital, Boston**. The code sampled an exponential probability for photon step size and used a lookup-table approach for scattering functions. It was compared against experiments on *ex vivo* skin. Jacques is now an affiliate professor of bioengineering at the University of Washington, Seattle.

### Marleen Keijzer — HOP / DROP / SPIN / CHECK (1989)

**Marleen Keijzer**, a graduate student from Delft University working in the Jacques lab, wrote the second version. She introduced the four-step photon logic — **HOP, DROP, SPIN, CHECK** — that still underlies all descendants of mcml.c. She modelled aorta as a multi-layer tissue and demonstrated fluorescence collection from turbid media. (Her original step names were in Dutch.) She is now a professor at the Delft Institute of Applied Mathematics, TU Delft.

### Scott A. Prahl — Cartesian Coordinates and HG Scattering (1989)

**Scott A. Prahl**, a graduate student at UT Austin in the lab of **A. J. Welch**, wrote the third version. He simplified Keijzer's cylindrical coordinate system to **Cartesian coordinates** and introduced **analytic sampling of the Henyey–Greenstein (HG) scattering function** — the approach still used in virtually all tissue MC codes today. In 1996, when the Jacques lab moved to the Oregon Medical Laser Center (OMLC), Prahl created [omlc.org/software/mc](https://omlc.org/software/mc), the most widely used tissue-optics MC repository. Prahl is a professor of electrical engineering at the Oregon Institute of Technology.

### Lihong V. Wang — Definitive MCML and the First Public Website (1992)

**Lihong V. Wang**, as a postdoc in the Jacques lab at UT MD Anderson Cancer Center, wrote the fourth and definitive version of mcml.c in 1992. He reorganised the code for multiple planar layers with unique optical properties (including refractive index) per layer, made it input-file driven, wrote a **173-page manual**, and launched the **first publicly accessible MC website** at UTMDACC. He was first author of the canonical paper:

> Wang L.V., Jacques S.L., Zheng L. (1995). *MCML — Monte Carlo modeling of photon transport in multi-layered tissues.* Comput. Methods Prog. Biomed. **47**, 131–146.

This paper has been cited over **4,000 times** as of 2022 [(Jacques 2022)](#key-references). Wang is now a professor of medical engineering and electrical engineering at **Caltech**, where he leads the Caltech Optical Imaging Laboratory (COIL).

### The MCML Founding Team at a Glance

| Person | Contribution | Institution |
|---|---|---|
| Steven L. Jacques | First code (1985), framework, supervision | MGH Wellman / UTMDACC / OMLC |
| Marleen Keijzer | HOP/DROP/SPIN/CHECK, multi-layer, fluorescence | Delft → Jacques lab |
| Scott A. Prahl | Cartesian coords, HG sampling, omlc.org | UT Austin (Welch lab) → OMLC |
| Lihong V. Wang | Definitive C code, 173p manual, 1st website | UTMDACC → Caltech |

---

## Generation 2: GPU Acceleration and 3D Voxel Codes (2002–2014)

### David A. Boas — tMCimg and the 3D Voxel Concept (2002)

**David A. Boas** at MGH/Harvard published **tMCimg** in 2002: the first publicly available 3D voxel-based MC code, capable of simulating photon migration through complex heterogeneous media including the adult human head. tMCimg is the direct ancestor of both **mcxyz.c** (Jacques/Li Ting, 2014) and **MCX** (Fang, 2009).

> Boas D.A., Culver J.P., Stott J.J., Dunn A.K. (2002). *Three dimensional Monte Carlo code for photon migration through complex heterogeneous media including the adult human head.* Opt. Express **10**(3), 159–170.

### Erik Alerstam — First GPU MCML (2008)

**Erik Alerstam**, **Tomas Svensson**, and **Stefan Andersson-Engels** at Lund University published the first **CUDA GPU-accelerated** version of mcml.c in 2008. This was the proof-of-concept that GPU parallelisation could make MC orders of magnitude faster.

> Alerstam E., Svensson T., Andersson-Engels S. (2008). *Parallel computing with graphics processing units for high-speed Monte Carlo simulation of photon migration.* J. Biomed. Opt. **13**(6), 060504.

### Jessica Ramella-Roman — Polarised Light MC (2005)

**Jessica Ramella-Roman**, a PhD student in the Jacques lab at OHSU, collaborated with Prahl to create a polarised-light version of mcml.c in 2005. The code tracks the full **Stokes vector** of photons. A GPU-accelerated version (pMCX) was later incorporated into MCX by Yan, Jacques, Ramella-Roman, and Fang (2022). Ramella-Roman is now a professor of biomedical engineering at Florida International University.

### Alexander Doronin — GPU mcxyz.c (c. 2012)

**Alexander Doronin**, as a postdoc in **Igor Meglinski's lab** at the University of Otago, New Zealand, implemented a **CUDA GPU-accelerated version of mcxyz.c** and shared it publicly at [github.com/aledoronin](https://github.com/aledoronin). This work is explicitly documented in [Jacques (2022)](#key-references). Doronin is now an assistant/senior lecturer in the School of Engineering and Computer Science at Victoria University of Wellington — the institutional home of the NMC project.

### Li Ting + Yin-chu Chen → mcxyz.c (2014)

**Li Ting** (postdoc, Jacques lab at OHSU, inspired by Boas's tMCimg) and **Yin-chu Chen** (graduate student, Prahl lab, 2005) wrote separate voxelated versions of mcml.c that were combined into the current **mcxyz.c** — a 3D voxel code where each voxel can have unique optical properties.

### A. Phong Tran — Curved Boundaries in Voxel MC (2020)

**A. Phong Tran** (graduate student, Fang lab at Northeastern, in collaboration with Jacques) modified mcxyz.c to allow unique refractive indices per voxel, with smooth pre-processed boundaries to handle refraction correctly at oblique interfaces.

> Tran A.P., Jacques S.L. (2020). *Modeling voxel-based Monte Carlo light transport with curved and oblique boundary surfaces.* J. Biomed. Opt. **25**(2), 025001.

### MCmatlab — DTU Fotonik (2018)

**Dominik Marti**, **Rikke N. Aasbjerg**, **Peter E. Andersen**, and **Anders K. Hansen** at the Technical University of Denmark developed **MCmatlab**: a MATLAB-integrated parallel version of mcxyz.c designed as a teaching tool. It adds a **heat diffusion simulator** and **Arrhenius tissue damage model**. The code runs parallel processors, achieving approximately 8× speedup over mcxyz.c on a single CPU.

> Marti D., Aasbjerg R.N., Andersen P.E., Hansen A.K. (2018/2020). *MCmatlab: open-source, MATLAB-integrated 3D Monte Carlo light transport solver with heat diffusion and tissue damage.* J. Biomed. Opt. **23**(12), 121622.

---

## Active Platforms Today

### MCX and MMC — Qianqian Fang, Northeastern University

**Qianqian Fang** leads the **Computational Optics and Translational Imaging (COTI) Lab** at Northeastern University, Boston. His group maintains the most feature-complete open-source MC platform currently available:

- **MCX** — GPU-accelerated voxel-based MC (CUDA and OpenCL)
- **MMC** — mesh-based MC using Plücker coordinates
- **pMCX** — GPU-accelerated polarised MC (co-developed with Ramella-Roman and Jacques)
- **MCX Cloud / mcxspace** — browser-accessible cloud MC (no installation)

Fang earned his PhD at Dartmouth and worked in **David Boas's lab** at MGH Martinos Center before joining Northeastern. His 2009 foundational paper demonstrated a 400× speedup over tMCimg.

Key team members: **Shijie Yan**, **Yaoshen Yuan**, **Leiming Yu**, **A. Phong Tran** (shared with Jacques), **Fanny Nina-Paravecino**.

| Code | Repo | Features |
|---|---|---|
| MCX | [mcx.space](https://mcx.space) | CUDA, OpenCL, heterogeneous geometry |
| MMC | [mcx.space/wiki/MMC](https://mcx.space/wiki/index.cgi?MMC) | Mesh-based, Plücker ray tracing |
| pMCX | included in MCX | Full Stokes polarisation |
| MCX Cloud | [mcx.space/cloud](https://mcx.space/cloud) | Browser-based, free |

### VTS / MCCL — Venugopalan & Spanier, UC Irvine BLI

The **Virtual Photonics Technology Initiative (VTS)** is based at the **Beckman Laser Institute (BLI)** at UC Irvine, supported by NIH as part of the LAMMP P41 research resource.

Key people:
- **Vasan Venugopalan** (PI, Prof. Chemical & Biomolecular Engineering, UCI)
- **Jerome Spanier** (co-founder, mathematician; Claremont Graduate Univ. / BLI; d. 2024)
- **Carole K. Hayakawa** (lead developer; PhD under Spanier; breakthrough in perturbation/differential MC)
- **David J. Cuccia** (ported code to C#, launched MCCL in 2010; now Modulim Inc.)

The original BLI MC code was written in 1998 by **Andy Dunn and Derek Smithies**. Hayakawa's PhD work on **perturbation MC** and **differential MC** — enabling real-time sensitivity calculations — became the scientific heart of the platform.

> Hayakawa C.K., Malenfant L., Ranasinghesagara J.C., Cuccia D.J., Spanier J., Venugopalan V. (2022). *MCCL: an open-source software application for Monte Carlo simulations of radiative transport.* J. Biomed. Opt. **27**(8), 083005.

### Fabrizio Martelli — Diffuse Optics Theory, Univ. Florence

**Fabrizio Martelli** (Assoc. Professor, Dept. of Physics and Astronomy, Univ. of Florence) heads a group specialising in **analytical solutions to the radiative transport equation** and their MC validation. His collaborators include **Angelo Sassaroli** (Tufts), **Samuele Del Bianco** (CNR), **Tiziano Binzoni** (Univ. Geneva), and former mentor **Giovanni Zaccanti** (Florence).

Their textbook — *Light Propagation through Biological Tissue and Other Diffusive Media* (SPIE Press, 1st ed. 2009, 2nd ed. 2022) — is one of the field's primary reference texts. The group recently extended to **Raman signal modelling** in diffusive media and co-organised the JBO Special Section "30 Years of Open-Source MC Codes" (2022) with Fang and Lilge.

---

## Online and Cloud-Based Simulators

Several groups have made MC available without installation:

| Platform | Group | URL | Notes |
|---|---|---|---|
| **MCX Cloud** | Fang / Northeastern | [mcx.space/cloud](https://mcx.space/cloud) | JSON-input, GPU cluster |
| **VTS online** | Venugopalan / UCI | [virtualphotonics.org](https://virtualphotonics.org) | C# MCCL, GUI |
| **Multi-Scattering** | Jönsson & Berrocal / Lund | [multi-scattering.com](https://www.multi-scattering.com) | GPU cluster, tissue + spray |
| **MCML online** | omlc.org | [omlc.org/software/mc](https://omlc.org/software/mc) | Prahl-maintained archive |

### Multi-Scattering — Jönsson & Berrocal, Lund University

**Edouard Berrocal** (PhD, Cranfield 2006; now Div. Combustion Physics, Lund) and **Joakim Jönsson** (PhD student) developed **Multi-Scattering**: a free online GPU-accelerated MC platform. It achieves **up to 800× acceleration** over CPU using four GPU cards, capable of simulating **1 billion photons in 10 seconds** at optical depth 10. It handles voxelated volumes and is validated for both spray diagnostics and biological tissue.

The Berrocal–Meglinski collaboration (2005) connects this platform's lineage to the Doronin/NMC branch through Igor Meglinski.

> Jönsson J., Berrocal E. (2020). *Multi-Scattering software. Part I: online accelerated Monte Carlo simulation of light scattering from a 3D object.* Opt. Express **28**(25).

---

## The Nonlinear Frontier: Raman and Beyond

Classical MC codes model **linear** photon transport: absorption, elastic scattering, and (sometimes) fluorescence. The emerging frontier addresses **nonlinear** and **inelastic** interactions.

### Spontaneous Raman MC

The first MC model specifically designed for **Raman scattering in tissue** was published by **Periyasamy and Pramanik** (IISc/NTU Singapore, 2015). Their **RMCC** code extended MCML to include inelastic Raman scattering events for embedded geometrical objects (sphere, cylinder, ellipsoid, cuboid) — the so-called **MCML-EO** framework.

> Periyasamy V., Pramanik M. (2015). *Experimental validation of Raman Monte Carlo simulations for a cuboid object to detect constituent layers.* J. Raman Spectrosc. **46**(7).

**Dumont, Fang, and Patil** (Temple University, 2021) later extended MCX with the **isoweight** Raman algorithm, achieving ~100× speedup for Raman MC via GPU parallelisation. Fang is co-author, making this a direct MCX extension.

**Akbarzadeh, Edjlali, Leblond et al.** (Polytechnique Montréal, 2020) developed a full **spectroscopic MC** package combining absorption, fluorescence, elastic, and inelastic (Raman) scattering for depth-resolved Raman sensing in tissue.

### Stimulated Raman Scattering (SRS) MC

The first time-dependent MC model for **stimulated Raman scattering** in turbid media was published by **Hokr, Yakovlev, and Scully** (Texas A&M, 2014). SRS is a coherent, intensity-dependent nonlinear process — the first genuinely nonlinear interaction to be incorporated into a tissue MC framework.

> Hokr B.H., Yakovlev V.V., Scully M.O. (2014). *Efficient time-dependent Monte Carlo simulations of stimulated Raman scattering in a turbid medium.* ACS Photonics **1**(12).

### Raman MC Groups at a Glance

| Group | Key contribution |
|---|---|
| Periyasamy / Pramanik (IISc → Iowa State) | MCML-EO + RMCC; geometry + spontaneous Raman MC |
| Dumont / Patil (Temple) + Fang collab. | isoweight GPU Raman MC via MCX |
| Akbarzadeh / Leblond (Polytechnique Montréal) | Full spectroscopic MC (elastic + fluorescence + Raman) |
| Hokr / Yakovlev (Texas A&M) | First SRS MC in turbid tissue |
| **Vladyko / Doronin / Yakovlev (VUW)** | **NMC: unified spontaneous + SRS MC framework** |

---

## NMC: Position and Contribution

The NMC project, developed by **Alexander Doronin's group at Victoria University of Wellington**, occupies a unique position in this landscape:

### What NMC does differently

1. **Unified nonlinear MC framework.** NMC is designed from the ground up to handle both spontaneous and stimulated Raman scattering in a single photon transport engine — not as an add-on to a linear code.

2. **Power-efficient computation.** The **Neu(t)ralMC** iteration (Clennell, Nguyen, Yakovlev, Doronin 2023) specifically targets **Apple M-series processors** (unified memory ARM architecture), enabling high-throughput simulation on low-power hardware without a dedicated NVIDIA GPU.

3. **Direct lineage from mcxyz.c.** Doronin's GPU mcxyz.c (implemented during his postdoc in Meglinski's lab at Otago, c. 2012) is acknowledged by Jacques (2022) as a key extension of the foundational code. NMC is the natural evolution of that work.

4. **Collaborative nonlinear physics.** The collaboration with **Vladislav Yakovlev** (Texas A&M) — who with Hokr and Scully published the first SRS MC model in 2014 — brings the deepest available expertise in nonlinear Raman physics into the MC framework.

5. **Object-oriented and peer-to-peer MC.** Earlier work (Doronin & Meglinski 2011, 2012) introduced object-oriented and distributed/P2P MC architectures that underpin NMC's modularity.

### Gap NMC fills

| Capability | MCML | MCX | MCmatlab | NMC |
|---|---|---|---|---|
| Multi-layer tissue | ✅ | ✅ | ✅ | ✅ |
| 3D voxel geometry | — | ✅ | ✅ | ✅ |
| GPU acceleration | — | ✅ | partial | ✅ |
| Polarisation | (ext.) | ✅ pMCX | — | ✅ |
| Fluorescence | — | — | — | planned |
| Spontaneous Raman | — | partial (Dumont ext.) | — | **✅ core** |
| Stimulated Raman (SRS) | — | — | — | **✅ core** |
| Apple M-series ARM opt. | — | — | — | **✅** |
| Energy-efficient design | — | — | — | **✅** |

---

## Lineage Tree (Text)

```
1985  Steven Jacques (Wellman/MGH) ─── first MC code for tissue
  │
1989  Marleen Keijzer (Delft → Jacques lab) ─── HOP/DROP/SPIN/CHECK
  │
1989  Scott Prahl (UT Austin, Welch lab) ─── Cartesian + HG scattering
  │
1992  Lihong V. Wang (UTMDACC, Jacques lab) ─── definitive MCML C code
  │                                              + first public website
  │
1995  Wang, Jacques, Zheng ─── MCML paper (4000+ citations)
  │
  ├── 2002  David A. Boas (MGH) ─── tMCimg (3D voxel, heterogeneous)
  │     │
  │     └── 2009  Qianqian Fang + Boas ─── MCX (GPU, CUDA)
  │                 └── 2021  Dumont / Patil / Fang ─── isoweight Raman MC
  │
  ├── 2005  Jessica Ramella-Roman (OHSU, Jacques lab) ─── polarised MC
  │     └── 2022  Yan / Fang / Ramella-Roman / Jacques ─── pMCX (GPU polarised)
  │
  ├── 2008  Erik Alerstam (Lund, Andersson-Engels) ─── GPU-MCML (CUDA, first)
  │
  ├── 2014  Li Ting + Chen (Jacques/Prahl labs) ─── mcxyz.c (voxel)
  │     │
  │     ├── 2012  Alex Doronin (Otago, Meglinski lab) ─── GPU mcxyz.c (CUDA)
  │     │     └── 2019–  NMC @ Victoria Univ. Wellington
  │     │           ├── Spontaneous Raman MC
  │     │           ├── Stimulated Raman (SRS) MC ←── Hokr/Yakovlev 2014
  │     │           └── Neu(t)ralMC / Apple M-series (2023)
  │     │
  │     └── 2018  Marti / Andersen / Hansen (DTU) ─── MCmatlab
  │
  └── 2014  Hokr / Yakovlev / Scully (Texas A&M) ─── SRS MC (first)
              └── (collaborates with NMC, 2023–)

[Independent lines]
  VTS / MCCL (UCI BLI, 1998–) ─── Venugopalan / Spanier / Hayakawa
  Multi-Scattering (Lund, 2020) ─── Berrocal / Jönsson
  Martelli group (Florence, 1997–) ─── diffuse optics theory + MC reference
```

---

## Key References

1. **Jacques S.L.** (2022). *History of Monte Carlo modeling of light transport in tissues using mcml.c.* J. Biomed. Opt. **27**(8), 083002. [doi:10.1117/1.JBO.27.8.083002](https://doi.org/10.1117/1.JBO.27.8.083002)

2. **Wang L.V., Jacques S.L., Zheng L.** (1995). *MCML — Monte Carlo modeling of photon transport in multi-layered tissues.* Comput. Methods Prog. Biomed. **47**, 131–146.

3. **Fang Q., Boas D.A.** (2009). *Monte Carlo simulation of photon migration in 3D turbid media accelerated by graphics processing units.* Opt. Express **17**(22), 20178–20190.

4. **Alerstam E., Svensson T., Andersson-Engels S.** (2008). *Parallel computing with graphics processing units for high-speed Monte Carlo simulation of photon migration.* J. Biomed. Opt. **13**(6), 060504.

5. **Marti D., Aasbjerg R.N., Andersen P.E., Hansen A.K.** (2018). *MCmatlab: an open-source, user-friendly, MATLAB-integrated three-dimensional Monte Carlo light transport solver with heat diffusion and tissue damage.* J. Biomed. Opt. **23**(12), 121622.

6. **Hayakawa C.K., Malenfant L., Ranasinghesagara J.C., Cuccia D.J., Spanier J., Venugopalan V.** (2022). *MCCL: an open-source software application for Monte Carlo simulations of radiative transport.* J. Biomed. Opt. **27**(8), 083005.

7. **Hokr B.H., Yakovlev V.V., Scully M.O.** (2014). *Efficient time-dependent Monte Carlo simulations of stimulated Raman scattering in a turbid medium.* ACS Photonics **1**(12).

8. **Periyasamy V., Pramanik M.** (2015). *Experimental validation of Raman Monte Carlo simulations for a cuboid object.* J. Raman Spectrosc. **46**(7).

9. **Jönsson J., Berrocal E.** (2020). *Multi-Scattering software. Part I.* Opt. Express **28**(25).

10. **Doronin A., Meglinski I.** (2011). *Online object-oriented Monte Carlo computational tool for biomedical optics.* Biomed. Opt. Express **2**.

11. **Doronin A., Meglinski I.** (2012). *Peer-to-peer Monte Carlo simulation of photon migration.* J. Biomed. Opt. **17**(9).

12. **Clennell B., Nguyen T., Yakovlev V.V., Doronin A.** (2023). *Neu(t)ralMC: open-source energy-efficient Monte Carlo algorithm for photon transport.* Opt. Express **31**(19).

13. **Vladyko I., Yakovlev V.V., Doronin A.** (2025). *Monte Carlo modelling of Raman signal enhancement using Apple M-series processors.* SPIE BiOS 2025.

---

*Document maintained by the NMC project team, Victoria University of Wellington.*
*Last updated: 2025.*
