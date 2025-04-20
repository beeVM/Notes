# [谱分解 Spectral decomposition of a matrix](https://zhuanlan.zhihu.com/p/80824370):
  - $AX=X\Lambda$
  - 所谓的谱(spectrum)就是一个矩阵特征值(eigenvalue)的集合。
  - 谱分解就是将矩阵分解为其eigenvector的线性组合
# [奇异值分解 Singular Value Decomposition](https://www.cnblogs.com/pinard/p/6251584.html):
  - SVD也是对矩阵进行分解，但是和特征分解不同，SVD并不要求要分解的矩阵为方阵。
  - $A=U\Sigma V^T$
# [协方差](https://www.zhihu.com/tardis/zm/art/37609917?source_id=1005):
  - 方差是用来度量单个随机变量的离散程度，而协方差则一般用来刻画两个随机变量的相似程度
  - 方差：$$\sigma^2_x = \frac{1}{n-1} \sum_{i=1}^n (x_i-\bar{x})^2$$
  - 协方差：$$\sigma (x,y) = \frac{1}{n-1} \sum_{i=1}^n (x_i-\bar{x})(y_i-\bar{y})$$