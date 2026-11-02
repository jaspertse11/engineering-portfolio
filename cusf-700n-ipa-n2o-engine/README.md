# 700 N IPA/N₂O engine with a coaxial swirl injector

**Cambridge University Spaceflight (CUSF), Propulsion Team · 2025 – present · Design complete (design review written December 2025)**

## Summary

I designed a 700 N liquid rocket engine burning isopropyl alcohol (IPA) and nitrous oxide (N₂O). It uses a coaxial swirl injector, chosen for its simplicity of design and manufacture, with the oxidiser injected tangentially to set up a swirling flow that meets the fuel. It is one of several engines in a CUSF comparison of injector types, each designed by a different student, with the aim of comparing their efficiencies.

**My role:** sole designer of this engine.

## Design point

| Parameter | Value |
|---|---|
| Thrust | 700 N |
| Propellants | IPA / N₂O |
| Injector | Coaxial swirl (tangential oxidiser injection) |
| Chamber pressure | 29 bar |
| Expansion ratio | 4.0 |
| c* efficiency (target operating parameter) | 80% |
| Predicted sea-level specific impulse | 182 s |
| Feed delivery pressures | N₂O 36.9 bar, IPA 37.9 bar |

## Approach

I applied thermodynamics and fluid mechanics to size the engine, using RPA for combustion and performance analysis. Injector orifices were sized with a single-phase incompressible model for the IPA and the non-homogeneous non-equilibrium (NHNE) model for the N₂O. The 80% c* efficiency is a target operating parameter, not a measured value.

## Design checks

The design review also covers the mechanical details that make the engine buildable:

- **Sealing:** silicone O-ring seals at the chamber-to-oxidiser-manifold and manifold-to-air joints, with groove depth and minimum wall thickness checked.
- **Plumbing:** BSPP G1/8 tappings for the fuel and oxidiser inlets, and NPT 1/4 ports for the igniter and chamber pressure. Thread depths, minimum wall thickness and fitting clearances were checked.
- **Injector-face retention:** five M6 grade 10.9 bolts hold the face against a chamber-pressure force of 5.16 kN (29 bar over a 47.6 mm diameter). The safety factor is 7.8 in shear and 9.1 in bearing on the 1.6 mm S355 plate.
- **Mounting:** six M5 grade 10.9 bolts on the standard 12-M5 mount pattern. The safety factor against the 700 N nominal thrust is 87 in bolt tension and 83 for thread shear in an assumed 6061-T6 aluminium substrate with 7 mm of engagement.

## Results

The predicted sea-level Isp at the design point is 182 s. This is a design prediction, not a measured result.

## If I were to continue this project

- Hot-fire the engine and measure c* efficiency against the 80% target.
- Compare the measured performance of each injector type like-for-like with the other students' designs.
- Run a sensitivity study of Isp against c* efficiency, chamber pressure and mixture ratio.
- Add a cooling and wall-temperature analysis for longer burns.
