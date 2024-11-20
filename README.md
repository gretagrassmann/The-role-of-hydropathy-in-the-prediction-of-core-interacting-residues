# The role of hydropathy in the prediction of core interacting residues

 <div align="justify"> 


**For more details see <a href="https://arxiv.org/abs/2407.20992" target="_blank">CIRNet</a>, <a href="https://www.sciencedirect.com/science/article/pii/S2001037020305183" target="_blank">Zernike shape</a>, <a href="https://www.nature.com/articles/s41598-023-37130-z" target="_blank">Zernike electrostatic</a>, and
<a href="https://www.frontiersin.org/articles/10.3389/fmolb.2021.626837/full" target="_blank">Hydrophobicity</a>.**


### Table of contents:
  * [Tasks](#tasks)
    + [Data download and preprocessing](#data-download-and-preprocessing)
    + 
  * [Bibliography](#bibliography)

    

Understanding the molecular mechanisms underlying protein-protein interactions is fundamental to molecular biology, with significant implications for therapeutic and biotechnological applications. 
Despite this, understanding the binding process and the stability of the resulting complexes is still a challenge. Binding partners must find each other amid thousands of other types of molecules in the crowded cell
while avoiding non-specific interactions. Binding sites (on interfaces) exhibit a complex combination of geometric and chemical matches that determine complex formation specificity and binding stability.  
The identification of the most informative components among all these contributors relies on learning effective molecular representations. The advent of Deep Learning methods has significantly enhanced the 
prediction of molecular properties. 

In this context, we recently developed the Core Interacting Residues Network (CIRNet). CIRNet classifies pairs of amino acids as interacting residues at the core of the interfaces or 
non-interacting residues with an accuracy of approximately 0.82 on a balanced dataset [1].
The protocol is based on a compact representation of regions on the protein’s molecular and electrostatic potential surfaces through the Zernike 2D polynomials expansion, that allows 
for a rapid evaluation of shape [2] and electrostatic complementarity [3]. Additionally, the method incorporates a rapid hydropathy complementarity evaluation [4]
Figure 1 shows CIRNet architecture.

<img src="https://github.com/gretagrassmann/The-role-of-hydropathy-in-the-prediction-of-core-interacting-residues/blob/main/Figures/Figure1.png" width="700">


In this project you will use CIRNet to identify core interacting residues from a dataset of 905 protein dimers and analyse how different hydropathy scale influence its performance.


## Tasks

### Data download and preprocessing


## Bibliography
[1] G. Grassmann, et al. ‘Compact assessment of molecular surface complementarities enhances neural network-aided prediction of key binding residues’. arXiv preprint:2407.20992. 2024.

[2] E. Milanetti, et al. ‘2D Zernike polynomial expansion: Finding the protein-protein binding regions.’  Computational and structural biotechnology journal, 19, 29-36. 2021

[3] G. Grassmann, et al. ‘Electrostatic complementarity at the interface drives transient protein-protein interactions’. Scientific reports, 13(1), 10207. 2023.

[4] L. Di Rienzo, et al. ‘Characterizing hydropathy of amino acid side chain in a protein environment by investigating the structural changes of water molecules network.’ Frontiers in molecular biosciences 8: 626837. 2021.

