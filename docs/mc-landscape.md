# Monte Carlo Light Transport in Biological Tissue — Field Map

> *A comprehensive guide to the research groups, software platforms, and scientific lineage of Monte Carlo photon transport simulation — and where the NMC project fits within this landscape.*
>
> Primary source: [Jacques S.L. (2022), J. Biomed. Opt. 27(8), 083002](https://doi.org/10.1117/1.JBO.27.8.083002)

---

## Contents

1. [Why Monte Carlo?](#1-why-monte-carlo)
2. [The Founding Lineage: MCML (1985–1995)](#2-the-founding-lineage-mcml-19851995)
3. [Generation 2: GPU Acceleration and 3D Voxel Codes (2002–2020)](#3-generation-2-gpu-acceleration-and-3d-voxel-codes-20022020)
4. [The Nonlinear Frontier: Raman and Beyond](#4-the-nonlinear-frontier-raman-and-beyond)
5. [Active Platforms Today](#5-active-platforms-today)
6. [NMC: Position and Contribution](#6-nmc-position-and-contribution)
7. [Lineage Tree](#7-lineage-tree)
8. [Key References](#8-key-references)

---

## 1. Why Monte Carlo?

Biological tissue is an extraordinarily complex optical medium. Photons launched into skin, brain, or other tissue undergo millions of scattering events before being absorbed, re-emitted, or escaping. Analytical solutions — the diffusion approximation, the Kubelka–Munk model — work only in specific regimes.

**Monte Carlo (MC) simulation** treats photon transport as a statistical sampling problem. Each photon packet is tracked step by step: it travels a random path length, scatters at a random angle, and deposits a fraction of its weight. By simulating millions of such packets, accurate fluence distributions, reflectance, and transmittance can be obtained for *any* geometry and *any* set of optical properties.

The trade-off is computational cost — which is why the field's history is a story of **making MC fast enough to be useful** (GPU acceleration, parallel computing, variance reduction), and **making MC general enough** to model new physics: polarisation, fluorescence, and nonlinear Raman interactions.

---

## 2. The Founding Lineage: MCML (1985–1995)

> Brian C. Wilson & G. Adam - first MC for biological tissues (1983)
> published the first MC model for tissue in 1983, treating tissue as homogeneous with isotropic scattering — the conceptual breakthrough that established the feasibility of the approach. [doi:10.1118/1.595361](https://doi.org/10.1118/1.595361).

The canonical biomedical MC code — **mcml.c** — emerged from a series of contributions between 1985 and 1995. The definitive historical account is [Jacques (2022)](#9-key-references).

### Steven L. Jacques — mcml.c (1985)

In 1985, **Steven L. Jacques** wrote **mcml.c** — the first MC code for tissue optics — while working as a research associate at the **Wellman Center for Photomedicine, Massachusetts General Hospital, Boston**. His code sampled an exponential probability for the photon step size. Jacques implemented a **lookup-table approach** to encoding various scattering functions and explored their effect on observable total and angular reflectance and transmittance for comparison with experiments on *ex vivo* skin.

### Marleen Keijzer — HOP / DROP / SPIN / CHECK (1989)

**Marleen Keijzer**, a graduate student from Delft University working in the Jacques lab, wrote the second version. She introduced the four-step photon logic — **HOP, DROP, SPIN, CHECK** — that underlies all descendants of mcml.c. She implemented a model of aorta tissue (epithelium, mucosa, submucosa) and demonstrated the delivery of light into a multi-layered tissue. In a companion work, she demonstrated the delivery of excitation light into aorta and the collection of fluorescence from the tissue.

> Keijzer M., Jacques S.L., Prahl S.A., Welch A.J. (1989). *Light distributions in artery tissue: Monte Carlo simulations for finite-diameter laser beams.* Lasers Surg. Med. **9**, 148–154. [doi:10.1002/lsm.1900090210](https://doi.org/10.1002/lsm.1900090210)
>
> Keijzer M. et al. (1989). *Fluorescence spectroscopy of turbid media: autofluorescence of the human aorta.* Appl. Opt. **28**(20), 4286–4292. [doi:10.1364/ao.28.004286](https://doi.org/10.1364/ao.28.004286)

### Scott A. Prahl — Variable Stepsize, Mismatched Boundaries, Cartesian Coordinates (1989)

**Scott A. Prahl**, a graduate student at UT Austin in the lab of **A. J. Welch**, wrote the third version. A **variable-stepsize weighted Monte Carlo model** was implemented. Both **mismatched boundary conditions** and **anisotropic scattering** were included, significantly increasing the realism of the model. He also simplified the code by using **Cartesian coordinates** and introduced **analytic sampling of the Henyey–Greenstein (HG) scattering function**.

> Prahl S.A., Keijzer M., Jacques S.L., Welch A.J. (1989). *A Monte Carlo model of light propagation in tissue.* Proc. SPIE IS5, 102–111. [doi:10.1117/12.2283590](https://doi.org/10.1117/12.2283590)

Later, in 1996 when the Jacques lab moved to the **Oregon Medical Laser Center (OMLC)**, Prahl created [omlc.org/software/mc](https://omlc.org/software/mc) — the **first public website** for sharing the MC code, and to this day the most widely used tissue-optics MC repository.

### Lihong V. Wang — Definitive MCML (1992)

**Lihong V. Wang**, as a postdoc in the Jacques lab at **UT MD Anderson Cancer Center**, wrote the fourth and definitive version of mcml.c in 1992. He reorganised the code so that it handled **multiple planar layers**, each with unique optical properties including refractive index. The pre-compiled code used an input file to specify a particular simulation, which was very convenient and greatly helped many to use the code. He wrote a **173-page manual** ([omlc.org/software/mc/man_mcml.pdf](https://omlc.org/software/mc/man_mcml.pdf)).

> Wang L.V., Jacques S.L., Zheng L. (1995). *MCML — Monte Carlo modeling of photon transport in multi-layered tissues.* Comput. Methods Prog. Biomed. **47**, 131–146. [doi:10.1016/0169-2607(95)01640-F](https://doi.org/10.1016/0169-2607(95)01640-F)

Wang is now a professor of medical engineering and electrical engineering at **Caltech (COIL lab)**.

### The MCML Founding Team

| Person | Key contribution | Institution |
|---|---|---|
| Steven L. Jacques | First code (1985), lookup-table scattering, framework & supervision | MGH Wellman → UTMDACC → OMLC/OHSU → Univ. Washington |
| Marleen Keijzer | HOP/DROP/SPIN/CHECK logic; multi-layer model; fluorescence MC | Delft Univ. → Jacques lab → TU Delft (Prof.) |
| Scott A. Prahl | Variable stepsize; mismatched boundaries; Cartesian coords; HG sampling; omlc.org (first public website) | UT Austin (Welch lab) → OMLC → Oregon Tech (Prof.) |
| Lihong V. Wang | Definitive MCML C code; 173-page manual; multi-layer + refractive index; input-file driven | UTMDACC → Washington Univ. → Caltech (Prof., COIL) |

---

## 3. Generation 2: GPU Acceleration and 3D Voxel Codes (2002–2020)

Building on MCML, a series of major extensions introduced GPU acceleration, 3D heterogeneous geometry, polarisation, and new code architectures.

### David A. Boas — tMCimg, 3D Voxel MC (2002)

**David A. Boas** at MGH/Harvard published **tMCimg**: the first publicly available 3D voxel-based MC code, capable of simulating photon migration through complex heterogeneous media including the adult human head. tMCimg became the direct conceptual ancestor of both mcxyz.c (2014) and MCX (2009).

> Boas D.A., Culver J.P., Stott J.J., Dunn A.K. (2002). *Three dimensional Monte Carlo code for photon migration through complex heterogeneous media including the adult human head.* Opt. Express **10**(3), 159–170.

### Erik Alerstam et al. — First GPU MCML (2008)

**Erik Alerstam**, **Tomas Svensson**, and **Stefan Andersson-Engels** at Lund University published the first **CUDA GPU-accelerated** version of mcml.c in 2008 — the proof of concept for orders-of-magnitude MC speedup. Explicitly cited by [Jacques (2022)](#9-key-references) as the first GPU extension of mcml.c.

> Alerstam E., Svensson T., Andersson-Engels S. (2008). *Parallel computing with graphics processing units for high-speed Monte Carlo simulation of photon migration.* J. Biomed. Opt. **13**(6), 060504.

### Qianqian Fang — MCX, GPU-Accelerated Voxel MC (2009)

**Qianqian Fang** (PhD, Dartmouth → MGH Martinos Center, David Boas lab → Northeastern Univ.) developed **MCX**: the most widely adopted GPU-accelerated 3D voxel MC platform. Whether primarily inspired by MCML or Boas's tMCimg, MCX can be traced to both — Fang worked directly in the Boas lab, and MCX's voxel architecture mirrors tMCimg while the photon physics inherits from MCML. The 2009 paper demonstrated a ~400× speedup.

> Fang Q., Boas D.A. (2009). *Monte Carlo simulation of photon migration in 3D turbid media accelerated by graphics processing units.* Opt. Express **17**(22), 20178–20190.

### Jessica Ramella-Roman — Polarised MC (2005)

**Jessica Ramella-Roman** (PhD, OHSU, Jacques lab) created a polarised-light MC code tracking the full **Stokes vector**. A GPU version (pMCX) was later incorporated into MCX by Yan, Jacques, Ramella-Roman, and Fang (2022). She is now a professor at Florida International University.

> Ramella-Roman J.C., Prahl S.A., Jacques S.L. (2005). *Three Monte Carlo programs of polarized light transport into scattering media: part I & II.* Opt. Express **13**(12) and **13**(25).

### Alexander Doronin — GPU mcxyz.c (2012)

**Alexander Doronin**, as a postdoc in **Igor Meglinski's lab** at the University of Otago, New Zealand, implemented a **CUDA GPU-accelerated version of mcxyz.c** — explicitly cited in [Jacques (2022)](#9-key-references) as a key extension. Code shared at [github.com/aledoronin](https://github.com/aledoronin). Doronin is now a senior lecturer at Victoria University of Wellington — the institutional home of the **NMC project**.

### Li Ting + Yin-chu Chen - mcxyz.c (2014)

**Li Ting** (postdoc, Jacques lab, OHSU; inspired by Boas's tMCimg) and **Yin-chu Chen** (graduate student, Prahl lab, 2005) wrote separate voxelated codes that were combined into the current **mcxyz.c**: a 3D voxel code where each voxel can have unique optical properties.

### Andersen & Marti, DTU Fotonik — MCmatlab (2018)

**Peter E. Andersen**, **Anders K. Hansen**, and **Dominik Marti** at the Technical University of Denmark (DTU Fotonik) developed **MCmatlab**: a MATLAB-integrated parallel version of mcxyz.c for teaching. Adds a **heat diffusion simulator** and **Arrhenius tissue damage model**. Runs parallel processors (~8× faster than mcxyz.c on a single CPU). Explicitly cited by Jacques (2022).

> Marti D., Aasbjerg R.N., Andersen P.E., Hansen A.K. (2018). *MCmatlab: open-source, MATLAB-integrated 3D Monte Carlo light transport solver with heat diffusion and tissue damage.* J. Biomed. Opt. **23**(12), 121622.

### A. Phong Tran — Curved Boundaries in Voxel MC (2020)

**A. Phong Tran** (graduate student, Fang lab at Northeastern, in collaboration with Jacques) modified mcxyz.c to allow unique refractive indices per voxel, with smooth pre-processed boundaries for correct refraction at oblique interfaces.

> Tran A.P., Jacques S.L. (2020). *Modeling voxel-based Monte Carlo light transport with curved and oblique boundary surfaces.* J. Biomed. Opt. **25**(2), 025001.

### Generation 2 Summary

| Person / Group | Key contribution | Institution |
|---|---|---|
| David A. Boas | tMCimg — first 3D voxel MC (2002) | MGH / Harvard |
| Erik Alerstam et al. | First GPU-MCML (CUDA, 2008) | Lund Univ. (Andersson-Engels lab) |
| Qianqian Fang | MCX — GPU 3D voxel MC (2009); MMC (2010) | Dartmouth → MGH → Northeastern |
| Jessica Ramella-Roman | Polarised MC — Stokes vector (2005); pMCX (2022) | OHSU (Jacques lab) → FIU |
| Alexander Doronin | GPU mcxyz.c (CUDA, ~2012) → NMC | Otago (Meglinski lab) → VUW |
| Li Ting + Yin-chu Chen | mcxyz.c (combined voxel code, 2014) | OHSU (Jacques & Prahl labs) |
| Andersen & Marti (DTU) | MCmatlab — MATLAB/teaching MC (2018) | DTU Fotonik |
| A. Phong Tran | Curved/oblique boundaries in voxel MC (2020) | Northeastern (Fang lab) + Jacques |

---

## 4. The Nonlinear Frontier: Raman and Beyond

Classical MC codes model **linear** photon transport: absorption, elastic scattering, and sometimes fluorescence. The emerging frontier addresses **nonlinear** and **inelastic** interactions — particularly Raman scattering, which enables label-free chemical tissue identification.

### Fabrizio Martelli — Analytical Raman Models + MC Reference Data

**Fabrizio Martelli** (Assoc. Professor, Univ. of Florence, Dept. Physics and Astronomy) leads a group bridging analytical diffusion theory with MC validation. His textbook (*Light Propagation through Biological Tissue and Other Diffusive Media*, SPIE Press, 2010/2022) is the field's primary reference text. From 2016 onward his group developed **time-domain analytical forward solvers for Raman signals** in diffusive media (parallelepiped and cylinder geometries), validated against MC simulations. Their model is based on coupled diffusion equations at excitation and Raman emission wavelengths, accounting for the full wavelength dependence of optical properties.

**Key works:**
- Martelli F., Binzoni T., Sekar S.K.V., Farina A., Cavalieri S., Pifferi A. (2016). *Time-domain Raman analytical forward solvers.* Opt. Express **24**(17).
- Martelli F. et al. (2017). *Time-resolved analytical model for Raman scattering in a diffusive medium.* SPIE 10412.
- Martelli F. et al. (2022). *Light Propagation through Biological Tissue and Other Diffusive Media: Theory, Solutions, and Validations*, 2nd ed. SPIE Press (includes Ch. 14 on time-domain DE solutions for Raman signals and Ch. 16 on MC reference data).
- Martelli F. (2025, Photonics). *A Heuristic Time-Domain Raman Forward Solver for Photon Migration in a Two-Layer Diffusive Medium.* Two-layer model: coupled DEs at excitation and emission wavelengths, validated against MC.

### Periyasamy & Pramanik — MCML-EO + RMCC (2014–2015)

**Vijitha Periyasamy** and **Manojit Pramanik** (IISc Bangalore / NTU Singapore → Iowa State Univ.) were the first group to extend MCML with embedded geometrical objects (sphere, cylinder, ellipsoid, cuboid) — **MCML-EO** (2014) — and then add inelastic **Raman scattering** — **RMCC** (2015). Experimentally validated against gelatin tissue phantoms with cuboid inclusions. Also extended to OCT MC simulation.

**Key works:**
- Periyasamy V., Pramanik M. (2014). *Monte Carlo simulation of light transport in tissue with third-order optical nonlinearities.* J. Biomed. Opt. **19**(4). [MCML-EO]
- Periyasamy V., Sil S., Ariese F., Umapathy S., Pramanik M. (2015). *Experimental validation of Raman Monte Carlo simulations for a cuboid object.* J. Raman Spectrosc. **46**(7). [RMCC]
- Periyasamy V., Pramanik M. (2017). *Advances in Monte Carlo simulation for light propagation in tissue.* IEEE Rev. Biomed. Eng. **10**.

### Hokr & Yakovlev — Stimulated Raman MC (2014)

**Brett H. Hokr**, **Vladislav V. Yakovlev**, and **Marlan O. Scully** (Texas A&M University) published the first **time-dependent MC model for stimulated Raman scattering (SRS)** in turbid tissue. SRS is a coherent, intensity-dependent nonlinear process — the first genuinely nonlinear interaction incorporated into a tissue MC framework. Yakovlev is now an active collaborator on the NMC project (2023–).

**Key works:**
- Hokr B.H., Yakovlev V.V., Scully M.O. (2014). *Efficient time-dependent Monte Carlo simulations of stimulated Raman scattering in a turbid medium.* ACS Photonics **1**(12).

### Krasnikov, Seteikin & Roth — Raman MC in Homogeneous Turbid Media (2016–2019)

**Ilya Krasnikov** (Amur State University, Russia) in collaboration with **Alexey Seteikin** and the group of **Bernhard Roth** and **Merve Meinhardt-Wollweber** (Hannover Centre for Optical Technologies, Leibniz Univ. Hannover) developed a series of efficient MC approaches for modelling Raman scattering in turbid media. Their two-stage method separates the calculation of photon fluence from the subsequent generation of Raman photons, allowing efficient simulation of confocal and fibre-probe Raman setups. Extended to account for detector geometry, sampling volume, and internal absorption effects.

**Key works:**
- Krasnikov I., Suhr C., Seteikin A., Roth B., Meinhardt-Wollweber M. (2016). *Two efficient approaches for modeling of Raman scattering in homogeneous turbid media.* J. Opt. Soc. Am. A **33**(3), 426–433.
- Krasnikov I., Seteikin A., Kniggendorf A.-K., Roth B., Meinhardt-Wollweber M. (2017). *Simulation of Raman scattering for realistic measurement scenarios including detector parameters and sampling volume.* J. Opt. Soc. Am. A **34**(12), 2138–2144.
- Krasnikov I., Suhr C., Seteikin A., Meinhardt-Wollweber M., Roth B. (2019). *Monte Carlo simulation of the influence of internal optical absorption on the external Raman signal for biological samples.* J. Opt. Soc. Am. A **36**(5), 877–882.

### Akbarzadeh, Edjlali & Leblond — Full Spectroscopic MC (2020)

**Alireza Akbarzadeh**, **Ehsan Edjlali**, **Frédéric Leblond** et al. (Polytechnique Montréal + CRCHUM) developed a full **spectroscopic MC** package combining absorption, fluorescence, elastic, and inelastic Raman scattering in a single framework. Validated against tissue phantoms for depth-resolved Raman sensing. Leblond's group is a world leader in intraoperative Raman guidance for brain and lung cancer surgery.

**Key works:**
- Akbarzadeh A., Edjlali E., Sheehy G., Selb J., Leblond F. et al. (2020). *Experimental validation of a spectroscopic Monte Carlo light transport simulation technique and Raman scattering depth sensing analysis in biological tissue.* J. Biomed. Opt. **25**(10).

### Vladyko & Doronin — NMC (2023–)

**Ilya Vladyko** and **Alexander Doronin** (Victoria University of Wellington), in collaboration with **Vladislav V. Yakovlev** (Texas A&M), developed the **NMC framework**: the first unified open-source MC engine for both spontaneous and stimulated Raman scattering in biological tissue, optimised for Apple M-series (ARM) processors. The Neu(t)ralMC iteration (2023) demonstrated energy-efficient high-throughput simulation without a dedicated NVIDIA GPU.

**Key works:**
- Clennell B., Nguyen T., Yakovlev V.V., Doronin A. (2023). *Neu(t)ralMC: open-source energy-efficient Monte Carlo algorithm for photon transport.* Opt. Express **31**(19).
- Vladyko I., Yakovlev V.V., Doronin A. (2025). *Monte Carlo modelling of Raman signal enhancement using Apple M-series processors.* SPIE BiOS 2025.

### Nonlinear Frontier Summary

| Group | Key contribution | Institution |
|---|---|---|
| Martelli et al. | Time-domain analytical Raman forward solvers; MC reference datasets for Raman in diffusive media (2016–) | Univ. of Florence, Italy |
| Periyasamy & Pramanik | MCML-EO (embedded geometry, 2014); RMCC (Raman MC, 2015) | IISc/NTU → Iowa State Univ. |
| Hokr & Yakovlev | First SRS (stimulated Raman) MC in turbid tissue (2014) | Texas A&M Univ. |
| Krasnikov, Seteikin & Roth | Two-stage Raman MC; realistic detector/probe geometries (2016–2019) | Amur State Univ. + Leibniz Univ. Hannover |
| Akbarzadeh, Edjlali & Leblond | Full spectroscopic MC: absorption + fluorescence + elastic + Raman (2020) | Polytechnique Montréal + CRCHUM |
| **Vladyko & Doronin** | **NMC: unified spontaneous + SRS MC; Apple M-series optimisation (2023–)** | **Victoria Univ. Wellington** |

---

## 5. Active Platforms Today

| Code | Group / PI | URL | Key features |
|---|---|---|---|
| **MCML** | S. Prahl (Oregon Tech) | [omlc.org/software/mc](https://omlc.org/software/mc) | Foundational multi-layer 1D MC; open-source C |
| **MCX / MMC** | Q. Fang (Northeastern) | [mcx.space](https://mcx.space) | GPU voxel MC (CUDA/OpenCL); mesh MC; pMCX (polarised); cloud |
| **MCmatlab** | Andersen & Marti (DTU) | [gitlab.gbar.dtu.dk/biophotonics/MCmatlab](https://gitlab.gbar.dtu.dk/biophotonics/MCmatlab) | MATLAB; 3D voxel; heat diffusion; teaching |
| **VTS / MCCL** | Venugopalan (UCI BLI) | [virtualphotonics.org](https://virtualphotonics.org) | C#; perturbation MC; differential MC; GUI; NIH-funded |
| **NMC** | A. Doronin (VUW) | [lighttransport.net](https://lighttransport.net) · [github.com/aledoronin](https://github.com/aledoronin) | Nonlinear MC; Raman (spontaneous + SRS); Apple M-series |
| **Multi-Scattering** | Berrocal & Jönsson (Lund) | [multi-scattering.com](https://www.multi-scattering.com) | Free online; GPU cluster; 800× speedup; tissue + spray |
| **MCML online** | S. Prahl (Oregon Tech) | [omlc.org/software/mc](https://omlc.org/software/mc) | Static reference archive; downloadable C source |

---

## 6. NMC: Position and Contribution

The NMC project, led by **Alexander Doronin** at Victoria University of Wellington, occupies a unique position at the **nonlinear frontier** of biomedical MC simulation.

### What NMC does differently

1. **Unified nonlinear MC framework.** NMC handles both spontaneous and stimulated Raman scattering in a single photon transport engine — not as an add-on to a linear code.
2. **Power-efficient computation.** The Neu(t)ralMC iteration (2023) targets Apple M-series (ARM) processors, enabling high-throughput simulation on low-power hardware without a dedicated NVIDIA GPU.
3. **Verified lineage.** Doronin's GPU mcxyz.c (~2012) is acknowledged by Jacques (2022). NMC is its direct evolution.
4. **Collaborative nonlinear physics.** The Yakovlev collaboration (Texas A&M) — first SRS MC paper, 2014 — brings the deepest available expertise in nonlinear Raman physics.
5. **Object-oriented and P2P architecture.** Earlier work (Doronin & Meglinski 2011, 2012) introduced OO and distributed P2P MC underpinning NMC's modularity.

### Capability Matrix

| Capability | MCML | MCX | MCmatlab | NMC |
|---|---|---|---|---|
| Multi-layer tissue | ✅ | ✅ | ✅ | ✅ |
| 3D voxel geometry | — | ✅ | ✅ | ✅ |
| GPU acceleration | — | ✅ | partial | ✅ |
| Polarisation / Stokes | ext. | ✅ pMCX | — | ✅ |
| Fluorescence | — | — | — | planned |
| **Spontaneous Raman MC** | — | ext. | — | **✅ core** |
| **Stimulated Raman (SRS)** | — | — | — | **✅ core** |
| Apple M-series ARM opt. | — | — | — | **✅** |
| Energy-efficient design | — | — | — | **✅** |

---

## 7. Lineage Tree

```
1983  Wilson & Adam (Univ. Toronto) ─── first MC for tissue (isotropic scattering)

1985  Steven L. Jacques (Wellman/MGH) ─── mcml.c, lookup-table scattering
  │
1989  Marleen Keijzer (Delft → Jacques lab) ─── HOP/DROP/SPIN/CHECK; multi-layer; fluorescence
  │
1989  Scott A. Prahl (UT Austin, Welch lab) ─── variable stepsize; mismatched boundaries;
  │                                              Cartesian coords; HG scattering; omlc.org (1996)
  │
1992  Lihong V. Wang (UTMDACC, Jacques lab) ─── definitive MCML; multi-layer + refractive index;
  │                                              173p manual; first public website
  │
1995  Wang, Jacques, Zheng ─── MCML paper (4000+ citations)
  │
  ├── 2002  Boas (MGH) ─── tMCimg (3D voxel, heterogeneous)
  │     └── 2009  Fang + Boas ─── MCX (GPU CUDA; ~400× speedup)
  │                └── [Raman ext.] Dumont/Patil/Fang (2021) — isoweight Raman MC
  │
  ├── 2005  Ramella-Roman (OHSU, Jacques lab) ─── polarised MC (Stokes)
  │     └── 2022  Yan/Fang/Ramella-Roman/Jacques ─── pMCX (GPU polarised, in MCX)
  │
  ├── 2008  Alerstam (Lund, Andersson-Engels) ─── first GPU-MCML (CUDA)
  │     └── [same Lund group] Jönsson/Berrocal (2020) ─── Multi-Scattering (online GPU)
  │
  ├── 2014  Li Ting + Chen (Jacques/Prahl labs) ─── mcxyz.c (3D voxel)
  │     │
  │     ├── ~2012  Doronin (Otago, Meglinski lab) ─── GPU mcxyz.c (CUDA) ← cited Jacques 2022
  │     │     └── 2019–  NMC @ Victoria Univ. Wellington
  │     │           ├── Spontaneous Raman MC engine
  │     │           ├── SRS MC engine ←── Hokr/Yakovlev 2014 (collaborator 2023–)
  │     │           └── Neu(t)ralMC / Apple M-series (2023)
  │     │
  │     └── 2018  Andersen & Marti (DTU) ─── MCmatlab (MATLAB teaching tool)
  │
  └── 2020  Tran (Fang lab + Jacques) ─── curved boundaries in voxel MC

[Independent / parallel lines]
  VTS / MCCL (UCI BLI, 1998–) ─── Venugopalan / Spanier / Hayakawa
  Martelli group (Florence, 1997–) ─── diffuse optics theory + Raman forward solvers
  Krasnikov / Roth (Hannover, 2016–) ─── Raman MC in homogeneous media
  Akbarzadeh / Leblond (Montréal, 2020) ─── full spectroscopic Raman MC

[Nonlinear cross-connections]
  Hokr/Yakovlev (Texas A&M 2014) ─── Vladyko/Doronin (VUW, 2023–)
  Dumont/Patil (Temple 2021): uses Fang's MCX codebase, Fang co-author
```

---

## 8. Key References

1. **Wilson B.C., Adam G.** (1983). A Monte Carlo model for the absorption and flux distributions of light in tissue. *Med. Phys.* **10**(6), 824–830. [doi:10.1118/1.595361](https://doi.org/10.1118/1.595361)
2. **Keijzer M. et al.** (1989). Light distributions in artery tissue: Monte Carlo simulations for finite-diameter laser beams. *Lasers Surg. Med.* **9**, 148–154. [doi:10.1002/lsm.1900090210](https://doi.org/10.1002/lsm.1900090210)
3. **Prahl S.A. et al.** (1989). A Monte Carlo model of light propagation in tissue. *Proc. SPIE IS5*, 102–111. [doi:10.1117/12.2283590](https://doi.org/10.1117/12.2283590)
4. **Wang L.V., Jacques S.L., Zheng L.** (1995). MCML — Monte Carlo modeling of photon transport in multi-layered tissues. *Comput. Methods Prog. Biomed.* **47**, 131–146. [doi:10.1016/0169-2607(95)01640-F](https://doi.org/10.1016/0169-2607(95)01640-F)
5. **Jacques S.L.** (2022). History of Monte Carlo modeling of light transport in tissues using mcml.c. *J. Biomed. Opt.* **27**(8), 083002. [doi:10.1117/1.JBO.27.8.083002](https://doi.org/10.1117/1.JBO.27.8.083002)
6. **Boas D.A. et al.** (2002). Three dimensional Monte Carlo code for photon migration through complex heterogeneous media. *Opt. Express* **10**(3), 159–170.
7. **Fang Q., Boas D.A.** (2009). Monte Carlo simulation of photon migration in 3D turbid media accelerated by graphics processing units. *Opt. Express* **17**(22), 20178–20190.
8. **Alerstam E. et al.** (2008). Parallel computing with graphics processing units for high-speed Monte Carlo simulation of photon migration. *J. Biomed. Opt.* **13**(6), 060504.
9. **Ramella-Roman J.C., Prahl S.A., Jacques S.L.** (2005). Three Monte Carlo programs of polarized light transport. *Opt. Express* **13**(12) and **13**(25).
10. **Marti D., Aasbjerg R.N., Andersen P.E., Hansen A.K.** (2018). MCmatlab. *J. Biomed. Opt.* **23**(12), 121622.
11. **Hayakawa C.K. et al.** (2022). MCCL: open-source software for MC radiative transport. *J. Biomed. Opt.* **27**(8), 083005.
12. **Hokr B.H., Yakovlev V.V., Scully M.O.** (2014). Efficient time-dependent Monte Carlo simulations of stimulated Raman scattering in a turbid medium. *ACS Photonics* **1**(12).
13. **Periyasamy V., Pramanik M.** (2015). Experimental validation of Raman Monte Carlo simulations for a cuboid object. *J. Raman Spectrosc.* **46**(7).
14. **Krasnikov I. et al.** (2016). Two efficient approaches for modeling of Raman scattering in homogeneous turbid media. *J. Opt. Soc. Am. A* **33**(3), 426–433.
15. **Krasnikov I. et al.** (2017). Simulation of Raman scattering including detector parameters and sampling volume. *J. Opt. Soc. Am. A* **34**(12), 2138–2144.
16. **Krasnikov I. et al.** (2019). Monte Carlo simulation of the influence of internal optical absorption on the external Raman signal. *J. Opt. Soc. Am. A* **36**(5), 877–882.
17. **Akbarzadeh A. et al.** (2020). Experimental validation of a spectroscopic Monte Carlo light transport simulation technique and Raman scattering depth sensing analysis in biological tissue. *J. Biomed. Opt.* **25**(10).
18. **Martelli F. et al.** (2016). Time-domain Raman analytical forward solvers. *Opt. Express* **24**(17).
19. **Jönsson J., Berrocal E.** (2020). Multi-Scattering software. *Opt. Express* **28**(25).
20. **Doronin A., Meglinski I.** (2011). Online object-oriented MC computational tool. *Biomed. Opt. Express* **2**.
21. **Doronin A., Meglinski I.** (2012). Peer-to-peer Monte Carlo simulation of photon migration. *J. Biomed. Opt.* **17**(9).
22. **Clennell B., Nguyen T., Yakovlev V.V., Doronin A.** (2023). Neu(t)ralMC. *Opt. Express* **31**(19).
23. **Vladyko I., Yakovlev V.V., Doronin A.** (2025). Monte Carlo modelling of Raman signal enhancement using Apple M-series processors. *SPIE BiOS 2025.*

---

*Document maintained by the NMC project, Victoria University of Wellington. Last updated 2025.*
