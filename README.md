# Classical machine learning approach for photodynamics properties prediction of photoredox catalyst

Photocatalysis is a rapidly developing area of chemistry, in which the creation of new substances capable of converting the energy of visible light into reaction energy is a fundamental task.

Classical approaches based on human intuition are often quite complex, and quantum mechanical calculations of the photodynamic properties of molecules are cumbersome and time-consuming. Machine learning approach solve time problem, but that work only for organic compound. 
Our repositorie introduce python notebooks and fine-tubbed models, that can predict photodynamics properties like a absorprion, emission wavelength and quantum yield of fluorescence not only organic compounds, but metalloroganic and metalorganic complex.
![Graphical_abstract](https://github.com/Yagr49/Photocatalyst_NN/assets/139890239/56fd1668-7e40-41e9-8211-280dda4786e0)


## Preparation of data

The list of organic molecules, solvents, absorption wavelength, emission wavelength and quantum yields were taken from [paper](https://www.nature.com/articles/s41597-020-00634-8). All corresponds structures was optimized by GFN2-xTB functionals.

Similar parameters of metal complexes and photocatalysts were taken from [paper](https://www.thieme-connect.com/products/ejournals/abstract/10.1055/a-1390-9065). All corresponds structures was optimized by GFN2-xTB functionals.

Our model studied on classical chemical descriptors, as a Morgan Fingerprints, MACCS Fingerprints, prepared by RDKIT packet, for metal complex we use combination of SLATM (from qml packet), Coulomb Matrix (from openbabel packet) and Bag of Bonds (from chemreps packet), additionaly Coulomb matrix and Bag of Bonds were subjected a PCA procedure due to the large size of the data. All preparations of datasets we publish in repository (named it) and conефшты information about the target molecule and the solvent in which the measurement experiment was carried out. 

Persistence Barcodes features (sum, mean, std, entropy of H0, H1, H2) were prepared by ripser package. The way of preparation: the 3D structure of compound was transform for a point cloud, then ripser package prepared persistence barcodes for H0, H1, H2 homologies. From resulting barcodes were prepared sum, mean, std and entropy for all homologies.

![persistent_homology](https://github.com/Yagr49/Photocatalyst_NN/assets/139890239/8ddfc70c-d995-4855-a8ce-8bf8a5da981c)

## Model Validation


