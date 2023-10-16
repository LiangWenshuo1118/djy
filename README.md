#7.6.3. Crystal lattice matches of two graphene

This subsection will introduce how to search the matched lattice for two materials which is used for building the simulated model for interface structure prediction.

This is an example for matching the lattices for graphene asymmetric grain boundary. The slab model with the optimally matched lattice adopted in the interface structure prediction will be generated automatically from graphene bulk structure.

File:	Description
calypso.x	The executable file of CALYPSO program to cleave the surface (i.e. create the primitive vectors of surface) with specified miller index
input.dat.ini	The pre-input file to generate the input.dat file of CALYPSO specifying the controllable key parameters
lat1.cif, lat2.cif	Structure files of bulk crystal in cif format with symmetry information
input_mismatch.dat	Input files of lattice match toolkit
gen_slab_model.sh	The script for calling calypso.x
mismatch-zur.x	The executable file of lattice match toolkit

calypso.x can be downloaded according to different operating systems.

input.dat.ini contains all the parameters of calypso.x to cleave the surface. Here is an example:
~~~
LSurface         = T
ICode            = 1
Kgrid         = 0.1
PopSize     = 40
MaxStep     = 10
Pre_surf_relax=F

Surface_thickness = 1.5
ForbiThickness    = 0.50000000
SPACESAVING = T
@SURFACE_ATOMS  # |atomic symbol|count|
C  4
@END

#used to build the surface from bulk info
#reconstruction symmetry
Substrate        = Auto
@MATRIX_NOTATION
1     0
0     1
@END
Use_Cif_File = T
Cif_File_Path = SUBSTRATE
Miller_index = which
Slab_Vacuum_Thick = 15.0
Slab_Num_Layers = 8
Num_Relaxed_Layers = 2

Gen_Substrate=T
~~~

lat1.cif and lat2.cif are the user defined initial crystal files used for cleave the surfaces. They are in CIF format and the symmetry information should be included.

input_mismatch.dat

~~~
# This variable specifies the maximum area of searched superlattices.
MaximumArea = 200.0 

# This variable specifies the maximum lattice-mismatch value.
MaximumMismatchValue = 0.05 

# This variable specifies the maximum component of Miller index used for creating
# the facets based on the input crystal structures.
MaximumMillerIndex = 5 

# The flag specifies whether the input crystal structures are 2D layered materials.
# This parameter is set as "True" if the input crystal structures are 2D layered materials.
2DStrucuture = F

# This parameter is only used for the grain boundary system, in which two bulk phases
# are belonged to the same materials. The flag specifies whether the twin boundaries are
# preserved. This parameter is set as "True" if the twin boundary is needed to be preserved.
SameSubstrate = F

# If this parameter is set as "True" only the surface with the Miller indices listed in
# the "MillerIndex1List" will be generated. If this parameter is set as "False", all of
# surfaces with Miller index less than the specified maximum Miller index will be considered.
SpecifyingMillerIndex1 = F

# This parameter is a list of specified Miller indices used for generating the surfaces
# for the 1st material.
@MillerIndex1List
1 4 1 
@End 

SpecifyingMillerIndex2 = F

@MillerIndex2List
1 1 0 
@End
~~~

gen_slab_model.sh is a script used to call calypso.x program to cleave surface. It can be downloaded from the website: http://download.calypso.cn/

Once all the input files are prepared, you may run the structure prediction via the following command, or alternatively, put this execution command into a job submit script:

./mismatch-zur.x > mis.log
The result is recorded in the file mismatch-total.dat. It’s ranked by the degree of lattice mismatch (maismatch_value). The parameters “hkl” and “uv” are used to construct the surface by other software, such as Material Studios.

No. hkl1       uv1       hkl2        uv2      mismatch_value
1   0 0 1   3.00  0.00   0 0 1   -2.00   3.00      1
            0.00  3.00           -3.00  -2.00
2   1 4 1   3.00  3.00   1 1 0   -2.00   2.00      2
           -1.00  0.00            0.50  -0.50
