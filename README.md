# MAPOR1.0: MutAtion landscaPe GeneratOR

(c) 2014 Mohamed R. Smaoui, Jerome Waldispuhl
School of Computer Science, McGill University, 3630 University Street, Montreal, Canada
jeromew@cs.mcgill.ca
http://amyloid.cs.mcgill.ca

MAPOR is a program for analyzing the mutational landscape of proteins. 
It uses an energy function that computes the Lennard-Jones, Coulomb, and solvation energies
to determine the effect of a mutation on a protein's stability.

MAPOR runs as command-line application.

## INSTALLATION:

MAPOR utilizes the output of several programs to compute stability. In particular, it requires
the prior installation of GROMACS, SCWRL4, AQUASOL and PDB2PQR.

If you have these programs installed, please configure their paths in the file MAPOR.py

If you don't have these programs installed, you can download the latest versions and install them from the
following sources:
* GROMACS: http://www.gromacs.org/Downloads
* SCWRL4:	http://dunbrack.fccc.edu/scwrl4/ 
* PDB2PQR: http://www.ics.uci.edu/~dock/pdb2pqr/userguide.html

Alternatively, you can also find these software in the "extra_tools" directory. The current version of MAPOR is compatible with versions found in the "extra_tools" directory.

Before you use MAPOR, you need to run python install.py to install the AquaSol program that
calculate Solvation energy and the PDB2PQR program. To do that execute the following 2 commands
with the sudo command if needed

```bash
	$ sudo tar zxvf AquaSol_Complexes.tgz
	$ sudo ./AquaSol_Complexes/Makefile
```

You will also have to install the other programs (GROMACS, SCWRL, and PDB2PQR)

The setup files for MD simulations and EM minimization are found under the "mdp" directory.

USAGE 1: Mutate a single residue

```bash
	$ python MAPOR.py -f protein.pdb -p position -m AAmutation -s sequence
		-f protein.pdb: the PDB file you wish to use
		-p position: the residue (amino acid) position you want to mutate
		-m AAmutation: the amino acid mutation you want to replace position p with.
		   This is a letter. ex. Alanine is A
		-s sequence: the complete amino acid sequence of the residues in the protein file
```

	This commands performs a single point mutation on your protein file and returns
	the Coulomb, LJ, and Solvation energies of the protein before and after the mutation. 


USAGE 2: Mutate multiple residues simultaneously

```bash
	$ python MAPOR.py -f protein.pdb -p p1,p2,p3,...,pn -m m1,m2,m3,...,mn -s sequence
		-f protein.pdb: the PDB file you wish to use
		-p p1,p2,p3,...,pn: A set of integer positions representing the amino acid 
		   positions you wish to mutate
		-m m1,m2,m3,...,mn: A set of amino acid point mutations you wish to replace
		   p1,p2,p3,...,pn with, respectively. Each mutation should be a letter. ex Alanine is A
		-s sequence: the complete amino acid sequence of the residues in the protein file
```

	This commands performs simultaneous multiple single point mutations on your protein file and
	returns the Coulomb, LJ, and Solvation energies of the protein before and after the mutation.  


USAGE 3: Complete Single Residue Mutation Landscape

```bash
	$ python MAPOR.py -f protein.pdb -p position -m all -s sequence
		-f protein.pdb: the PDB file you wish to use
		-p position: the residue (amino acid) position you want to mutate
		-m all: this will perform all 19 mutations on this specific residue.
		-s sequence: the complete amino acid sequence of the residues in the protein file
```

	This commands executes 20 runs. It performs 19 separate single point mutations on your 
	protein file and returns the Coulomb, LJ, and Solvation energies of the 20 proteins. 
	This call explores the mutation landscae of a single residue

USAGE 4: Complete Protein Landscape

```bash
	$ python MAPOR.py -f protein.pdb -p n,m -s sequence
		-f protein.pdb: the PDB file you wish to use
		-p n,m: this will perform 19 mutations on all the residues of your protein between n and m
		   inclusive.
		-s sequence: the complete amino acid sequence of the residues in the protein file
```

	This command executes 20*n runs, given that your protein contains n residues (amino acids). It
	will perform 19 separate single point mutations for each residue position. This call explores
	the mutation landscape of your entire protein.
	This call will return three tables:
		1. A table outlining the Coulomb, LJ, and Solvation energies of all your mutations
		2. A table outlining the potential of each residue in destabilizing your protein.
		3. A table outlining the weakest and most susceptible residues that can be used to
		   destabilize your protein.
		4. A table with amino acid positons (x-axis), and mutants (y-axis) and the protein 
		   energy caused by the mutants on each position.


