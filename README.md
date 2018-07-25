# RANS_stableFE31

## Introduction to the problem and solution
In past simulations of free-surface waves (both breaking and non-breaking) using RANS models, there have been a marked a collective tendency to over-estimate the turbulence levels. In cases involving breaking waves, this has even been most pronounced prior to breaking with turbulence levels pre-breaking of same order of magnitude as in the surf-zone. The underlying cause was identified by Mayer and Madsen (2000) who showed that turbulence production exists in potential flow waves. They further showed that if omega falls below a certain threshold, the model becomes unstable, leading to an exponential growth in both the eddy viscosity and turbulent kinetic energy.


fe31
This code is still being developed and is not yet finished. Stablized turbulence models for foam-extend 3.1

Standard two-equation RANS models are unstable when applied to free-surface waves as documented in Larsen and Fuhrman 2018. In Larsen and Fuhrman 2018 it was likewise shown how standard two-equation turbulence models can be formally stabilized. This repository contains formally stabilized versions of standard two-equation turbulence models for foam-extend 3.1.

## Installation
Source foam-extend 3.1:

	source /home/$USER/foam/foam-extend-3.1/etc/bashrc

In a linux terminal download the package with git by typing:

	git clone https://github.com/BjarkeEltardLarsen/RANS_stableFE31.git

Move the folder to the user source code

	mv RANS_stableFE31 $WM_PROJECT_USER_DIR/src/turbulence/incompressible/
	
Go to the directory and compile the turbulence models

	cd $WM_PROJECT_USER_DIR/src/turbulence/incompressible/RANS_stableFE31
	
	wmake libso
	
## Usage
Include the libary of the stabilized turbulence models in the system/controlDict folder

	libs
	(
    	"libMyStableRASModels.so"
	);

Change the constant/RASproperties
Each of the four models can be chosen by uncommenting the desired model.
If lambda2=0 the models default to their standard OpenFOAM implementation.

	RASModel        kOmegaStab;
	//RASModel        kOmegaSSTStab;
	//RASModel        kEpsilonStab;
	//RASModel        RNGkEpsilonStab;


	turbulence      on;

	printCoeffs     on;

	kOmegaStabCoeffs
	{
  	lambda2 0.05;
	}

	kOmegaSSTStabCoeffs
	{
  	lambda2 0.05;
	}

	kEpsilonStabCoeffs
	{
  	lambda2 0.05;
	}

	RNGkEpsilonStabCoeffs
	{
  	lambda2 0.05;
	}

Change the system/fvSchemes from (if they are specified individually)

	ddt(k)
	ddt(epsilon)
	ddt(omega)
	...
	div(phi,omega)
	div(phi,epsilon)
	div(phi,k)
	
to 
	
	ddt(rho,k)
	ddt(rho,epsilon)
	ddt(rho,omega)
	...
	div(rho*phi,omega)
	div(rho*phi,epsilon)
	div(rho*phi,k)

	
