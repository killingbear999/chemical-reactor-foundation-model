# Accelerated Modeling of Various Chemical Reactors using Foundation Models: A Meta-Learning Approach with Physics-Informed Few-shot Adaptation using Reptile

Zihao Wang, Zhe Wu </br>
Paper:  </br>

**Requires: Python 3.11.3, Tensorflow Keras 2.13.0, Numpy, Sklearn** </br>
File description:
* Files in "Sine Wave Regression" folder are used to validate the performance of MAML and Reptile on modelling different sine waves. </br>
* Files in "Reactors" folder are used to model different types of reactors, including continuous stirred-tank reactors (CSTRs), batch reactors (BRs), and plug flow reactors (PFRs). <br>
* Under "CSTR+Batch+PFR" folder, CSTR_Batch_PFR_Reptile.ipynb is used to generate the foundation model for all three types of reactors using Reptile,
  model_reptile_cstr_batch.sav is the trained foundation model, and CSTR_Batch_PFR_FewShots.ipynb is used to evaluate the few-shot learning performance
  of the foundation model on unseen chemical reactions as compared with transfer learning-based pre-trained model. </br>
