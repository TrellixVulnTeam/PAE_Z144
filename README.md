# Regularized Normalizing Flow (RNF)

## Setup
We provide both a **`requirements.txt`** and **`rnf.yml`** that list all dependencies and can be used for either installing them with pip:   
```pip install -r requirements.txt```  
or creating a conda environment:  
```conda env create -f rnf.yml```   

The package itself can be installed with   
``` pip install -e .``` (from inside the RNF directory)

## Running the code
To display all the options for running the code, simply run        
```python main.py --helpfull```   
You can specify, e.g., the data set, the latent space dimensionality, the directories for storing the downloaded datasets, the module and model files. The model files can be read with tensorboard.

## Reproducing results
Our results are obtained in 3 steps  
1) train a (V)AE  
2) train a RealNVP on the encoded data  
3) analyse downstream tasks  
  a) measure reconstruction error and FID score  
  b) run Out-of-Distribution detection tests  

#### ad 1) (V)AE Training
The model trains by default on Fashion-MNIST, and all default parameters are set to reproduce our submitted results.
To reproduce one of our models trained on Fashion-MNIST, run, e.g.    
```python main.py --loss='AE' --latent-size=4 --model_dir='./my_model_dir' --module_dir='./my_module_dir'```   
To reproduce the results shown for the celeba dataset<sup>[1]</sup>, run   
```python main.py --loss='AE' --latent-size=32 --data_set='celeba' --learning_rate=0.005 --dropout_rate=0.2 --batch_size=64 --data_dir='/my_celeba_dir'```   

#### ad 2) Fitting the Real NVP
Once the VAE or AE has been trained, the fit of the RealNVP is performed in a jupyter notebook. We choose this setting, because it allows for easy on the spot visualization and analysis. To train the realNVP run and follow instructions in   
**`TrainNVP_fmnist.ipynb`** or **`TrainNVP_celeba.ipynb`**

#### ad 3a) Measurement of Reconstruction Errors and FID scores
The combined ((V)AE+RealNVP) model performance is analysed in   
**`FIDScore_and_Reconstruction_Error.ipynb`**

#### ad 3b) Out-of-Distribution Detection Tests
The inspection of the out-of-distribution detection is performed in the notebooks   
**`OOD_detection_no_density.ipynb`**   
**`OOD_detection_density.ipynb`**   
**`OOD_detection_ELBO.ipynb`**   

Finally, in **`SummaryPlots.ipynb`**, we gather results obtained with the above notebooks and produce summary plots.  

## Trained Models
The trained models that were used to obtain the submitted results can be obtained from https://zenodo.org/record/3668296 . For anonymization, all paths have been changed to relative paths, expecting that the modules are unzipped in the *modules*  subdirectory. If they are extracted in a different directory, the module paths (params['module_path']) will have to be adapated.

## Figure Index
A list of which notebooks were used to produce which figures in the draft. Most results are already saved in the parameter files, so running  **`SummaryPlots.ipynb`** reproduces the figures in the draft.
Run the other notebooks to overwrite these results.   
Fig 1) **`FIDScore_and_Reconstruction_Error.ipynb`**+**`SummaryPlots.ipynb`**  
Fig 2) **`FIDScore_and_Reconstruction_Error.ipynb`**  
Fig 3) **`FIDScore_and_Reconstruction_Error.ipynb`**  
Fig 4) **`FIDScore_and_Reconstruction_Error.ipynb`**  
Fig 5) **`FIDScore_and_Reconstruction_Error.ipynb`**+**`SummaryPlots.ipynb`**  
Fig 6) **`OOD_detection_no_density.ipynb`** +**`SummaryPlots.ipynb`**  
Fig 7) **`OOD_detection_no_density.ipynb`** +**`SummaryPlots.ipynb`**   
Fig 8) **`OOD_detection_density.ipynb`**  
Fig 9) **`OOD_detection_density.ipynb`**+**`OOD_detection_no_density.ipynb`**+**`SummaryPlots.ipynb`**  
Fig 10)**`OOD_detection_density.ipynb`**  
Fig 11)**`OOD_detection_no_density.ipynb`**+**`OOD_detection_ELBO.ipynb`**  

<sup>[1]</sup> : running the RNF on celeba will require downloading the celeba dataset (http://mmlab.ie.cuhk.edu.hk/projects/CelebA.html) and preprocessing it with the **`load_celeba_data`** function in **`rnf/load_data.py`**
