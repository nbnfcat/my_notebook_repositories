# 机器学习
based 
- Andrew Ng 教授在斯坦福大学 2022 年推出的《机器学习专项课程》（Machine Learning Specialization）
- 周志华. 机器学习[M]. 北京：清华大学出版社, 2016.

## 基本定义

### 机器学习（Machine Learning）
定义：
- Field of study that gives computers the ability to learn without being explicitly programmed（译：使计算机能够在不进行显式编程的情况下获得学习能力的研究领域）。
-  致力于研究如何通过计算的手段，利用经验来改善系统自身的性能。关于在计算机上从数据中产生“模型（Model）”的算法，即“学习算法（Learning algorithm）”，通过学习算法将经验数据提供给它，它就能基于这些数据产生模型，在面对新的情况时，模型会给我们提供相应的判断。（[Mitchell,1997]：假设用P来评估计算机程序在某任务类T上的性能，若一个程序通过利用经验E在T中任务上获得了性能改善，则我们就说关于T和P，该程序对E进行了学习）。
-  更多的学习机会（/次数）将带来更优性能。

类型：
- 监督学习 Supervised learning
- 无监督学习 Unsupervised learning
- 推荐系统 Recommender systems
- 强化学习 Reinforcement learning

### 监督学习
定义：表示x到y的映射（正确的y）,对映射完成学习后，在输入未见过的x时系统将输出相应的y。例：机器翻译 “English # Input” to "Chinese # Output"

分类：
- 回归 Regression：从"无限可能数 # 可能结果域，域空间无穷 "中，预测一个"数字 # Output"。预测结果是按照离散的数据进行拟合得到的输入与输出的"映射关系 # 直线/曲线/复杂函数拟合"。
- 分类 Classification：从少量可能中，预测一个类型(categories/class)。通过数据拟合划定一个分类的边界，从而预测当前输入（单个/多个输入）的类型。

### 无监督学习
定义：在无明确输出映射关联的"数据 # Input"中找到某些"关系 # Output"。

分类：
- 聚类 Clustering:自主地将数据进行分组。
- 异常检测 Anomaly detection:找出异常的数据。
- 降维 Dimensionality reduction:将数据压缩，用更小的信息对其进行描述。