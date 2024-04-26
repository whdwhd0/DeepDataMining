## Github上传作业

Github个人仓库地址：https://github.com/whdwhd0/DeepDataMining

### 流程

1. 注册个人github账号

   ![img](https://github.com/whdwhd0/DeepDataMining/blob/image/Register.png)

2. 登录个人github账号。

   ![img](https://github.com/whdwhd0/DeepDataMining/blob/image/user.png)

3. 建立一个仓库命名为DeepDataMining。

   ![img](https://github.com/whdwhd0/DeepDataMining/blob/image/load.png)
4. 上传自己的PPT到DeepDataMining中。

   ![img](https://github.com/whdwhd0/DeepDataMining/blob/image/last.png)






## 想学习的计算机技能-运用自监督方法进行缺陷检测

### **基础理论与概念**

![img](https://amitness.com/posts/images/self-supervised-workflow.png)
<p align="center">
   图 自监督端到端学习工作流程
</p>
1. **自监督学习原理**：
   - **定义与动机**：理解自监督学习的核心思想，即如何通过构建自我监督任务从无标签数据中学习有用的表征。
   - **预训练与微调**：熟悉自监督学习的两个主要阶段：使用自定义任务进行预训练，以及将预训练模型应用于下游任务（如缺陷检测）时的微调过程。
2. **自监督学习方法**：
   - **对比学习**：深入理解对比学习（Contrastive Learning）方法，如SimCLR、MoCo、SwAV等，掌握数据增强、编码、损失函数（如InfoNCE loss）的设计与作用。
   - **掩码建模**：学习基于掩码的自监督方法，如MAE、BEiT，理解如何通过掩码部分输入并预测缺失部分来学习表征。
   - **生成式自监督**：了解基于生成任务的自监督方法，如旋转预测、色彩恢复、上下文推理等，以及如何通过重构或预测某些图像属性来学习特征。



### **计算机视觉中的缺陷检测**

1. **传统缺陷检测方法**：
   - **基础知识**：理解缺陷检测的基本任务要求，包括异常检测、分类、定位与分割。
   - **经典算法**：了解传统的图像处理与机器学习方法，如阈值分割、模板匹配、纹理分析、SVM、Adaboost等，以及它们在缺陷检测中的应用。
2. **深度学习在缺陷检测中的应用**：
   - **目标检测**：掌握常见深度学习目标检测模型（如Faster R-CNN、YOLO、SSD）的工作原理和在缺陷检测中的适应性。
   - **语义/实例分割**：理解深度学习分割模型（如U-Net、FCN、Mask R-CNN）如何用于精确地识别缺陷区域。



### **自监督学习与缺陷检测的结合**

1. **自监督预训练**：

   - **选择与设计预训练任务**：根据缺陷检测的具体需求，选择或设计合适的自监督学习任务，如对比学习中的图像对构建策略、掩码建模中的掩码策略等。
   - **预训练数据准备**：了解如何准备和处理无标签的缺陷样本或类似场景的数据，以用于自监督学习。

2. **迁移学习与微调**：

   - **预训练模型选择**：根据应用场景和硬件资源，选择适合的预训练模型架构（如ResNet、ViT等）及对应的自监督版本。
   - **微调策略**：学习如何针对缺陷检测任务调整模型架构（如有必要）、优化器设置、学习率策略、正则化方法等，并进行有监督微调。

3. **自监督特征评估与应用**：

   - **特征可视化与分析**：学习如何使用TSNE、UMAP等降维可视化方法，以及CAM、Grad-CAM等工具来理解自监督模型学到的特征表示。

   - **性能评估**：掌握适用于缺陷检测任务的评价指标，如精度、召回率、IoU、F1分数等，以及如何使用交叉验证、AUC-ROC曲线等方法来全面评估模型性能。

     

### **实验与实践**

#### **编程技能**

1. **Pytorch框架**：

   **1. 1模型构建**

   - **定义网络结构**：利用`nn.Module`类作为基类创建自定义网络，通过`nn.Conv2d`、`nn.MaxPool2d`、`nn.ReLU`等模块构建卷积层、池化层、激活函数等基础组件。对于自监督任务，可能还需要实现特定的自编码器（AutoEncoder）、多头自注意力（Multi-Head Attention）或Transformer等结构。
   - **继承预训练模型**：使用`torchvision.models`库加载预训练的CNN模型（如ResNet、EfficientNet等），并可通过子类化这些模型来添加或修改特定层以适应自监督任务。例如，在预训练模型顶部添加自定义头部（head）用于特定任务的特征提取。

   **1.2 训练流程**

   - **前向传播**：在模型类中定义`forward(self, x)`方法，实现输入数据`x`经过网络各层的计算过程。这通常涉及逐层调用各模块的`forward()`方法，并可能包含自定义的中间计算步骤。
   - **反向传播与梯度计算**：使用`loss.backward()`自动执行反向传播，计算模型参数相对于损失函数的梯度。确保在调用此方法前，模型处于训练模式（`model.train()`）且设置了要求梯度（`x.requires_grad_()`）。
   - **权重更新**：使用优化器（如`torch.optim.Adam`、`torch.optim.SGD`）更新模型参数。在每个训练步骤后，调用`optimizer.step()`应用梯度更新，然后使用`optimizer.zero_grad()`清空梯度以准备下一轮迭代。

   **1.3 模型保存与加载**

   - **保存模型**：使用`torch.save()`将模型的权重（`.state_dict()`)或整个模型（包括架构与权重）保存到文件。

   - **加载模型**：使用`torch.load()`加载已保存的模型权重或整个模型。

     

2. **TensorFlow框架**：

   **2.1模型构建**

   - **定义网络结构**：使用`tf.keras.layers`模块构建卷积层（`Conv2D`）、池化层（`MaxPooling2D`）、激活函数（如`ReLU`）等。同样，对于自监督任务，可能需要实现自编码器、Transformer等复杂结构。利用`tf.keras.Sequential`或`tf.keras.Model`构建模型层级结构。
   - **继承预训练模型**：通过`tf.keras.applications`导入预训练模型（如`ResNet50`、`EfficientNetB0`等），并可使用`Model`类对其进行扩展，添加或替换特定层以适应自监督任务。

   **2. 2训练流程**

   - **前向传播**：在`call(self, inputs, training=None)`方法中定义模型的前向传播逻辑。`training`参数指示当前是否处于训练模式，可根据此参数启用/禁用批次归一化层的更新、dropout等。
   - **反向传播与梯度计算**：使用`Model.compile()`方法指定损失函数、优化器和评估指标，然后调用`Model.fit()`进行训练。反向传播和梯度计算由框架自动完成。
   - **权重更新**：在`compile()`时指定优化器（如`Adam`、`SGD`），框架会在`fit()`过程中自动应用权重更新。

   **2. 2模型保存与加载**

   - **保存模型：**使用Model.save_weights()保存模型权重，或使用Model.save()保存整个模型（包括架构与权重）。

   - **加载模型**：使用`Model.load_weights()`加载已保存的模型权重，或直接使用`tf.keras.models.load_model()`加载整个模型。

     

3. **数据处理**：

   - **数据加载**：使用`torch.utils.data.Dataset`和`DataLoader`接口封装数据集，实现批量数据读取与预处理。
   - **图像操作**：运用PIL、OpenCV等库进行图像裁剪、缩放、翻转、颜色空间转换等预处理操作。
   - **数据增强**：根据自监督任务特性，设计并实现合适的数据增强策略，如随机裁剪、色彩抖动、旋转、平移等，以增加模型的泛化能力。



#### **项目实施**

1. **数据集准备**

   - **数据收集**：确定数据来源，可能包括公开数据集（如MVTec AD、Kaggle竞赛数据集等）、企业内部数据或自行采集。确保数据涵盖各类正常样本以及各种类型的缺陷。
   - **数据清洗**：去除无效、损坏或无关的图像，确保数据质量。对于有监督微调阶段，可能还需要进行标注工作，包括缺陷类别标注、边界框标注或像素级分割标注。
   - **数据划分**：合理划分数据集为训练集、验证集和测试集。对于自监督预训练，通常仅需大量无标签数据；若有监督微调，则需确保各集合均有对应的标签信息。

2. **实验流程**

   **自监督预训练阶段**：

   - **任务选择与实现**：根据前期理论学习，选择适合缺陷检测的自监督任务（如对比学习、掩码建模），并在深度学习框架中实现相应的损失函数、数据增强策略、模型架构等。
   - 模型训练：
     - 设置合理的超参数，如学习率、批量大小、训练轮数等。
     - 实施预训练，监控训练损失、验证指标（如自定义的自监督任务指标）以及模型收敛情况。
     - 定期保存中间模型权重，以便后续分析与比较。
     - 可视化学习到的特征表示，如通过PCA、t-SNE等降维方法观察特征分布变化，以初步评估预训练效果。

   **有监督微调阶段**：

   - **模型调整**：根据缺陷检测任务特点，可能需要对预训练模型进行微调，如添加额外的输出层、调整最后几层的参数学习率、引入注意力机制等。
   - **损失函数设定**：定义适用于缺陷检测任务的损失函数，如交叉熵损失（分类）、L1/L2损失（回归）、Dice/Focal loss（不平衡分类）或IoU损失（分割）。
   - 训练与评估：
     - 使用预训练模型初始化权重，进行有监督微调。
     - 监控训练损失、验证集上缺陷检测指标（如精度、召回率、IoU、F1分数等）。
     - 利用学习率调度策略调整学习率，优化模型收敛速度与性能。
     - 定期保存最优模型权重。

3. **性能评估与模型比较**

   - **评估指标计算**：根据任务类型（分类、定位、分割），计算相应的评估指标，如准确率、精确率、召回率、F1分数、IoU（Jaccard相似系数）等。
   - **混淆矩阵分析**：绘制并解读混淆矩阵，分析模型在各类别上的表现，包括假正例、假负例等，找出模型的强项与短板。
   - **AUC-ROC曲线与PR曲线**：对于不平衡数据集，绘制AUC-ROC曲线和PR曲线，评估模型在不同阈值下的性能。
   - **模型比较**：对比不同自监督预训练策略（如不同的自监督任务、数据增强方案等）对最终缺陷检测性能的影响，选择最优模型。

#### **持续学习与跟进**

1. **学术文献追踪**：
   - 订阅相关领域的学术期刊、会议通知，如IEEE Transactions on Pattern Analysis and Machine Intelligence (TPAMI)、International Journal of Computer Vision (IJCV)、CVPR、ICCV、ECCV等。
   - 使用Google Scholar、arXiv Sanity Preserver等工具，设置关键词警报，及时获取最新研究成果。
   - 阅读并总结重要论文，理解其创新点、实验方法与结论，思考如何将新方法应用于自己的项目。
2. **开源项目与代码复现**：
   - 关注GitHub上的热门计算机视觉与自监督学习项目，如`facebookresearch/dino`、`microsoft/MoCo-v3`、`google-research/simclr`等。
   - 参与或发起代码复现项目，通过实际编程实现论文中的算法，加深对理论的理解，同时锻炼编程与问题解决能力。
   - 参与开源社区讨论，提问、解答他人问题，分享自己的实践经验，促进知识交流与个人成长。
