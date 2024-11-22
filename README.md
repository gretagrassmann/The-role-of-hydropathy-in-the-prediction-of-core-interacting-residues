# The role of hydropathy in the prediction of core interacting residues

 <div align="justify"> 


**For more details see <a href="https://arxiv.org/abs/2407.20992" target="_blank">CIRNet</a>, <a href="https://www.sciencedirect.com/science/article/pii/S2001037020305183" target="_blank">Zernike shape</a>, <a href="https://www.nature.com/articles/s41598-023-37130-z" target="_blank">Zernike electrostatic</a>, and
<a href="https://www.frontiersin.org/articles/10.3389/fmolb.2021.626837/full" target="_blank">Hydrophobicity</a>.**


### Table of contents:
  * [Project steps](#project-steps)
    + [Data download](#data-download)
    + [CIRNet training with the original data](#cirnet-training-with-the-original-data)
    + [CIRNet evaluation with the original data](#cirnet-evaluation-with-the-original-data)
       - [I TASK](#i-task)
       - [II TASK](#ii-task)
    + [Effect of new hydropathy scales](#effect-of-new-hydropathy-scales)
       - [III TASK](#iii-task)
       - [IV TASK](#iv-task)
       - [V TASK](#v-task)
       - [VI TASK](#vi-task)
  * [Bibliography](#bibliography)

    

Understanding the molecular mechanisms underlying protein-protein interactions is fundamental to molecular biology, with significant implications for therapeutic and biotechnological applications. 
Despite this, understanding the binding process and the stability of the resulting complexes is still a challenge. Binding partners must find each other amid thousands of other types of molecules in the crowded cell
while avoiding non-specific interactions. Binding sites (on interfaces) exhibit a complex combination of geometric and chemical matches that determine complex formation specificity and binding stability.  
The identification of the most informative components among all these contributors relies on learning effective molecular representations. The advent of Deep Learning methods has significantly enhanced the 
prediction of molecular properties. 

In this context, we recently developed the Core Interacting Residues Network (CIRNet). CIRNet classifies pairs of amino acids as interacting residues at the core of the interfaces or 
non-interacting residues with an accuracy of approximately 0.82 on a balanced dataset [1].
The protocol is based on a compact representation of regions on the protein’s molecular and electrostatic potential surfaces through the Zernike 2D polynomials expansion, that allows 
for a rapid evaluation of shape [2] and electrostatic complementarity [3]. Additionally, the method incorporates a rapid hydropathy complementarity evaluation [4].
Figure 1 shows CIRNet architecture.

<img src="https://github.com/gretagrassmann/The-role-of-hydropathy-in-the-prediction-of-core-interacting-residues/blob/main/Figures/Figure1.png" width="700">


In this project you will use CIRNet to identify core interacting residues from a dataset of protein dimers and analyse how different hydropathy scale influence its performance.


## Project steps

### Data download
Download the directories: </div>

**CODES**: all the starting python codes.
 ```
codes
 └──cnn.py -> CIRNet architecture
```
</div>

 **DATASET**: training and testing data.
 ```
dataset
 ├── train
       ├── classification.txt -> True classification of the residue pair as core interacting (1) or not (0)
       ├── dataset.txt -> True classification, name of the complex, residue on the first protein (A_n), residue on the second protein (B_n)
       ├── shape.txt -> Shape complementarity between A_n and B_n and between A_n and the nine neighbors of B_n (B_n,B_n_1,...,B_n_10)
       ├── el.txt -> Electrostatic complementarity between A_n and B_n and between A_n and the nine neighbors of B_n
       ├── hr.txt ->  Hydropathy complementarity (defined according to the L_hydrophobicity_scale)  between A_n and B_n and between A_n and the nine neighbors of B_n
       └── dist.txt -> Distance of B_n,B_n_1,...,B_n_10 from B_n
 └── test
       ├── classification.txt 
       ├── dataset.txt
       ├── shape.txt 
       ├── el.txt
       ├── hr.txt
       └── dist.txt
```
</div>

**HYDROPATHY**: Original and new hydropathy scales
```
hydropathy
  ├── L_hydrophobicity_scale.csv [4]
  ├── K_hydrophobicity_scale.csv [5]
  ├── H_hydrophobicity_scale.csv [6]
  ├── E_hydrophobicity_scale.csv [7]
  ├── R_hydrophobicity_scale.csv [8]
  └── W_hydrophobicity_scale.csv [9]
```

### CIRNet training with the original data
    
Use cnn.py to train CIRNet on the original dataset **(based on the L_hydrophobicity_scale)**. Save the trained network.

### CIRNet evaluation with the original data
Evaluate CIRNet performance on the test dataset **(based on the L_hydrophobicity_scale)**. 
</div>

Load the test dataset with the function you can find in cnn.py
```
test_data, classification_test_data = load_full_dataset(test_data_path)
```
Test the saved NN
```
nn_model = tf.keras.models.load_model('YOUR_PATH/my_model.keras')
nn_prediction = nn_model.predict(test_data)
```

#### I TASK
Plot the distributions of the NN prediction for true interacting and non interacting residues. 
Find the optimal threshold for the NN classification starting from these distributions.
           
#### II TASK
How does the accuracy on the Test dataset vary for different residues pairs type? You can find the residues names in the file dataset.txt. 
</div>

Residue can be classified according to their chemical nature in Polar (P), Hydrophobic (H), and Charged (C).</div> 

H = ['GLY', 'ALA', 'VAL', 'LEU', 'ILE','MET','PHE', 'TYR', 'TRP'] </div> 

P = ['SER', 'PRO','THR','CYS','ASN', 'GLN'] </div> 

C = ['HIS', 'LYS','ARG','ASP','GLU'] </div> 

The chemical composition of the interfaces is reflected in the hydropathy complementarity values between core interacting residue pairs (PP, HH, CC, PH, PC, HC). 

> :wink: **Tip**: To evaluate the accuracy you can compute the ROC AUC between the distributions of true and false cases.

### Effect of new hydropathy scales
Study the effect of each one of the new hydropathy scales downloaded from Github. 

#### III TASK
For each scale:
1. Starting from the L_hydrophobicity_scale, hydropathy complementarity can be defined as in the Figure:
<img src="https://github.com/gretagrassmann/The-role-of-hydropathy-in-the-prediction-of-core-interacting-residues/blob/main/Figures/Figure2.png" width="700">

Define an appropriate hydropaty complementarity formula for the other hydropathy scales.
> :wink: **Tip**: Residues with similar hydropathy values (both high or low) have stronger interactions compared to those between residues with opposing hydropathy characteristics.

2. Write new hr.txt files using these new values. Use the residues names contained in the dataset.txt file.

3. Train CIRNet on the provided dataset using the new hydropathy values and test it on the test dataset.

#### IV TASK
Starting from the saved NN for each scale:
1. Plot the distributions of the NN prediction for true interacting and non interacting residues. Find the optimal threshold for the NN classification starting from these distributions.
2. How does the accuracy on the test dataset vary for different residues pairs type? You can consider only A_n and B_n

#### V TASK
Compare the results obtained for all the proposed hydropathy scales.

#### VI TASK
Try to improve the accuracy of CIRNet. You can either modify the NN structure or add more specific thresholds to the NN prediction.



## Bibliography
[1] G. Grassmann, et al. ‘Compact assessment of molecular surface complementarities enhances neural network-aided prediction of key binding residues’. arXiv preprint:2407.20992. 2024.

[2] E. Milanetti, et al. ‘2D Zernike polynomial expansion: Finding the protein-protein binding regions.’  Computational and structural biotechnology journal, 19, 29-36. 2021.

[3] G. Grassmann, et al. ‘Electrostatic complementarity at the interface drives transient protein-protein interactions’. Scientific reports, 13(1), 10207. 2023.

[4] L. Di Rienzo, et al. ‘Characterizing hydropathy of amino acid side chain in a protein environment by investigating the structural changes of water molecules network.’ Frontiers in molecular biosciences 8: 626837. 2021.

[5] J. Kyte, et al. ‘A simple method for displaying the hydropathic character of a protein.‘ Journal of molecular biology 157.1: 105-132. 1982.

[6] T. P. Hopp, et al. ‘Prediction of protein antigenic determinants from amino acid sequences‘. Proceedings of the National Academy of Sciences, 78(6), 3824–3828. 1981.

[7] Eisenberg, D., et al. ‘Hydrophobic moments and protein structure‘. Faraday Symposia of the Chemical Society, 17, 109–120. 1984.

[8] M. A. Roseman. ‘Hydrophilicity of polar amino acid side-chains is markedly reduced by flanking peptide bonds‘. Journal of Molecular Biology, 200(4), 513–522. 1988.

[9] W. C. Wimley, et al. ‘Experimentally determined hydrophobicity scale for proteins at membrane interfaces‘. Nature Structural Biology, 3(10), 842–848. 1996.




