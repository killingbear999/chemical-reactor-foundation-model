# Accelerated Modeling of Various Chemical Reactors using Foundation Models: A Meta-Learning Approach with Physics-Informed Few-shot Adaptation using Reptile

Zihao Wang, Zhe Wu </br>
Paper:  </br>

**Requires: Python 3.11.3, Tensorflow Keras 2.13.0, Numpy, Sklearn** </br>
File description:
* Files in "Sine Wave Regression" folder are used to validate the performance of MAML and Reptile on modelling different sine waves. </br>
* Files in "Reactors" folder are used to model different types of reactors (i.e., including both normal data-driven method and physics-informed method), including continuous stirred-tank reactors (CSTRs), batch reactors (BRs), and plug flow reactors (PFRs). <br>
* Under "CSTR+Batch+PFR" folder
  1. CSTR_Batch_PFR_Reptile.ipynb is used to generate the foundation model for all three types of reactors using Reptile, model_reptile_cstr_batch.sav is the corresponding trained foundation model. </br>
  2. CSTR_Batch_PFR_Transfer.ipynb is used to generate the foundation model for all three types of reactors using transfer learning, model_transfer_cstr_batch.sav is the corresponding trained foundation model. </br>
