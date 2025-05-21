# 磁力计校准

## 1.通过n个数据点得出椭圆一般方程

#### 一、已知椭圆一般方程为(适用于大部分曲线)

$$
Ax^2+Bxy+Cy^2+Dx+Ey+F=0--------（1）
$$

#### 	已知n个数据点Node[i]={xi,yi}, 保证其均匀分布, 且数量足够, 由以下公式可计算椭圆圆心Node={x0,y0}:

$$
Node=\frac{1}{n}\sum_{1}^{n} Node[i]--------（2）
$$

#### 	代入x'=x-x0, y'=y-y0, 将椭圆平移到以零点为圆心, 由此可消除Dx与Ey, 可得出新方程为:

$$
Ax^2+Bxy+Cy^2+F=0-------（3）
$$

#### 	根据椭圆标准方程可得F=-1

$$
可得Ax^2+Bxy+Cy^2=1                            -------（4）
$$

#### 	通过Node[i]-Node0可获得满足方程(4)的点.

#### 二、通过最小二乘法求方程参数A,B,C. 已知方程(4), 且有N个数据点. 将数据点代入可得N个方程:

$$
Ax_1^2+Bx_1y_1+Cy_1^2=1
$$

$$
Ax_2^2+Bx_2y_2+Cy_2^2=1
$$

$$
Ax_3^2+Bx_3y_3+Cy_3^2=1
$$

$$
...
$$

$$
Ax_n^2+Bx_ny_n+Cy_n^2=1
$$

$$
-------（5）
$$



#### 	将方程组写成矩阵形式:

$$
\begin{bmatrix}
x_1^2 & x_1y_1 & y_1^2 \\
x_2^2 & x_2y_2 & y_2^2 \\
x_3^2 & x_3y_3 & y_3^2 \\
... \\
x_n^2 & x_ny_n & y_n^2 \\
\end{bmatrix}
\begin{bmatrix}
A \\
B \\
C \\
\end{bmatrix}
=
\begin{bmatrix}
1 \\
1 \\
1 \\
... \\
1 \\
\end{bmatrix}--------（6）
$$

#### 	表示为以下简单形式:

$$
\mathbf{X}\beta=\mathbf{b}-------（7）
$$

#### 	因为N个数据点为实际磁力计测量, 存在误差, 该方程可能无解. 所以仅要求最小化残差平方和:

$$
SSE = \sum_{1}^{n}(Ax_i^2+Bx_iy_i+Cy_i^2-1)^2=
\begin{Vmatrix}
\mathbf{X}\beta-\mathbf{b}
\end{Vmatrix}^2-------（8）
$$

#### 	求导公式(8)SSE:

$$
SSE'=\frac{\partial{SSE}}{\partial\beta}=2\mathbf{X}^{T}(\mathbf{X}\beta-\mathbf{b})=0-------（9）
$$

#### 	解得:

$$
\mathbf{X}^{T}\mathbf{X}\beta=\mathbf{X}^{T}\mathbf{b}
$$

$$
\beta=(\mathbf{X}^{T}\mathbf{X})^{-1}\mathbf{X}^{T}\mathbf{b}-------（10）
$$

#### 	求证以上方程正确性:

##### 1.将公式(8)展开后对β的3维A,B,C分别求偏导 

$$
SSE=(\mathbf{X}\beta-\mathbf{b})^T(\mathbf{X}\beta-\mathbf{b})
$$

$$
=[(\mathbf{X}\beta)^T-\mathbf{b}^T](\mathbf{X}\beta-\mathbf{b})
$$

$$
=(\mathbf{X}\beta)^T\mathbf{X}\beta-\mathbf{b}^T\mathbf{X}\beta-(\mathbf{X}\beta)^T\mathbf{b}+\mathbf{b}^T\mathbf{b}
$$

$$
=\beta^T\mathbf{X}^T\mathbf{X}\beta-\mathbf{b}^T\mathbf{X}\beta-\beta^T\mathbf{X}^T\mathbf{b}+\mathbf{b}^T\mathbf{b}
$$

##### 2.又因为标量的转置为其本身

$$
\mathbf{b}^T\mathbf{X}\beta=
\begin{bmatrix}
1&1&1&1&...&1
\end{bmatrix}
\begin{bmatrix}
x_1^2 & x_1y_1 & y_1^2 \\
x_2^2 & x_2y_2 & y_2^2 \\
x_3^2 & x_3y_3 & y_3^2 \\
... \\
x_n^2 & x_ny_n & y_n^2 \\
\end{bmatrix}
\begin{bmatrix}
A \\
B \\
C \\
\end{bmatrix}
=\begin{bmatrix}
1&1&1&1&...&1
\end{bmatrix}
\begin{bmatrix}
1\\
1\\
1\\
...\\
1\\
\end{bmatrix}
=
\begin{bmatrix}
1*1+1*1+...1*1\\
\end{bmatrix}=
\begin{bmatrix}
N\\
\end{bmatrix}
=N
$$



##### 3.所以可得最终公式:

$$
\beta^T\mathbf{X}^T\mathbf{X}\beta-2\mathbf{b}^T\mathbf{X}\beta+\mathbf{b}^T\mathbf{b}-------（11）
$$

##### 4.以此求导

$$
\frac{\partial{\beta^T\mathbf{X}^T\mathbf{X}\beta}}{\partial{\beta}}=2\mathbf{X}^T\mathbf{X}\beta
$$

$$
\frac{\partial{2\mathbf{b}^T\mathbf{X}\beta}}{\partial{\beta}}=-2\mathbf{X}^T\mathbf{b}
$$

$$
\frac{\partial{\mathbf{b}^T}\mathbf{b}}{\partial{\beta}}=0
$$

##### 5.矩阵求导

$$
矩阵对向量求导
$$

$$
即对向量每一维进行偏导
$$

$$
以第一个方程举例:\beta^T\mathbf{X}^T\mathbf{X}\beta=\sum_{i=1}^{n}(\sum_{j=1}^{p}\sum_{k=1}^{p}\beta_j(\mathbf{X}^T\mathbf{X})_{jk}\beta_{k})
$$

$$
对\beta_{m}求偏导(只有j=m或k=m时不为0): \frac{\partial{(\sum_{j,k}\beta_j(\mathbf{X}^T\mathbf{X})_{jk}\beta_k)}}{\partial{\beta_m}}
=
2\sum_{k=1}^{p}(\mathbf{X}^T\mathbf{X})_{mk}\beta_k
$$

##### 6.求最小值

$$
2\mathbf{X}^T\mathbf{X}\beta-2\mathbf{X}^T\mathbf{b}=0
$$

$$
\boldsymbol{\beta}=(\mathbf{X}^T\mathbf{X})^{-1}\mathbf{X}^T\mathbf{b}
$$

$$
又因\beta = 
\begin{bmatrix}
A\\
B\\
C\\
\end{bmatrix}
由此可得椭圆方程所有参数
$$

