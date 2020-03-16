# RSNA颅内出血检测项目
 
 
## 1.项目简介：

  这是本人第一次参加Kaggle比赛所做的项目，抱着学习，练手的目的来参与，该比赛的链接是:  https://www.kaggle.com/c/rsna-intracranial-hemorrhage-detection。

 该比赛的目的是建立深度学习模型，使其输入CT图像，输出该定CT图像属于各种颅内出血的概率。具体来说，该比赛需要根据CT图像来判断如下五种颅内出血的概率：
  * 内出血(Intraventricular hemorrhage, IVH)
  * 脑实质性出血(Intraparenchymal hemorrhage, IPH)
  * 蛛网膜下腔出血(Subarachnoid hemorrhage, SAH)
  * 硬膜外出血(Epidural hematoma, EDH)
  * 和硬膜下血肿(Subdural hematoma, SDH)
  
  数据集的大小是167.5GB，其中训练集有图像674262张，测试集有图像78545张。
  
 

## 2.图像数据预处理
 参考Kaggle上讨论区高票的医学图像数据预处理方案:
 
 https://www.kaggle.com/reppic/gradient-sigmoid-windowing
 
 我们首先将图片转化为HU值图片(HU值定义：https://en.wikipedia.org/wiki/Hounsfield_scale )， 接着我们可以利用Sigmoid变换了分离出骨骼，脑膜，大脑三部分的具体，组成处理后图片的三个频道，这是因为一般来说HU值是骨骼>脑膜>大脑组织。
 
## 3.数据增广
 由于该训练集是高度不平衡的，各个类型的确诊病例都非常稀少，我们考虑对于稀疏的类型进行数据增广(Data Augmentation)。我们利用imgaug，对图片进行增广处理，具体的，对于每张训练集中的图片我们均随机从选取 "flipping", "rotation", "scaling", "cropping", "translate"
五种变换中随机选取若干种，随机生成参数来生成新的图片。

## 4.ResNet50模型
 该项目中我使用了ResNet50模型。为了深入学习这个模型，本人仔细阅读了Keras实现的源代码，并且在项目中对该模型进行了适度修改，将激活函数改为ELU函数，加入L2正则项。
