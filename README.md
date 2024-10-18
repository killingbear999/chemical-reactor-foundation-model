# Foundation Model for Chemical Reactor Modeling

Zihao Wang, Zhe Wu
* Paper: https://arxiv.org/abs/2405.11752 (we are working on the updated version) </br>

**Requires: Python 3.11.3, Tensorflow Keras 2.13.0, Numpy, Sklearn, Pickle** </br>

### File description
* Files in "Sine Wave Regression" folder are used to validate the performance of MAML and Reptile on modelling different sine waves. </br>
* Files in "Reactors" folder are used to model different types of reactors (i.e., including both normal data-driven method and physics-informed method), including continuous stirred-tank reactors (CSTRs), batch reactors (BRs), and plug flow reactors (PFRs). <br>
* Under "CSTR+Batch+PFR" folder:
  1. CSTR_Batch_PFR_Reptile.ipynb is used to generate the foundation model using Reptile, model_reptile_cstr_batch.sav is the corresponding foundation model. </br>
  2. CSTR_Batch_PFR_Transfer.ipynb is used to generate the pre-trained model using transfer learning, model_transfer_cstr_batch.sav is the corresponding pre-trained model. </br>
  3. CSTR_Fewshots.ipynb, Batch_Fewshots.ipynb, PFR_Fewshots.ipynb are used to fine-tune the foundation models using few-shot learning to adapt to the new chemical reactions in unseen CSTRs, BRs, PFRs respectively. </br>
  4. CSTR_FewShots_Collocation.ipynb, Batch_FewShots_Collocation.ipynb, PFR_FewShots_Collocation.ipynb are used to investigate the relationship between few-shot performance in terms of testing MSE and the number of collocation points used in physics-informed-based modelling for CSTR-based, BR-based, PFR-based reactions respectively. </br>
* Under "photochemical reactor" folder:
  1. CSTR_Batch_PFR_Reptile_36timesteps.ipynb is used to generate the chemical reactor foundation model, model_reptile_cstr_batch_36timesteps.sav is the corresponding foundation model. </br>
  2. photochemical_reactor_36timesteps_7shots.ipynb is used to adapt the foundation model to an unseen reactor type, i.e., a photochemical reactor, with **real experiment data**. </br>

### Objective
* Constructs a foundation model for various chemical reactions in three classical generic reactors, including continuous stirred tank reactors (CSTRs), batch reactors (BRs), and plug flow reactors (PFRs), enabling rapid adaptation to **unseen chemical reactions** with a minimal number of samples from new tasks (i.e., few-shot adaptation)
* Demonstrates the adaptability of the foundation model to **unseen reactor types** (i.e., other than CSTRs, BRs, and PFRs, e.g., photochemical reacotr)

![alt text](https://github.com/killingbear999/chemical-process-foundation-model/blob/main/reptile.png)

### Scope
Within the scope of this work, we consider chemical reactors that can be described by material and energy balance equations (i.e., ODEs or PDEs).

### Methodology
Our methodology comprises two distinct phases: meta-training (i.e., meta-learning using **Reptile**) and meta-testing (i.e., **physics-informed** adaptation):

* During meta-training, we immerse the model (i.e., specifically, a recurrent neural network (RNN)) in a diverse array of chemical reactions across various reactor types. In short, the meta-training process involves two steps. First, we obtain the optimal parameters $W_\tau$ for a specific task $\tau$ using mean-squared error (MSE) and the Adam optimizer (i.e., since we are performing regression). Once $W_\tau$ is determined, we update the network parameters $\theta$ using the Reptile update rule: </br>
<p align=center> $\theta \leftarrow \theta + \epsilon(W_\tau - \theta)$ </br>

&ensp;&ensp;&ensp;&ensp; where $\epsilon$ is the meta-optimization step size hyperparameter. This process is then repeated recursively for all tasks.

* In the subsequent meta-testing phase, we select previously unseen chemical reactions and undertake physics-informed few-shot adaptations to them, respectively, leveraging the Reptile-based foundation model obtained during meta-training. It should be pointed out that we use few-shot data $x$ (i.e., $x \in X \subset D$, where $X$ denotes the set of few-shot training data) to compute the data-driven loss term $L_{d}$ and use collocation points $x_c$ (i.e., $x_c \in X_c \subset D$, where $X_c$ denotes the set of collocation data points, and $X \cap  X_c= \emptyset$) to compute the physics-informed loss term $L_{p}$ (i.e., a collocation point refers to a specific location within the domain of interest where the governing physics equations are enforced as part of the training process). As we do not utilize any labeled output data for $L_{p}$, there is no need to perform physical experiments to gather additional data, which remains consistent with our few-shot setting.

### Extension to integer orders of reactions
Using the outlined methodology, we train distinct foundation models corresponding to different integer orders of reactions. For an unseen reaction, we apply few-shot adaptation to each foundation model, generating intermediate models. Finally, we employ a minimum-voting strategy (selecting the model with the lowest validation/testing MSE) to produce the final adapted model, incorporating an ensemble learning approach.

![alt text](https://github.com/killingbear999/chemical-process-foundation-model/blob/main/ensemble.png)

### Extension to fractional orders of reactions
To further extend the foundation model to fractional orders of reactions, we expand the meta-training dataset to include random fractional orders. Once the foundation model is trained, we conduct meta-testing (few-shot adaptation) as usual.

### Extension to unseen reactor types
We have conducted real-world experiments to gather experiment data under 9 different conditions for a photochemical reactor, and 7 conditions (i.e., 7 trajectories or 7 shots) are used to fine-tune the foundation model and later test on the remaining 2 conditions (the experimental data and photochemical reactor design are confidential and considered proprietary to the company; therefore, they will not be made publicly available).

Our foundation model can rapidly adapt to previously unseen reactor types, such as photochemical reactors, offering several advantages:
* Few-Shot Adaptation: By leveraging the power of the foundation model and few-shot learning, we only need to conduct a limited number of experiments under a few conditions. This allows the model to effectively generalize to new conditions without extensive data collection. </br>
* Versatility Across Reactor Types: The model's adaptability is not limited to common reactor types like CSTR, BR, or PFR; it extends to other reactor types with minimal data requirements. </br>
* **No Dependence on First-Principles Models**: Unlike transfer learning, which requires deriving first-principles models to generate simulation datasets - a process that can take days - our foundation model is ready to use immediately. It can deliver impressive performance in just **a few seconds**, making it a practical and efficient solution. </br>

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
