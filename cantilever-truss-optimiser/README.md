# Cantilever truss: design, optimisation and test to failure

**University pair project · 2025 · Complete, with post-mortem**

## Summary

An open-ended design problem, done in a pair: design and build a light aluminium cantilever truss, in two parallel planes, that carries 2.7 kN over an 800 mm span, then test it to failure. We used a browser-based truss solver and evolutionary optimiser to search for light designs for one plane, built the structure, and tested it to destruction to check the model against reality.

The optimiser designs a single plane. Most of the saved candidates were run with the full 2.7 kN on one plane, which would give each of the two planes a safety factor of 2. The design we actually built, `truss_model254.json`, was run at 1.35 kN per plane, which is the two planes sharing 2.7 kN with no margin.

![The built truss mounted in the test rig; the mass is written on the plate](images/truss-test-rig.jpg)

## Key numbers

| | Value |
|---|---|
| Span | 800 mm |
| Structure | Two parallel planes of aluminium angles with steel joint plates |
| Required load | 2.7 kN in total, shared by the two planes |
| Design load per plane in the optimiser | 2.7 kN for the candidates in `models/candidates-2700N/`; 1.35 kN for the design we built |
| Design failure load of the built design | 2.7 kN (no margin) |
| Mass of the built structure | 0.86 kg |
| Modelled member mass | 254 g per plane for the built design; 269 – 304 g per plane for the 2.7 kN candidates |
| **Measured failure load** | **550 N, about 20% of the required load** |

## The tool

`src/truss-designer.html` is a single-file HTML/JavaScript app with no dependencies. Open it in a browser and it starts with the design we built; use **Load JSON** to open another design from `models/`, then **Solve** or **Optimize**.

![The designer showing the design we built, truss_model254: green members are below 95% of their limit and orange members are within 5% of it](images/designer-example.png)

**Why this design.** It was not the best-scoring candidate. We chose it because it was good enough on the model and simpler to build: the bottom chord (joints 1–5–4) and the upper chord (3–6–2) are each close to a straight line, so each could be cut from one length of angle with the intermediate joints fixed along it, instead of every member being a separate piece. Fewer separate members meant fewer joints to drill and align. The price shows in the picture: all three bottom-chord segments sit at 99% of their buckling limit.

- **Solver:** a 2D pin-jointed truss, which is one plane of the structure. Joints, members, supports and the tip load are edited on a canvas. Member forces come from joint equilibrium, solved as a linear system. Members are coloured by stress ratio (tension) or by buckling ratio (compression).
- **Sections:** five aluminium angle sizes, with cross-section areas from 27.7 to 73.2 mm². Tension is limited to σ_allow (250 N/mm² by default). Compression is limited by a buckling stress read from a slenderness curve (member length divided by section width).
- **Optimiser:** an evolutionary search over joint positions (Gaussian mutation) and member section sizes, with crossover and elitist selection. Four presets, from Fast to Max, set the population (24 to 64), number of parents, iterations (200 to 5000), mutation sizes and an early-stopping patience.
- **Objective:** total member mass, plus penalties for exceeding a stress or buckling limit, for members shorter than 100 mm, for joints closer than 50 mm and for crossing members.
- **Models:** `models/final-design/truss_model254.json` is the design we built, run at 1.35 kN per plane. `models/candidates-2700N/` holds the earlier single-plane candidates run at 2.7 kN. The number in each file name is the modelled member mass in grams, so `truss_model288.json` is a 287.8 g plane. `models/starting-layouts/` holds three simple starting layouts (`tri`, `bibi` and `tribi`).
- `src/truss-multi-viewer.html` shows several saved designs side by side.

### Who wrote what

I specified the objective and the evolutionary algorithm. Most of the code was written by an AI assistant from those specifications, not by me, so this repository shows engineering judgement about what to optimise and how to search, not programming of the JavaScript itself.

## What went wrong

The truss **failed by buckling at 550 N**, about 20% of the required load, because it was under-braced. The tool and its saved designs show why the model could not see this:

- **No margin.** The optimiser minimises mass against its own limits, so it pushes members to the edge. Every saved candidate has two to five members at 95% or more of their calculated stress or buckling limit, and the built design has its whole bottom chord at 99% of the buckling limit. On top of that, the built design was run at 1.35 kN per plane, so the factor of 2 that the earlier candidates carried was gone. Any effect the model leaves out is then enough to cause failure.
- **One plane only.** The model is a single plane in its own plane. Bracing between the two planes, out-of-plane buckling and joint eccentricity were not modelled, so a design can pass every check in the tool and still be under-braced.
- **Mass was incomplete.** The objective counts member mass only, with no joint plates, fasteners or bracing. The built design models 254 g of members per plane, about 510 g for two planes. The built structure weighed 0.86 kg, so about 40% of its mass was never in the objective.
- **Stiffness was never calculated.** The tool has no deflection calculation, so it could not trade strength and weight against stiffness.

## If I did this again

- Model both planes and the bracing between them, and put joint plates and fasteners in the objective.
- Add out-of-plane buckling checks as constraints, and keep a utilisation limit (for example 0.8) so the search keeps a margin.
- Add a deflection calculation and search for a Pareto front of mass against stiffness and strength.
- Check the best few candidates with a finite-element model before building anything.
- Instrument the test with video and a load cell, and record which member buckles first, so that it can be compared with the model's prediction.

## What is not published

The structures project was assessed coursework. I publish only the design tool, the saved designs and this write-up, and not the engineering drawings or lab reports.
