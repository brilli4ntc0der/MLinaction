# This is a kNN practice project based on sklearn

1. 模型
特征空间中，对每个训练实例点$ x_ i$,距离该点比其他点更近点所有点组成一个单元（cell）。所有训练实例点的单元构成对特征空间的一个划分。
2. 距离度量
特征空间中两个实例点点距离是两个实例点相似程度的反映。特征空间一般是$n$维实数向量空间$\mathbf{R}^n$。
设特征空间$\mathbf{\chi}$是$n$维实数向量空间$\mathbf{R}^n$, $x_i$, $x_j \in \chi$, $x_i=(x^{(1)}_i,x^{(2)}_i,...,x^{(n)}_i)^T$,$x_j = (x^{(1)}_j,x^{(2)}_j,...,x^{(n)}_j)^T$, $x_i, x_j$的$L_p$距离定义为
$$ L_p（x_i,x_j）= (\sum^{n}_{l=1}|x^{(l)}_i - x^{(l)}_j|^p)^{( \frac{1}{p})}        \tag{2.1}$$
 p=2时，$L_2$称为欧式距离。
 p=1时，$L_1$称为曼哈顿距离。
 p=$\infty$时，$L_\infty$是各坐标的最大值。 
 3.分类决策规则
 多数表决表决规则（marjority voting rule）:如果分类的损失函数为0-1损失函数，分类函数为：
 $$f:\textbf{R}^n \rightarrow \{c_1, c_2, ...,c_K\} $$
 误分类概率是：
 $$P((Y \neq f(X))=1-P(Y=f(X))$$
 对于给定的实例$x \in \chi$,其最近邻的$k$个训练实例点构成集合$N_k(x)$。如果涵盖$N_k(x)$的区域的类别是$c_j$，误分类率为：
 $$\frac{1}{k}\sum_{x_i \in N_k(x)}{I(y_i \neq c_j)}=1- \frac{1}{k}\sum_{x_i \in N_k(x)}{I(y_i = c_j)}$$
 要使得误分类率最小即经验风险越小，即使得$\sum_{x_i \in N_k(x)}{I(y_i = c_j)}$最大。 多数表决表决规则等价于经验风险越小化。
 3.k近邻法的实现：kd树
    + kd树中的k指k维空间（即$n$），kd树是二叉树，表示对$k$维空间的一个划分。。构造kd树相当于不断用垂直与坐标轴的超平面将$k$维空间切分，构成一系列$k$维超矩形区域。kd树的每个节点对应于一个$k$维超矩形区域。
    + kd树的构造：
    输入:$k$维空间数据集$T=\{x_1,x_2,..,x_N\}$，其中$x_i=(x^{(1)}_i,x^{(2)}_i,...,x^{(k)}_i)^T$，$i=1,2,...,N$
    输出：kd树
    对于深度为$j$的节点，选择$x^{(l)}$为切分的坐标轴,$l=j(mod\ k)+1$，以该结点的区域中所有实例的$x^{(l)}$坐标的中位数为切分点，该节点生成深度为$j+1$的左右子结点：左子结点对应坐标$x^{(l)}$小于切分点的子区域，右子结点对应坐标$x^{(l)}$大于切分点的子区域。
    + 搜索kd树:
    （1）在kd树中找出包含目标点$x$的叶结点
    （2）递归回退，在每个子结点上进行：(1)如果当前结点保存的实例点比当前最近点距离目标点更近，则以该实例点为“当前最近点”(b)检查“当前最近点”对应子结点的父结点的另一子结点对应的区域是否有更近的点（即另一子结点包含的$k$维超矩形是否与目标点为球心，目标点与“当前最近点”的距离为半径的超球体相交）。
        若有则递归搜索，否则向上回退。

