# 7.6.3. Crystal lattice matches of two graphene

This subsection will introduce how to search the matched lattice for two materials which is used for building the simulated model for interface structure prediction.

This is an example for matching the lattices for graphene asymmetric grain boundary. The slab model with the optimally matched lattice adopted in the interface structure prediction will be generated automatically from graphene bulk structure.

| File Name           | Description                                                                                                         |
|---------------------|---------------------------------------------------------------------------------------------------------------------|
| calypso.x           | The executable file of CALYPSO program to cleave the surface (i.e. create the primitive vectors of surface) with specified miller index          |
| input.dat.ini       | The pre-input file to generate the input.dat file of CALYPSO specifying the controllable key parameters                                        |
| lat1.cif            | Structure files of bulk crystal in cif format with symmetry information                                                                         |
| lat2.cif            | Structure files of bulk crystal in cif format with symmetry information                                                                         |
| input_mismatch.dat  | Input files of lattice match toolkit                                                                                                           |
| gen_slab_model.sh   | The script for calling calypso.x                                                                                                               |
| mismatch-zur.x      | The executable file of lattice match toolkit                                                                                                    |


`calypso.x` can be downloaded according to different operating systems.

`input.dat.ini` contains all the parameters of calypso.x to cleave the surface. Here is an example:
~~~
LSurface    = T                      # The flag specifies whether surface reconstruction prediction will be performed.
ICode       = 1
Kgrid       = 0.1
PopSize     = 40
MaxStep     = 10
Pre_surf_relax=F

Surface_thickness = 1.5              # This variable (in unit of angstrom) specifies the thickness of surface reconstruction, and it should be set as a value slightly larger than the double distance of two adjacent atomic layers in bulk materials.
ForbiThickness    = 0.50000000
SPACESAVING = T
@SURFACE_ATOMS  # |atomic symbol|count|
C  4
@END

#used to build the surface from bulk info
#reconstruction symmetry
Substrate        = Auto               # By defining this variable to ‘Automatic’ or ‘Auto’, the substrate will be generated automatically from its bulk structure. 
@MATRIX_NOTATION                      # The row (m) rank of the matrix is determined by “NumberOfSpecies”. For each row, the matrix contains two columns. The first column (string) is the elemental symbol of each chemical species, followed by the number of atoms for such a chemical species (integer).
1     0
0     1
@END

Use_Cif_File = T
Cif_File_Path = SUBSTRATE              # Specifies the name of crystal structure (in CIF format), whose surface structure prediction is to be performed.
Miller_index = which                   # Defines the targeted surface as determined by the Miller indices h, k, and l for doing surface reconstruction calculations.
Slab_Vacuum_Thick = 15.0               # This parameter (in units of angstrom) defines the vacuum region, where the whole slab is separated from its periodic images.
Slab_Num_Layers = 8                    # Number of layers in the substrate
Num_Relaxed_Layers = 2                 # Number of the relaxable layers in the substrate, must be smaller than SlabNumLayers

Gen_Substrate=T
~~~

`lat1.cif` and `lat2.cif` are the user defined initial crystal files used for cleave the surfaces. They are in CIF format and the symmetry information should be included.

`input_mismatch.dat`

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

`gen_slab_model.sh` is a script used to call calypso.x program to cleave surface. It can be downloaded from the website: http://download.calypso.cn/

Once all the input files are prepared, you may run the structure prediction via the following command, or alternatively, put this execution command into a job submit script:

~~~
./mismatch-zur.x > mis.log
~~~

The result is recorded in the file `mismatch-total.dat`. It’s ranked by the degree of lattice mismatch (maismatch_value). The parameters “hkl” and “uv” are used to construct the surface by other software, such as Material Studios.
~~~
No. hkl1       uv1       hkl2        uv2      mismatch_value
1   0 0 1   3.00  0.00   0 0 1   -2.00   3.00      1
            0.00  3.00           -3.00  -2.00
2   1 4 1   3.00  3.00   1 1 0   -2.00   2.00      2
           -1.00  0.00            0.50  -0.50
~~~


# 7.6.3. 两层石墨烯的晶体晶格匹配

此小节将介绍如何为两种材料搜索匹配的晶格，该方法用于构建界面结构预测的模拟模型。

这是一个用于匹配石墨烯非对称晶粒界限的晶格的示例。界面结构预测中使用的具有最佳匹配晶格的平板模型将自动从石墨烯体相结构生成。

| 文件名             | 描述                                                                                                             |
|-------------------|------------------------------------------------------------------------------------------------------------------|
| calypso.x         | CALYPSO程序的可执行文件，用于用指定的Miller指数切割表面（即创建表面的原始向量）                                         |
| input.dat.ini     | 生成CALYPSO的input.dat文件的预输入文件，指定可控的关键参数                                                            |
| lat1.cif          | 附带对称信息的体晶格结构的cif格式文件                                                                              |
| lat2.cif          | 附带对称信息的体晶格结构的cif格式文件                                                                              |
| input_mismatch.dat| 晶格匹配工具的输入文件                                                                                           |
| gen_slab_model.sh | 调用calypso.x的脚本                                                                                              |
| mismatch-zur.x    | 晶格匹配工具的可执行文件                                                                                         |

`calypso.x` 可以根据不同的操作系统进行下载。

`input.dat.ini` 包含了切割表面的calypso.x的所有参数。这里有一个示例：
~~~
LSurface    = T                      # 此标志指定是否执行表面重构预测。
ICode       = 1                      # Defines which code to be used for local structure optimization during the structure prediction.
Kgrid       = 0.1                    # The precision of the K-point sampling for local geometric optimization for VASP, Quantum-Esspresso and DFTB+ codes.
PopSize     = 40                     # The population size, i.e., the total number of structures per generation.
MaxStep     = 10                     # The maximum number of generations to be executed for the entire structure prediction simulation.
Pre_surf_relax=F

Surface_thickness = 1.5              # 该变量（以埃为单位）指定表面重构的厚度，它应稍大于体材料中两个相邻原子层的双倍距离。
ForbiThickness    = 0.50000000
SPACESAVING = T                      # If this parameter is set as “True”, the output files for structure relaxation will be deleted.
@SURFACE_ATOMS                       # 矩阵的行(m)等级由“NumberOfSpecies”决定。对于每一行，矩阵包含两列。第一列(字符串)是每种化学物种的元素符号，后面跟着该化学物种的原子数量(整数)。
C  4                                 # |原子符号|计数|
@END

#用于从体信息构建表面
#重构对称性
Substrate        = Auto               # 通过将此变量定义为“Automatic”或“Auto”，底物将自动从其体结构生成。
@MATRIX_NOTATION                      
1     0
0     1
@END

Use_Cif_File = T
Cif_File_Path = SUBSTRATE              # 指定将进行其表面结构预测的晶体结构的名称（CIF格式）。
Miller_index = which                   # 根据Miller指数h、k和l定义目标表面，以进行表面重构计算。
Slab_Vacuum_Thick = 15.0               # 该参数（以埃为单位）定义真空区域，其中整个平板与其周期性图像分开。
Slab_Num_Layers = 8                    # 底板中的层数
Num_Relaxed_Layers = 2                 # 底板中的可放松层数，必须小于SlabNumLayers

Gen_Substrate=T
~~~

`lat1.cif` 和 `lat2.cif` 是用户定义的用于切割表面的初始晶体文件。它们采用CIF格式，应包括对称性信息。
`input_mismatch.dat`

~~~
# 此变量指定搜索超晶格的最大面积。
MaximumArea = 200.0 

# 此变量指定最大晶格失配值。
MaximumMismatchValue = 0.05 

# 此变量指定用于根据输入的晶体结构创建晶面的Miller指数的最大组成部分。
MaximumMillerIndex = 5 

# 此标志指定输入的晶体结构是否为2D层状材料。
# 如果输入的晶体结构是2D层状材料，此参数设为“True”。
2DStrucuture = F

# 此参数仅用于晶界系统，其中两个体相属于相同的材料。
# 该标志指定孪生边界是否被保留。如果需要保留孪生边界，此参数设为“True”。
SameSubstrate = F

# 如果此参数设为“True”，只有在“MillerIndex1List”中列出的Miller指数的表面才会生成。
# 如果此参数设为“False”，所有Miller指数小于指定的最大Miller指数的表面都将被考虑。
SpecifyingMillerIndex1 = F

# 此参数是用于为第一种材料生成表面的指定Miller指数的列表。
@MillerIndex1List
1 4 1 
@End 

SpecifyingMillerIndex2 = F

@MillerIndex2List
1 1 0 
@End
~~~

`gen_slab_model.sh` 是用来调用 calypso.x 程序切割表面的脚本。您可以从以下网站下载它：http://download.calypso.cn/

一旦准备好所有的输入文件，您可以通过以下命令运行结构预测，或者将此执行命令放入作业提交脚本中：

~~~
./mismatch-zur.x > mis.log
~~~

结果记录在文件 `mismatch-total.dat` 中。它是根据晶格失配度（maismatch_value）排名的。参数“hkl”和“uv”用于通过其他软件（例如 Material Studios）构建表面。
