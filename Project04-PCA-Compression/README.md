# Project 04: PCA Compression Explorer (Applied Linear Algebra & SVD)

**Course:** Basics of Algorithmization / Algorithms (ČVUT / CTU)  
**Authors:** Arda Atik, Sviatoslav Bezhenar, Efe Hacıoğlu  
**Key Focus:** Principal Component Analysis (PCA), Singular Value Decomposition (SVD), Dimensionality Reduction, Lossy Compression  

---

## 1. Problem Statement
Given an input data matrix $X$ with $N$ samples and $D$ dimensions ($D = 3$), compress the dataset into a lower-dimensional representation $K < D$ ($K = 2$) while preserving maximum geometric variance and discarding redundant noise.

### Geometric Preprocessing: Zero-Mean Centering
To prevent the first principal component from merely pointing to the mean of the data cloud, the data must be centered at the origin[cite: 3, 7]:
$$X_{\text{centered}} = X - \frac{1}{n} \sum_{i=1}^n x_i$$
Centering performs a geometric translation that aligns the center of mass with the origin without distorting relative pairwise distances or rotations[cite: 3, 7].

---

## 2. Mathematical Decomposition via SVD

Rather than forming the full covariance matrix $X^T X$, we apply **Singular Value Decomposition (SVD)** directly on the centered data matrix for numerical stability and memory efficiency[cite: 3, 7]:

$$X_{\text{centered}} = U \Sigma V^T$$

- **$V$ columns (Right singular vectors):** Represent the principal directions (Eigenvectors)[cite: 3, 7].
- **$\Sigma$ singular values:** Directly proportional to the variance captured along each axis[cite: 3, 7].
- **Projection Matrix $W_2$:** Formed by the top 2 principal components to project $3D \rightarrow 2D$ coordinates ($Z = X_{\text{centered}} W_2$)[cite: 3, 7].

---

## 3. Variance Retained & Compression Results

Empirical breakdown on synthetic 3D correlated features[cite: 3, 7]:

- **PC1 (1st Principal Component):** Captures **99.6%** of total variance[cite: 3, 7].
- **PC2 (2nd Principal Component):** Captures **0.3%** of total variance[cite: 3, 7].
- **Cumulative Variance Retained:** **99.9%** across 2 dimensions[cite: 3, 7].
- **Storage Reduction:** Linear storage dropped by **33.3%** with negligible reconstruction error[cite: 3, 7].

---

## 4. Verification & Assert Suite
- **Unit Vector Norm:** Verified that all principal component vectors satisfy $\|v_i\| = 1.0$[cite: 3, 7].
- **Orthogonality Check:** Confirmed dot product $v_i \cdot v_j \approx 0$ for all $i \neq j$[cite: 3, 7].
- **Reconstruction Identity:** Verified that setting $K = D$ reconstructs the exact original matrix within machine epsilon[cite: 3, 7].
- **Handled Edge Cases:** Zero-variance feature columns, perfectly correlated feature sets, and single-sample edge inputs[cite: 3, 7].
