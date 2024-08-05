# Towards Foundation Model for Chemical Reactor Modeling: Meta-Learning with Physics-Informed Adaptation

Zihao Wang, Zhe Wu </br>
Paper: https://arxiv.org/abs/2405.11752 </br>

**Requires: Python 3.11.3, Tensorflow Keras 2.13.0, Numpy, Sklearn, Pickle** </br>

**File description**
* Files in "Sine Wave Regression" folder are used to validate the performance of MAML and Reptile on modelling different sine waves. </br>
* Files in "Reactors" folder are used to model different types of reactors (i.e., including both normal data-driven method and physics-informed method), including continuous stirred-tank reactors (CSTRs), batch reactors (BRs), and plug flow reactors (PFRs). <br>
* Under "CSTR+Batch+PFR" folder:
  1. CSTR_Batch_PFR_Reptile.ipynb is used to generate the foundation model for all three types of reactors using Reptile, model_reptile_cstr_batch.sav is the corresponding trained foundation model. </br>
  2. CSTR_Batch_PFR_Transfer.ipynb is used to generate the foundation model for all three types of reactors using transfer learning, model_transfer_cstr_batch.sav is the corresponding trained foundation model. </br>
  3. CSTR_Fewshots.ipynb is used to fine-tune the foundation models using few-shot learning to adapt to the new chemical reactions in unseen CSTRs, while CSTR_FewShots_Collocation is used to investigate the relationship between few-shot performance in terms of testing MSE and the number of collocation points used in physics-informed-based modelling for CSTR-based reactions. </br>
  4. Batch_Fewshots.ipynb is used to fine-tune the foundation models using few-shot learning to adapt to the new chemical reactions in unseen BRs, while Batch_FewShots_Collocation is used to investigate the relationship between few-shot performance in terms of testing MSE and the number of collocation points used in physics-informed-based modelling for BR-based reactions. </br>
  5. PFR_Fewshots.ipynb is used to fine-tune the foundation models using few-shot learning to adapt to the new chemical reactions in unseen PFRs, while PFR_FewShots_Collocation is used to investigate the relationship between few-shot performance in terms of testing MSE and the number of collocation points used in physics-informed-based modelling for PFR-based reactions. </br>

**Objective**
* Constructs a foundation model for various chemical reactions in three classical generic reactors, including continuous stirred tank reactors (CSTRs), batch reactors (BRs), and plug flow reactors (PFRs), enabling rapid adaptation to specific chemical reactions with a minimal number of samples from new tasks (i.e., few-shot adaptation)

![alt text](https://github.com/killingbear999/chemical-process-foundation-model/blob/main/reptile.png)

**Scope** </br>
Within the scope of this paper, we only considered elementary chemical reactions that transform reactant A to product B (irreversible reaction) for all three types of reactors that can be described by material and energy balance equations (i.e., ODEs or PDEs). Specifically, we consider CSTRs and BRs with a second-order reaction from A to B, and PFR with a first-order reaction from A to B to demonstrate the capability of the proposed modeling method for handling various elementary reactions with different orders and in different reactors.

**Methodology** </br>
Our methodology comprises two distinct phases: meta-training (i.e., meta-learning using **Reptile**) and meta-testing (i.e., **physics-informed** adaptation):

* During meta-training, we immerse the model (i.e., specifically, a recurrent neural network (RNN)) in a diverse array of chemical reactions across various reactor types. In short, the meta-training process involves two steps. First, we obtain the optimal parameters $W_\tau$ for a specific task $\tau$ using mean-squared error (MSE) and the Adam optimizer (i.e., since we are performing regression). Once $W_\tau$ is determined, we update the network parameters $\theta$ using the Reptile update rule: </br>
<p align=center> $\theta \leftarrow \theta + \epsilon(W_\tau - \theta)$ </br>

&ensp;&ensp;&ensp;&ensp; where $\epsilon$ is the meta-optimization step size hyperparameter. This process is then repeated recursively for all tasks.

* In the subsequent meta-testing phase, we select previously unseen chemical reactions and undertake physics-informed few-shot adaptations to them, respectively, leveraging the Reptile-based foundation model obtained during meta-training. It should be pointed out that we use few-shot data $x$ (i.e., $x \in X \subset D$, where $X$ denotes the set of few-shot training data) to compute the data-driven loss term $L_{d}$ and use collocation points $x_c$ (i.e., $x_c \in X_c \subset D$, where $X_c$ denotes the set of collocation data points, and $X \cap  X_c= \emptyset$) to compute the physics-informed loss term $L_{p}$ (i.e., a collocation point refers to a specific location within the domain of interest where the governing physics equations are enforced as part of the training process). As we do not utilize any labeled output data for $L_{p}$, there is no need to perform physical experiments to gather additional data, which remains consistent with our few-shot setting.

## Citation </br>
If you find our work relevant to your research, please cite:
```
@article{wang2024foundation,
  title={Foundation Model for Chemical Process Modeling: Meta-Learning with Physics-Informed Adaptation},
  author={Wang, Zihao and Wu, Zhe},
  journal={arXiv preprint arXiv:2405.11752},
  year={2024}
}
```
