# Comparison of Continual Learning Methods
Continual Learning (CL) is a machine learning paradigm that focuses on training models to learn continuously from a stream of data, without forgetting previously learned knowledge. In the context of image classification tasks, CL involves training models to sequentially learn from multiple datasets or tasks over time, adapting to new data while retaining the ability to perform well on previously encountered tasks.

## Implementation

The datasets are strategically partitioned into multiple tasks, each representing a distinct subset of the data or a unique classification challenge. The number of tasks can be customized based on user preferences. During training, the neural network is sequentially exposed to each task, learning from the specific subset of data associated with it. The testing phase involves evaluating the model's performance not only on the current task but also on previously encountered tasks. This iterative process facilitates the creation of a model with consistent performance across all tasks, thus ensuring its suitability for diverse domains and novel classes.

## Continual Learning Methods

1. **Fine-tuning**: It involves training models sequentially, one task after the other. The model is initially trained on the first task and then fine-tuned on subsequent tasks, adjusting its parameters to learn new patterns while preserving knowledge from previous tasks. This technique is to know the lower-bound performance of the neural network across multiple tasks.

2. **Joint-datasets**: In this approach, all datasets are combined into a single training set, and the model is trained jointly on all tasks simultaneously. This method aims to leverage the diversity of the datasets to improve generalization and adaptability. This technique is to know the upper-bound performance of the neural network across multiple tasks.

3. **Rehearsal**: It involves selecting random samples from previous tasks and adding them to the current task dataset during training. This method helps mitigate catastrophic forgetting by allowing the model to periodically revisit and reinforce its knowledge of past tasks while learning new ones. 
   
4. **Elastic Weight Consolidation (EWC)**: EWC is a regularization technique that mitigates catastrophic forgetting by preserving important parameters learned during previous tasks. It achieves this by penalizing changes to critical weights based on their importance for previous tasks.

5. **Learning without Forgetting (LwF)**: LwF addresses forgetting by distilling knowledge from the previous model onto the current model during training on new tasks. It does so by using the previous model's predictions as soft targets to guide the learning process. Additionally, an alternative training approach involves incorporating an auxiliary network that is optimized for the current task [1]. This results in a loss function comprising both a stability term (based on the previous network) and a plasticity term (related to the auxiliary network).

6. **Bilateral Memory Consolidation (BiMeCo)**: BiMeCo incorporates two neural networks: a short-term network and a long-term network. The short-term network is designed for rapid learning from new tasks, while the long-term network serves as a repository for storing essential information from previous tasks. The memory consolidation process involves knowledge distillation and feature extraction, facilitating the transfer of knowledge from the short-term network to the long-term network while minimizing interference with existing knowledge [2].

7. **BiMeCo + LwF**: This approach combines BiMeCo with LwF, leveraging the strengths of both methods to enhance performance and mitigate forgetting.

## Run the code

To initiate training using various continual learning methods and apply multiple techniques, please follow these instructions:

1. Clone this repository to your local machine.
  ```bash
  https://github.com/pascutc98/continual-learning-methods
  cd continual-learning-methods
  ```
2. Create and activate a conda environment:
  ```bash
  conda create -n cl_methods python=3.8
  conda activate cl_methods
  ```
3. Install the required dependencies by using the provided `requirements.txt` file:
  ```bash
  pip install -r requirements.txt
  ```
4. Execute the file ```run_main.sh``` or ```run main.py``` directly. You can modify the input parameters as needed:
  ```bash
  bash run_main.sh
  ```
  ```bash
  python main.py
  ```
## Input parameters

Here's detailed information about the input parameters:

- General Parameters:
    - ```exp_name```: Name of the experiment or project.
    - ```seed```: Random seed for reproducibility.
    - ```epochs```: Number of training epochs.
    - ```lr```: Learning rate for optimization.
    - ```lr_decay```: Learning rate decay factor.
    - ```lr_patience```: Number of epochs to wait before reducing the learning rate.
    - ```lr_min```: Minimum learning rate threshold.
    - ```batch_size```: Batch size for training.
    - ```num_tasks```: Number of tasks in the continual learning setup.
      
- Dataset Parameters
    - ```dataset```: Choice of dataset for experimentation (e.g., mnist, cifar10, cifar100, cifar100-alternative-dist).
        - ```mnist```: Datasets used are MNIST and Fashion MNIST. This option configures the number of tasks to 2 by default.
        - ```cifar10```: Dataset used is CIFAR-10. The number of tasks can be customized according to user preferences.
        - ```cifar100```: Dataset used is CIFAR-100. The number of tasks can be customized according to user preferences.
        - ```cifar100-alternative-dist```: Dataset used is CIFAR-100. This option sets the number of tasks to 2. Each task exhibits a distinct data distribution: Task 1 comprises 80 classes, while Task 2 includes 20 classes. Moreover, there is a memory leakage of 5% of data from each class of Task 2 into Task 1.
      
- EWC Parameters
    - ```ewc_lambda```: Regularization parameter for Elastic Weight Consolidation (EWC).
      
- Distillation Parameters (LwF)
    - ```lwf_lambda```: Hyperparameter controlling the importance of distillation loss in Learning without Forgetting (LwF).
    - ```lwf_aux_lambda```: Hyperparameter controlling the importance of auxiliary distillation loss in LwF.
      
- BiMeCo Parameters
    - ```memory_size```: Size of the memory buffer which stores samples from previous tasks in Bilateral Memory Consolidation (BiMeCo).
    - ```bimeco_lambda_short```: Regularization parameter for short-term network in BiMeCo.
    - ```bimeco_lambda_long```: Regularization parameter for long-term network in BiMeCo.
    - ```bimeco_lambda_diff```: Regularization parameter controlling the difference between the feature extractors of short-term and long-term networks in BiMeCo.
    - ```m```: Momentum parameter for updating the model parameters.

Understanding these parameters will allow you to customize the training process and experiment with different configurations to achieve optimal results. For more information about these parameters, you can run the following command: 
  ```
  python main.py --help
  ```

## Results

For each run, a folder will be created in ```results``` with the experiment name. This folder contains detailed Excel files for each CL method. These files display the train and validation loss for each epoch and the corresponding test accuracy for each task, providing a comprehensive view of each CL method's performance. Additionally, at the end of each run, an Excel file is generated with a summary of each CL method. This summary includes the average accuracy of each task and the individual accuracy of each task, facilitating easy comparison between methods.

The ```results``` folder showcases multiple experiments conducted with different datasets available in this repository: MNIST with Fashion MNIST, CIFAR-10, CIFAR-100, and CIFAR-100 with data leakage. In these experiments, the number of tasks was set to 2, and the memory buffer size from BiMeCo varied across different experiments. Specifically, the memory buffer size ranged from 50%, 30%, to 10% of the data from task 1, allowing for thorough exploration of the impact of memory buffer size on model performance.

## References
[1] Sanghwan Kim, Lorenzo Noci, Antonio Orvieto, Thomas Hofmann. [Achieving a Better Stability-Plasticity Trade-off via Auxiliary Networks in Continual Learning](https://arxiv.org/abs/2303.09483). In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pp. 11930-11939. 2023.

[2] Xing Nie, Shixiong Xu, Xiyan Liu, Gaofeng Meng, Chunlei Huo, Shiming Xiang. [Bilateral Memory Consolidation for Continual Learning](https://openaccess.thecvf.com/content/CVPR2023/html/Nie_Bilateral_Memory_Consolidation_for_Continual_Learning_CVPR_2023_paper.html). Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), 2023, pp. 16026-16035.








