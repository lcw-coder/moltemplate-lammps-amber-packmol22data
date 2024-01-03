AmberTools23:
antechamber -i mol.pdb -fi pdb -o mol_bcc.mol2 -fo mol2 -pf y -c bcc
parmchk2 -i mol_bcc.mol2 -f mol2 -o mol_gaff2.frcmod -s 2 -a Y -pf 2 -FC 2
amber2lt.py --in mol.frcmod --name MyForceField >> my_force_field.lt
mol22lt.py --in mol_bcc.mol2 --out aac.lt --name Aac --ff MyForceField --ff-file my_force_field.lt
获得基底lt模板文件：
ltemplify.py -name slab slab.data  > slab.lt   其中slab.dataw文件是使用msi2lmp.exe工具获得的！
通过packmol获得system.pdb格式：
packmol < min.inp
min.inp:
tolerance 2.0
filetype pdb
output systemAB.pdb
add_box_sides
structure SLAB.pdb
		number 1
		inside box 0. 0. 6. 62. 65. 35.
end structure
structure GLU.pdb
		number 30
		inside box 0. 0. 20. 62. 65. 90.
end structure
structure spce.pdb
		number 30
		inside box 0. 0. 20. 62. 65. 90.
end structure
通过moltemplate.sh获得与systemAB.pdb缺失的拓扑参数；、
moltemplate.sh -pdb systemAB.pdb system.lt
