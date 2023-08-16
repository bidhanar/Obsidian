# Structural Modelling

## 2.1 Introduction
- To analyse the model, two structural model, one for blade deformation and one for swashplate is developed.  
- The swashplate model has two primary objectives:
	1. The prediction of servo loads
	2. Study of effect of periodic variation of control system stiffness and swashplate dynamics on blade loads. 
- The goal of this modelling is to isolate the physics of structural dynamics from the aeroelastic response problem to understand the mechanisms behind high structural loads encountered during unsteady maneuvers.
- For this work, second order non-linear beam theory is used. This uses additional frames attached locally to a set of individual beam elements, which only undergo moderate deformation.
- The level flight data is used to compare three structural formulations using measured airload analysis:
	1. Second order non-linear beam Finite Element Method (FEM) with modal reduction.
	2. Second order non-linear beam (FEM) without modal reduction
	3. Second order non-linear beam FEM within a multibody formulation.
- The natural frequencies of the blades often lie close to the forcing harmonics, this poses a big problem in measured airloads. 
- The blade deformations calculated using measured airload analysis are termed as _prescribed deformations_ and are used to compare airloads in different aerodynamic model so as to separate the physics of aerodynamics from the aeroelastic response problem.

## 2.2 Multibody Formulation with Full FEM
- The rotor blades and supporting structures are first divided into multiple bodies, both rigid and flexible.
- The rotor is modelled as a second order nonlinear Euler-Bernoulli beam with axial elongation and elastic twist modelled.
- This helps in managing arbitrary large deformations by allowing the finite motion of the frames attached to the individual element while the elastic deformation within each element remains moderate.
- The rotor pitch control angles are imposed as linear displacements at the bottom of the pitch links and are adjusted iteratively to generate the measured root pitch angles.

### 2.2.1 Blade Coordinate System
- 