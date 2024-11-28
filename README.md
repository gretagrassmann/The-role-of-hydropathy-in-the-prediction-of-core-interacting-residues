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
       - [VII TASK](#vii-task)
  * [Bibliography](#bibliography)

    

Understanding the molecular mechanisms underlying protein-protein interactions is fundamental for comprehending the basic mechanisms of cellular processes, with significant implications for therapeutic and biotechnological applications. Despite this, elucidating the binding process and the stability of the resulting complexes remains a challenging task.  
A protein structure is represented by a set of atomic coordinates, where each atom has an x, y, and z position in three-dimensional space. A dimer is defined as the (non-covalent) interaction between two protein structures, which together form a protein complex. Each protein comprises a certain number of residues (amino acids that make up the protein's polypeptide chain), but only a subset of these are considered interacting residues, referred to as binding sites. They are typically determined based on their distance from the residues of the partner protein. The prediction of binding site residues between two interacting proteins remains an open challenge in computational biology. Solving this challenge would enable protein-protein docking algorithms to more efficiently identify native-like solutions.  
In this context, we recently developed the Core Interacting Residues Network (CIRNet), a novel predictive approach for identifying interacting residues based on neural networks. CIRNet classifies pairs of residues as interacting residues at the core of the interfaces (central residues in the two binding sites) or non-interacting residues, achieving an accuracy of approximately 0.87 on a balanced dataset [1].  
The neural network was trained on a set of indicators providing a compact representation of protein regions, incorporating properties derived from both Coulomb potential and Lennard-Jones potential (which strongly influences the shape complementarity of interacting molecular surfaces). Specifically, the descriptors were obtained through the Zernike 2D polynomial expansion of molecular surface patches of the two interacting proteins, enabling rapid evaluation of shape [2] and electrostatic complementarity [3].  
More importantly for this project, the method integrates a rapid evaluation of hydrophobicity or hydrophilicity (here, we use the term "hydropathy") complementarity [4]. Over the decades, many hydropathy scales have been developed to numerically characterize the hydropathy profile of an amino acid. For each of these scales—some based on experimental data and others on statistical knowledge (theoretical scales)—each of the 20 natural amino acids is assigned a value quantifying its degree of hydropathy. The protocol implemented in CIRNet can accept, among its various descriptors, a hydropathy scale (a 20-dimensional vector, one value per amino acid) to compute the hydropathy complementarity between interacting residues within the two binding sites. Therefore, depending on the input hydropathy scale, CIRNet's performance in identifying residue-residue interacting pairs will vary.  
Thus, the main objective of this work is to investigate the impact of different types of hydropathy scales on CIRNet's ability to predict interacting residue pairs.


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
       ├── dataset.txt -> True classification, name of the complex, residue on the first protein (resA), residue on the second protein (resB) and its nine neighbors (resB_1,..resB_9)
       ├── shape.txt -> Shape complementarity between resA and resB and between resA and the nine neighbors of resB
       ├── el.txt -> Electrostatic complementarity between resA and resB and between resA and the nine neighbors of resB
       ├── hr.txt ->  Hydropathy complementarity (defined according to the L_hydrophobicity_scale)  between resA and resB and between resA and the nine neighbors of resB
       └── dist.txt -> Distance of resB_1,...,resB_9 from resB
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
hydropathy -> L_hydrophobicity_scale.csv [4] and other 27 scales 
```
The 27 scales were extracted from the  <a href="https://www.genome.jp/aaindex/" target="_blank">AAindex</a> database in Python, using the aaindex Python package.   
Each file is named as the AAindex  <a href="https://www.genome.jp/aaindex/AAindex/list_of_indices" target="_blank">accession number</a> of that scale.

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
Find the optimal threshold for the NN classification starting from these distributions. You can use any method of your choice.
           
#### II TASK
How does the accuracy on the Test dataset vary for different residues pairs type? You can find the residues names in the file dataset.txt. 
</div>

Residue can be classified according to their chemical nature in Polar (P), Hydrophobic (H), and Charged (C).</div> 

H = ['GLY', 'ALA', 'VAL', 'LEU', 'ILE','MET','PHE', 'TYR', 'TRP'] </div> 

P = ['SER', 'PRO','THR','CYS','ASN', 'GLN'] </div> 

C = ['HIS', 'LYS','ARG','ASP','GLU'] </div> 

The chemical composition of the interfaces is reflected in the hydropathy complementarity values between core interacting residue pairs (PP, HH, CC, PH, PC, HC). 

> :wink: **Tip**: To evaluate the accuracy you can compute the F1-score.

### Effect of new hydropathy scales
Study the effect of each one of the new hydropathy scales downloaded from Github. 

#### III TASK
For each scale:
1. Starting from the L_hydrophobicity_scale, hydropathy complementarity can be defined as in the Figure:
<img src="https://github.com/gretagrassmann/The-role-of-hydropathy-in-the-prediction-of-core-interacting-residues/blob/main/Figures/Figure2.png" width="700">

Define an appropriate hydropathy complementarity formula for the other hydropathy scales.
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
Compute a Principal Component Analysis (PCA) on all the 28 scales. Each PC will be defined by a combination of the 28 scales, and can be seen as a “new scale”.  
Analyze the explained variance for each PC to understand how many components are needed to capture most of the variance.  
For each of the selected PCs:
1. Define an appropriate hydropathy complementarity formula for this "new scale".
2. Train and test CIRNet.
3. Examine the loadings (contributions) of each scale to the PC.
4. Compare the results with what you obtained for the original proposed hydropathy scales. Are these linear combinations (maybe the ones with the highest explained variance) better than the original scales for the NN performance?


#### VII TASK
Try to improve the accuracy of CIRNet. You can start by using the best scale or PC, and then either modify the NN structure or add more specific thresholds to the NN prediction.



## Bibliography
[1] G. Grassmann, et al. ‘Compact assessment of molecular surface complementarities enhances neural network-aided prediction of key binding residues’. arXiv preprint:2407.20992. 2024.

[2] E. Milanetti, et al. ‘2D Zernike polynomial expansion: Finding the protein-protein binding regions.’  Computational and structural biotechnology journal, 19, 29-36. 2021.

[3] G. Grassmann, et al. ‘Electrostatic complementarity at the interface drives transient protein-protein interactions’. Scientific reports, 13(1), 10207. 2023.

[4] L. Di Rienzo, et al. ‘Characterizing hydropathy of amino acid side chain in a protein environment by investigating the structural changes of water molecules network.’ Frontiers in molecular biosciences 8: 626837. 2021.

