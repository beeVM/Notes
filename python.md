## numpy基础
点积：np.dat(a,b) 。a,b为ndarray，就是计算a和b的内积，如果a和b符合矩阵乘积，则直接乘，不满足则将b取转置再乘,都不符合就报错。