# Classical machine learning approach for photodynamics properties prediction of photoredox catalyst

Photocatalysis is a rapidly developing area of chemistry in which the creation of new substances capable of converting the energy of visible light into reaction energy is a fundamental task.

Classical approaches based on human intuition are often quite complex, and quantum mechanical calculations of the photodynamic properties of molecules are cumbersome and time-consuming. Machine learning approaches solve time problems, but they work only for organic compounds.
Our repository introduces Python notebooks and fine-tuned models that can predict photodynamic properties like absorption, emission wavelength, and quantum yield of fluorescence for not only organic compounds but also metalloroganic and metalorganic complexes.

![Graphical_abstract _4](https://github.com/Yagr49/Photocatalyst_NN/assets/139890239/baa7d028-31b1-4d2f-9d0d-e23b00215197)





## Preparation of data

The list of organic molecules, solvents, absorption wavelengths, emission wavelengths, and quantum yields was taken from [paper](https://www.nature.com/articles/s41597-020-00634-8). All corresponding structures were optimized by [GFN2-xTB](https://xtb-docs.readthedocs.io/en/latest/) functional.

The same parameters for metal complexes and photocatalysts were taken from [paper](https://www.thieme-connect.com/products/ejournals/abstract/10.1055/a-1390-9065). All corresponding structures were optimized by [GFN2-xTB](https://xtb-docs.readthedocs.io/en/latest/) functional.

Our model studied on classical chemical descriptors, as Morgan Fingerprints, MACCS Fingerprints, prepared by [RDKIT](https://www.rdkit.org/), for metal complex we use combination of SLATM (from [qml](https://pypi.org/project/qml/)), Coulomb Matrix (from [openbabel](https://pypi.org/project/openbabel/#:~:text=Project%20description,-This%20is%20a&text=Open%20Babel%20is%20a%20chemical,%2C%20biochemistry%2C%20or%20related%20areas.)) and Bag of Bonds (from [mol](https://pypi.org/project/molml/)), additionaly Coulomb matrix and Bag of Bonds were subjected to a [PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) procedure due to the large size of the data. All preparations of datasets we publish in repository (named it) and conclude information about the target molecule and the solvent in which the measurement experiment was carried out. 

Persistence Barcodes features (sum, mean, std, entropy of H0, H1, H2) were prepared by [gitto-tda](https://giotto-ai.github.io/gtda-docs/0.5.1/library.html). The way of preparation: the 3D structure of compounds was transformed into a point cloud, then [giotto](https://giotto-ai.github.io/gtda-docs/0.5.1/library.html) prepared persistence barcodes for H0, H1, H2 homologies. Sum, mean, standard deviation, and entropy were calculated from the resulting barcodes for all homologies.

![persistent_homology](https://github.com/Yagr49/Photocatalyst_NN/assets/139890239/330e2b50-c959-449f-b662-83aff6bdc519)

A combination of Coulomb matrix and MorganFingerprints for metal complexes was prepared by taking 10 atoms Coulomb matrix and ligand's SMILES transform to MorganFingerprints. The resulting metal complexes contain information about the environment of the largest atom and the ligands of the metal complexes; adding topological features allows you to enter information about the entire structure.

![Coulomb_matrix_pict](https://github.com/Yagr49/Photocatalyst_NN/assets/139890239/9c7e2980-3461-4e02-81f9-d99bcfe62748)




## Selection of descriptors and models

We evaluated the most popular classical machine learning methods (Kernel Ridge Regression, Decision Trees, and Gradient Boosting) using a combination of chemical descriptors for quantum yield, emission wavelength, and absorption wavelength predictions. The SLATM and Coulomb matrix combination with topologies, features, and fingerprints yielded the lowest errors.


![Heatmap_abs](https://github.com/Yagr49/Photocatalyst_NN/assets/139890239/8d5e8bf5-8429-4dc9-8422-62174ddbf842)


## Check model on organic compounds
As lead features, we choose Coulomb Matrix + MorganFingerprints + topology and SLATM + topology. The image below displays the correlation and mean absolute error (MAE) for the final model, which is the best result we were able to get using the [Optuna](https://arxiv.org/abs/1907.10902)  on [XGBoost](https://xgboost.readthedocs.io/en/stable/) and [CatBoost](https://catboost.ai/) Regressor models. The same metrics are shown by SLATM and Coulomb combinations, yet the Coulomb matrix is marginally superior. Because the structure of the ligands must be obtained manually, a model based on the SLATM combination will be constructed more easily for future use than the Coulomb Matrix combination.

![Picture_corr](https://github.com/Yagr49/Photocatalyst_NN/assets/139890239/df0e7113-a6bf-414d-968c-1b0609a6f06f)

## Validate approach on metal complexes

On metall complexes, we verified the optimal methodology, and the [CatBoost](https://catboost.ai/) model yielded the best outcome. The image below shows the RMSE and MAE for each machine learning strategy, and error from quantum calculation for absorption wavelength prediction carried out by [ORCA](https://orcaforum.kofo.mpg.de/).

![Metal_error](https://github.com/Yagr49/Photocatalyst_NN/assets/139890239/195d10db-f8b1-4a07-a8e9-6308ffc774e7)

## How repository work

Datasets and trained models are available at [https://ibm.box.com/v/MoLFormer-data](https://drive.google.com/drive/u/2/folders/1KzQ7TYHFC6to7_y65IolNAQtIaz7IJ-O)

```bash
MSU_AI_Photocatal/
└── Models
    ├── CatBoost_Absorption_metal_CoulombMatrix.cbm                   # CatBoost model for Absorption wavelenght prediction trained on combination Coulomb Matrix 10x10 + ligand MorganFingerptints + topologies feautures 
    ├── CatBoost_Absorption_metal_SLATM.cbm                           # CatBoost model for Absorption wavelenght prediction trained on combination SLATM + topologies feautures 
    ├── CatBoost_Emission_metal_CoulombMatrix.cbm                     # CatBoost model for Emission wavelenght prediction trained on combination Coulomb Matrix 10x10 + ligand MorganFingerptints + topologies feautures 
    ├── CatBoost_Emission_metal_SLATM.cbm                             # CatBoost model for Emission wavelenght prediction trained on combination SLATM + topologies feautures 
    ├── Catboost_Quantum_Yield_org_compound_CoulombMatrix.cbm         # CatBoost model for Quantum Yield prediction trained on combination Coulomb Matrix 10x10 + ligand MorganFingerptints + topologies feautures 
    ├── Catboost_Quantum_Yield_org_compound_SLATM.cbm                 # CatBoost model for Quantum Yield prediction trained on combination SLATM + topologies feautures 
    ├── XGB_Absorption_org_compound_CoulombMatrix.json                # XGBoost model for Emission wavelenght prediction trained on combination Coulomb Matrix 10x10 + ligand MorganFingerptints + topologies feautures
    ├── XGB_Absorption_org_compound_SLATM.json                        # XGBoost model for Emission wavelenght prediction trained on combination SLATM + topologies feautures
    ├── XGB_Emission_org_compound_CoulombMatrix.json                  # XGBoost model for Emission wavelenght prediction trained on combination Coulomb Matrix 10x10 + ligand MorganFingerptints + topologies feautures
    ├── XGB_Emission_org_compound_SLATM.json                          # XGBoost model for Emission wavelenght prediction trained on combination SLATM + topologies feautures
└── Datasets
    └── Coordinates_xyz
        ├── Metal_complexes_optimize_XTB_coordinates.zip              # Archive of GFN2-xTB optimized metal complexes structures in .xyz format 
        ├── Metal_complexes_solvents_optimize_XTB_coordinates.zip     # Archive of GFN2-xTB optimized metal complex's solvents structures in .xyz format
        ├── Organic_compounds_optimize_MM_coordinates.zip             # Archive of Molecular Mechanics optimized organic compounds structures in .xyz format
        ├── Organic_compounds_optimize_XTB_coordinates.zip            # Archive of GFN2-xTB optimized organic compounds structures in .xyz format
        ├── Organic_compounds_solvents_optimize_XTB_coordinates.zip   # Archive of GFN2-xTB optimized organic compound's solvent structures in .xyz format
    └── Metal_complexes_descriptors
        ├── Metal_complexes_ligands_SMILES.xlsx                       # Dataset of SMILES of ligands corresponding metal complex
    └── Organic_compounds_descriptors
        ├──
    └── Target_dataset
        ├── Metal_complexes_dataset.xlsx                              # Dataset consist Absorption, Emission wavelenght, Molecular weight and SMILES of solvents for metal complex
        ├── Organic_compound_final_dataset.xlsx                       # Dataset consist Imputed Absorption, Emission wavelenght, Quantum Yield, Molecular weight and SMILES of solvent for organic compounds
        ├── Organic_compound_final_dataset.xlsx                       # Dataset consist Absorption, Emission wavelenght, Quantum Yield, Molecular weight and SMILES of solvent for organic compounds from [paper](https://www.nature.com/articles/s41597-020-00634-8)
```

For descriptors preparation you should run notebooks from /notebooks/preparing_datasets folder. Path for input file load from corresponding Google Drive.

For models validation for organic compounds you should run notebooks from /notebooks/validation folder. Path for input file load from corresponding Google Drive.

To get trained models for organic compounds you should run notebooks from /notebooks/optimization folder or load trained models . Path for input file load from corresponding Google Drive.

To get trained models for metal complexes you should run notebooks from /src/optimization folder or load trained models . Path for input file load from corresponding Google Drive.

The enviroment for run notebooks locally download `enviroment.yml` from /enviroment folder.
        
## Acknowledgements

**Work is greatly supported by Non-commercial Foundation for the Advancement of Science and Education INTELLECT, and my mentor, Sergey Kolpinskiy**
