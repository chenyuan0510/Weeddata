# WeedData
A UAV hyperspectral dataset and code for rice seedling weed recognition to enable early classification of rice, Echinochloa crus-galli, and Leptochloa chinensis.

## Hyperspectral Data
We constructed three staggered-sowing field trials to build a temporally robust UAV hyperspectral dataset for early stage paddy weed recognition, covering rice , barnyard grass and Chinese sprangletop. All plots received consistent field management, and hyperspectral imagery was acquired uniformly at the 3-4 leaf seedling stage to reduce phenotypic bias and ensure reproducible spectral analysis and model training.

Dataset files are hosted on Quark Cloud Drive:
Batch 1 dataset: [Quark Batch1](https://pan.quark.cn/s/3b7a9eae33e3)
Batch 2 dataset: [Quark Batch2](https://pan.quark.cn/s/13fc4560e71e)
Batch 3 dataset: [Quark Batch3](https://pan.quark.cn/s/120b7309ea9d)

### Batch 1 Setup
Four pure Chinese sprangletop frames were sown on April 14, 2025. 15 pure rice frames and 15 pure barnyard grass frames were added on April 30. Three equal ratio two compartment mixed plots and two six compartment interlaced mixed plots were built to simulate natural crop weed coexistence. UAV hyperspectral data were collected on May 13 at the healthy 3-4 leaf seedling stage.

### Batch 2 Setup
Eight pure Chinese sprangletop frames were sown on May 20, 2025. 15 rice and 15 barnyard grass frames were planted on May 27 under unified management standards. Multi density mixed plots (three two compartment, two four compartment, one six compartment rice barnyard grass combinations) were constructed. Imagery was captured on June 10 at the 3-4 leaf stage to supply diverse mixed species training samples.

### Batch 3 Setup (Independent Cross Date Test Set)
Eight pure Chinese sprangletop frames were sown on June 11, 2025; 15 rice and 15 barnyard grass frames were supplemented on June 18. Imaging took place on June 27 at the 3-4 leaf stage. Due to unstable temperature and rainfall, two rice frames and all eight Chinese sprangletop frames exhibited poor growth. These abnormal samples were discarded from all subsequent processing and analysis.

### Data Partitioning & Experimental Merits
Imaging for all batches was performed at the critical 3-4 leaf seedling stage for early weed detection. Batch 1 and Batch 2 serve as training validation sets for preprocessing comparison, feature selection and model tuning. Batch 3 acts as a fully independent cross date test set to evaluate model generalization. The manually controlled plot design guarantees accurate species labels while preserving real world outdoor remote sensing noise, supporting systematic assessment of the complete weed recognition workflow.

## Workflow & How to Reproduce
### Background pixel removal: Run ‘Background pixel removal.ipynb’
Manually delineate plot regions, compute SAVI index, discard pixels where SAVI <0.36 (nonvegetation background pixels). Output clean vegetation pixel dataset.
### Data balancing: Run ‘Data balancing.ipynb’
Apply ROS, Kmeans stratified undersampling, CSMOTE resampling. Each class balanced to 1000 training samples. Resampling only applied to modeldevelopment set (Batch1+Batch2). Batch3 independent test set keeps original sample distribution without resampling.
### Classifier performance heatmap (Fig.3): Run ‘Heatmap of classifier performance.ipynb’
Evaluate nine spectral preprocessing methods × five classifiers with 5fold crossvalidation on training dataset; compute Macro Recall and plot performance heatmap.
### Preprocessingfeatureselection comparison (Fig.4A): Run ‘Preprocessing and feature selection.ipynb’
Compare 2 preprocessing strategies (SG, MinMax) combined with five featureselection algorithms (UVE, SPA, mRMR, SVMRFE, XGBoost); calculate mean Macro Recall and retained feature count for each combination. SGXGBoost is selected as optimal featureselection pipeline.
### Mean spectral curves & PCA plot (Fig.5A, Fig.5B): Run ‘Mean spectral curves and PCA score plot.ipynb’
Draw mean reflectance spectral curves for three species on 12 selected wavelengths; perform PCA transformation on 12band dataset and visualize sample distribution in PCA2D space.
### Wavelength significance & boxplot (Fig.5C): Run ‘Wavelength significance analysis and boxplot.ipynb’
Generate boxplots of reflectance for each selected wavelength; perform interspecies statistical significance test.
### R² coefficient of determination heatmap (Fig.7A): Run ‘coefficient of determination.ipynb’
Compute pairwise coefficient of determination (R²) among 12 selected wavelengths, analyze spectral redundancy between adjacent bands.
### All wavelength combinations screening (Fig.7B): Run ‘Results of all wavelength combinations.ipynb’
Generate all candidate sixband subsets from 12band pool via combinatorial search, evaluate each subset performance, count occurrence frequency of each wavelength within top5 highperformance combinations. Obtain final six representative bands.
### Optimal model pipeline (SGXGBoostPLSDA): Run ‘spectral preprocessing, feature selection and classifiers.ipynb’
Execute the complete experimental pipeline, including spectral preprocessing, XGBoostbased feature selection, and PLSDA classification. This script generates the optimal SGXGBoostPLSDA model and performs model evaluation using the independent crossdate test set from Batch 3.
### Confusion matrix (Fig.6): Run ‘Confusion matrix.ipynb’
Generate confusion matrices for 12band model and sixband model on Batch3 independent crossdate test dataset.
### ixband pixelwise spatial visualization (Fig.8B): Run ‘6band pixelwise visualization.ipynb’
Load sixband PLSDA model, predict pixels over controlled mixed ricebarnyard grass plots; output spatial classification map for mixedpixel scenario validation.
