# CFD-Nozzle-Study
CFD comparison of Nozzle geometries for Aerospace applications

This repository contains the CFD study of different nozzle geometries demonstrated on my bachelor Thesis.

The goal of this study is to compare the performance of two nozzle geometries under similar boundary conditions and highlight differences in expansion behavior and flow characteristics.

https://www.overleaf.com/read/mvnynkhxnynf#9b793b

# Aerospace Nozzles: Theory & CFD Analysis

> Compressible flow analysis of jet engine and rocket nozzles — from theoretical study to numerical validation of a De Laval nozzle in ANSYS Fluent.

<p align="center">
  <img src="images/mach_contour.png" alt="Mach number contour" width="70%">
  <br>
  <em>Mach number distribution showing subsonic → sonic → supersonic transition in a De Laval nozzle (CFD result, ANSYS Fluent).</em>
</p>

---

## Overview

This repository combines two related pieces of work:

1. **Bachelor's thesis** — *Ugelli di scarico dei motori a getto e degli endoreattori* (Politecnico di Milano, 2024). Theoretical aerothermodynamic study of nozzle technologies for aerospace propulsion. Co-authored with Bonati Alessandro and Luigi Capra. Original language: Italian.
2. **Independent CFD extension** *(ongoing)* — numerical validation of the theoretical predictions in ANSYS Fluent. This part was undertaken on my own initiative, beyond the thesis requirements, to develop practical skills with industry-standard CAE tools. ANSYS Fluent was self-taught through tutorials, documentation, and iterative experimentation.
3. **Structural FEM extension** *(in progress)* — thermal–structural analysis of the nozzle wall in ANSYS Mechanical, applying the same self-directed learning approach.

The narrative is from **literature review → analytical framework → numerical validation → structural verification**, building toward a multi-physics view of a single component.

---

## Why nozzles?

The exhaust nozzle is the component that converts thermal energy into directed kinetic energy, and it sets the upper bound on propulsive efficiency. Design choices (fixed vs. variable geometry, bell vs. aerospike, material and cooling strategy) trade off altitude compensation, mass, manufacturing complexity, and thermal survivability. A complete engineer's view of a nozzle therefore needs both:

- **Theory:** what compressible flow allows, where the design space is bounded by physics.
- **Simulation:** what a specific geometry actually does under realistic boundary conditions.

This project covers both for converging–diverging (De Laval) nozzles, with discussion of altitude-compensating alternatives (aerospike, E-D, dual-expander).

---

## Part 1 — Bachelor's Thesis (Theoretical)

**Topics covered:**

- **Compressible flow fundamentals** — quasi-1D conservation equations in variable-area ducts; Mach number evolution; total quantities; choking and the critical pressure ratio $p^*/p_0 = \left(\frac{2}{\gamma+1}\right)^{\gamma/(\gamma-1)}$.
- **Fixed geometry nozzles** — convergent and converging–diverging (De Laval); operating regimes; subsonic / adapted / overexpanded / underexpanded behaviour; normal and oblique shock formation in the diverging section.
- **Variable geometry nozzles** — petal nozzles for afterburning; thrust reversers (cascade, clamshell, bucket); thrust vectoring with focus on the F-35B 3BSD (Three Bearing Swivel Duct).
- **Aerospike nozzles** — toroidal and linear configurations; flow physics around the spike under over/adapted/underexpanded conditions; truncation effects and the open-wake / closed-wake transition; base bleed; thrust vectoring strategies (differential throttling, secondary fluid injection).
- **Other compensating designs** — Expansion–Deflection (E-D) nozzles, variable-throat nozzles with mechanical pintle, and Dual-Expander concentric nozzles for SSTO applications.
- **Materials** — nickel superalloys (Inconel 718, 625), titanium alloys (Ti-6Al-4V, Ti-6242), ceramic matrix composites (SiC/SiC, oxide/oxide, zirconia-based CMC), niobium C-103 with R-512A silicide coating. Norton's creep law applied to Inconel 718.
- **Cooling techniques** — film cooling, regenerative cooling, transpiration cooling, radiative cooling (Stefan–Boltzmann limit).
- **Design considerations** — material/cooling selection across booster, upper-stage, and RCS applications.

📄 [Read the thesis (PDF, in Italian)](./thesis/Ugelli_di_scarico.pdf)

> **Note on authorship:** The thesis was a team project of three students. My contribution focused on [TODO: fill in the specific sections you worked on, e.g., compressible flow fundamentals + aerospike chapter + cooling techniques].

---

## Part 2 — CFD Analysis (Independent Extension)

Numerical simulation of a converging–diverging Laval nozzle to validate the theoretical predictions of Part 1 against ANSYS Fluent results.

### Geometry

Bell-shaped axisymmetric nozzle defined in ANSYS Discovery via spline profile:

| Parameter | Value |
|-----------|------:|
| Inlet diameter | 200 mm |
| Throat diameter | 113 mm |
| Exit diameter | 357 mm |
| Converging length | 150 mm |
| Diverging length | 450 mm |
| Total length | 600 mm |
| Area ratio $A_e/A_t$ | ≈ 10 |

The 2D meridional curve was generated from a 3-point converging spline and a 4-point bell-shaped diverging spline, then revolved around the axis of symmetry.

<p align="center">
  <img src="images/geometry.png" alt="Nozzle geometry" width="55%">
</p>

### Mesh & Solver

- **Mesh:** unstructured tetrahedral, ANSYS Prime Mesh, global element size 5 mm. *[TODO: add cell count and quality metrics]*
- **Solver:** density-based, steady, inviscid
- **Working fluid:** custom `rocket-fluid` — ideal gas, $c_p = 2494\,\mathrm{J/(kg\cdot K)}$, $M_w = 20\,\mathrm{kg/kmol}$

### Boundary Conditions

| Boundary | Type | Value |
|----------|------|------:|
| Inlet | Pressure inlet | $p_0 = 9.8\,\mathrm{MPa}$, $T_0 = 3710\,\mathrm{K}$ |
| Outlet | Pressure outlet | $p_{\text{static}} = 101{,}325\,\mathrm{Pa}$ |
| Wall | No-slip (irrelevant for inviscid run) | — |

### Results

The flow accelerates smoothly through the throat (sonic, $M=1$) and reaches approximately $M \approx 3.8$ at the exit, with no shock formation, indicating near-adapted operation.

<p align="center">
  <img src="images/mach_contour.png" width="48%">
  <img src="images/velocity_contour.png" width="48%">
  <br>
  <img src="images/pressure_contour.png" width="48%">
  <img src="images/temperature_contour.png" width="48%">
  <br>
  <em>Left to right, top to bottom: Mach number, velocity magnitude, static pressure, static temperature.</em>
</p>

### Validation against analytical isentropic theory

*[TODO — add this table once computed from the closed-form isentropic relations.]*

| Quantity | Analytical (isentropic) | CFD | Relative error |
|----------|------------------------:|----:|---------------:|
| $M_{\text{throat}}$ | 1.000 | … | … |
| $M_{\text{exit}}$ | … | ≈ 3.8 | … |
| $T_{\text{exit}}\,[\mathrm{K}]$ | … | ≈ 2000 | … |
| $p_{\text{exit}}\,[\mathrm{Pa}]$ | … | ≈ $1.01 \times 10^5$ | … |
| $u_{\text{exit}}\,[\mathrm{m/s}]$ | … | ≈ 3300 | … |

The CFD reproduces the expected quasi-1D isentropic behaviour: choking at the throat, monotonic supersonic expansion, no shock formation under the imposed pressure ratio.

📄 [Read the full CFD report (PDF)](./Nozzle_analysis.pdf)

---

## Part 3 — Structural FEM (in progress)

Planned thermal–structural analysis of the nozzle wall in ANSYS Mechanical:

- Steady-state thermal analysis with convective inner wall and radiative outer wall
- Coupled structural analysis applying the resulting thermal field and internal pressure load
- Comparison of stress fields for different candidate materials (Inconel 718 vs. SiC/SiC)
- Buckling check on the diverging section

*This part is being developed in parallel with the same self-taught approach used for the CFD work.*

---

## Tech Stack

- **CAD / Geometry:** ANSYS Discovery
- **Meshing & CFD:** ANSYS Workbench, ANSYS Fluent
- **FEM (planned):** ANSYS Mechanical
- **Documentation:** LaTeX

---

## Repository Structure

```
.
├── README.md
├── thesis/
│   └── Ugelli_di_scarico.pdf       # Bachelor thesis (Italian)
├── cfd/
│   ├── Nozzle_analysis.pdf         # Current CFD writeup
│   ├── nozzle_2d.wbpj              # ANSYS Workbench project
│   ├── nozzle_results.wbpj         # ANSYS Workbench results
│   └── nozzle.dsco                 # ANSYS Discovery geometry
├── fem/                            # (in progress)
└── images/                         # Plots and contours used in this README
```

---

## Limitations & Future Work

- **Inviscid model** — viscous simulations with proper boundary-layer resolution and a $k$–$\omega$ SST turbulence model are the natural next step.
- **No mesh independence study yet** — refinement at three levels (coarse / medium / fine) pending to confirm grid independence.
- **Single operating point** — extending the run to multiple back-pressure ratios would let me reproduce the overexpanded / shock-in-divergent / adapted / underexpanded regimes discussed in the thesis.
- **Single geometry** — direct CFD comparison between bell and aerospike geometries (at sea level and altitude) would close the loop with the aerospike chapter of the thesis.
- **Combustion is not modelled** — the inlet uses prescribed total quantities rather than a reacting flow upstream.
- **Structural analysis pending** — the ANSYS Mechanical work is the next major addition.

---

## Author

**Rodrigo Conti Gallenti**
MSc Aerospace Engineering, UiT — The Arctic University of Norway
BSc Aerospace Engineering, Politecnico di Milano

[GitHub](https://github.com/rodrig0conti) · [LinkedIn](https://www.linkedin.com/in/your-handle-here) · rcgallenti@gmail.com

---

## References

Selected references from the thesis bibliography (full list in the thesis PDF):

- Bach, C. et al. *How to steer an aerospike.* 2018.
- Corda, S. et al. *Flight testing the linear aerospike SR-71 experiment (LASRE).* 1998.
- Hagemann, G. et al. *Advanced Rocket Nozzles.* J. Propulsion & Power 14, 1998.
- Johnson, P. *CFD Analysis of a Linear Aerospike Engine with Film-cooling.* 2019.
- Lentini, D. *Propulsione Aerospaziale.* Sapienza Università di Roma, 2012.
- Mouritz, A.P. *Introduction to aerospace materials.* 2012.
- Nair, P., Suryan, A., Kim, H. *Study of Conical Aerospike Nozzles with Base-Bleed and Freestream Effects.* J. Spacecraft and Rockets 56, 2018.
- Wiegand, C. et al. *F-35 Air Vehicle Technology Overview.* 2019.
