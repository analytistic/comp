# 问题一

在每个评审阶段,作品通常都是随机分发的,每份作品需要多位评委独立评审。为了增加不同评审专
家所给成绩之间的可比性,不同专家评审的作品集合之间应有一些交集。但有的交集大了,则必然有交集小
了,则可比性变弱。请针对3000支参赛队和125位评审专家,每份作品由5位专家评审的情况,建立数学模型
确定最优的“交叉分发”方案,并讨论该方案的有关指标(自己定义)和实施细节。

## 2. 数学模型
### 2.1. 定义变量和参数
- $𝑁=3000$：参赛作品数。
- $𝑀=125$：评审专家数。
- $𝐾=5$：每个作品的评审专家数。
- $𝑆_{𝑖𝑗}$：表示第$i$专家是否评审 $j$个作品，若评审则为 1，否则为 0。
### 2.2. 量化指标
- $I_{ii'}$：定义两评审专家对，$i$和$i'$的交集模为：
$$
\begin{equation}
    I_{ii'} = \sum_{j=1}^{N} S_{ij} \times S_{i'j}
\end{equation}
$$
- $I_{all}$：所有专家对的交集模之和为:
  $$
  \begin{equation}
    I_{all} = \sum_{i}^{M}\sum_{j=i+1}^{M} I_{i}{j}
  \end{equation}
  $$
- $\bar I$：定义每两个专家对之间交集模的平均值：
  $$
  \begin{equation}
    \bar I = \frac{I_{all}}{(_{M}^{2})}
  \end{equation}
  $$

首先证明，所有专家对的交集模之和$I_{all}$为固定值$(_{2}^{M})$
- proof: 
  $$

  I_{all} = \sum_{i=1}^{M}\sum_{i'=i+1}^{M}\sum_{j=1}^{N}S_{ij} \times S_{i'j}

  $$
  交换求和顺序，得到：
  $$

    I_{all} = \sum_{j=1}^{N}\sum_{i=1}^{M}\sum_{i'=i+1}^{M} S_{ij} \times S_{i'j}   

  $$
  由于每个作品固定分给五位专家, $K=5$，所以：
  $$
  \max\sum_{i=1}^{M}\sum_{i'=i+1}^{M} S_{ij} \times S_{i'j} =(_{2}^{K})
  $$
  $$
    I_{all} = \sum_{j=1}^{N}(_{2}^{K})=\frac{K(K-1)}{2}\times N
  $$
  所以，$\bar I = \frac{5\times 4\times 3000}{125 \times 124} \approx 3.87$

### 2.3. 规划模型
#### 2.3.1 目标函数
- 可比性：
  
  首先我我们希望对于每件作品，分配的五位专家之间的交集模较高，这会提高一件作品的评分的可信度:
  $$
  \begin{equation}
    \text{maximize } 
    Fitness_{1} = \sum_{p=1}^{N}\sum_{i\in \{S_{ip} \neq 0\}} \sum_{j\in \{S_{ip} \neq 0\}, 
    j\neq i} I_{ij}
  \end{equation}
  $$ 
- 交集适中:
  
  其次我们希望交集不会过大或过小。所以我们将每个专家对之间的交集模优化到平均值附近。

  $$
  \begin{equation}
    \text{minimize } Fitness_{2}=\sum_{i=1}^{M} \sum_{i'=i+1}^{M}(I_{ii'}-\bar I)^2 
  \end{equation}
  $$
  
  由（）可知，$\bar I$ 为3.87

### 2.3.2约束
- 每份作品被5位专家评审：
    $$
    \begin{equation}
    \sum_{i=1}^{M} S_{ij} = K \qquad \forall j \in \{1, 2, \ldots, N\}
    \end{equation}
    $$
- 每位专家的作品数量适中：
  易知每个专家平均审核$\frac{K \times N}{M}=120$个作品，则有：
  $$

    \sum_{j=1}^{N} S_{ij} = 120 \qquad\forall i \in \{ 1,2,\ldots,M\}
 
  $$



  

















