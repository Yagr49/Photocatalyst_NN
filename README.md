# Classical machine learning approach for photodynamics properties prediction of photoredox catalyst

Photocatalysis is a rapidly developing area of chemistry, in which the creation of new substances capable of converting the energy of visible light into reaction energy is a fundamental task.

Classical approaches based on human intuition are often quite complex, and quantum mechanical calculations of the photodynamic properties of molecules are cumbersome and time-consuming. Machine learning approach solve time problem, but that work only for organic compound. 
Our repositorie introduce python notebooks and fine-tubbed models, that can predict photodynamics properties like a absorprion, emission wavelength and quantum yield of fluorescence not only organic compounds, but metalloroganic and metalorganic complex.

![Graphical_abstract _4](https://github.com/Yagr49/Photocatalyst_NN/assets/139890239/baa7d028-31b1-4d2f-9d0d-e23b00215197)





## Preparation of data

The list of organic molecules, solvents, absorption wavelength, emission wavelength and quantum yields were taken from [paper](https://www.nature.com/articles/s41597-020-00634-8). All corresponds structures was optimized by GFN2-xTB functionals.

Similar parameters of metal complexes and photocatalysts were taken from [paper](https://www.thieme-connect.com/products/ejournals/abstract/10.1055/a-1390-9065). All corresponds structures was optimized by GFN2-xTB functionals.

Our model studied on classical chemical descriptors, as a Morgan Fingerprints, MACCS Fingerprints, prepared by RDKIT packet, for metal complex we use combination of SLATM (from qml packet), Coulomb Matrix (from openbabel packet) and Bag of Bonds (from chemreps packet), additionaly Coulomb matrix and Bag of Bonds were subjected a PCA procedure due to the large size of the data. All preparations of datasets we publish in repository (named it) and conclude information about the target molecule and the solvent in which the measurement experiment was carried out. 

Persistence Barcodes features (sum, mean, std, entropy of H0, H1, H2) were prepared by ripser package. The way of preparation: the 3D structure of compound was transform for a point cloud, then ripser package prepared persistence barcodes for H0, H1, H2 homologies. From resulting barcodes were prepared sum, mean, std and entropy for all homologies.

![persistent_homology](https://github.com/Yagr49/Photocatalyst_NN/assets/139890239/330e2b50-c959-449f-b662-83aff6bdc519)

A combination of Coulomb matrix and MorganFingerprints was prepared by taking 10 atoms part of metal complex Coulomb matrix and ligand's MorganFingerprints. The resulting metal complexes contain information about the environment of the largest atom and the ligands of the metal complexes; adding topological features allows you to enter information about the entire structure.

![Coulomb_matrix_pict](https://github.com/Yagr49/Photocatalyst_NN/assets/139890239/9c7e2980-3461-4e02-81f9-d99bcfe62748)




## Model Validation

We tested the most widely used classical machine learning algorithms (gradient boosting, decision trees, KernelRidgeRegresson) on a combination of chemical descriptors for absorption wavelength, emission wavelength, and quantum yield.


![Heatmap_abs](https://github.com/Yagr49/Photocatalyst_NN/assets/139890239/8d5e8bf5-8429-4dc9-8422-62174ddbf842)


## Check model on organic compounds
We took SLATM + topology and Coulomb Matrix + MorganFingerprints + topology as lead features. The best result we getted on XGBoost and CatBoost Regressor model with Optuna Package, the picture below demostrate coorelation and MAE for resulting model. SLATM and Coulomb demostrate the same metrics, but Coulomb matrix slightly better. For future using model SLATM prepared more easily, Coulomb Matrix combination.

![Picture_corr](https://github.com/Yagr49/Photocatalyst_NN/assets/139890239/df0e7113-a6bf-414d-968c-1b0609a6f06f)

## Validate approach on metal complexes

We validate the best approach on a small metall complex, CatBoost model perform the best result. SLATM and Coulomb Matrix approach show the same result. We compare machine learning approach result with Quantum calculation result for absorption, the picture below demostrate RMSE and MAE for each approach.

![Metal_error](https://github.com/Yagr49/Photocatalyst_NN/assets/139890239/81dad4df-7b24-46cc-a0c1-87ebcc445a5c)



