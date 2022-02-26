# Linear Algebra

# Chapter 1

### Vectors

- How to represent vector
    - With n numbers
    - Arrow from (0 ... 0)
    - Point in the space
        
        
- $\begin{bmatrix}a \\b \\c \\\end{bmatrix} = (a , b, c)$

### Lenths and Dot Products

- Dot product $v\cdot w$ multiplies each component $v_i$ by $w_i$ and addi all $v_i$$w_i$
- $v\cdot w = v'*w$
- $u=\frac{v}{||v||}$ → Unit Vector : length 1
- The dot product is $v\cdot w = 0$ when vectors $v$ and $w$ are perpendicular.
- $cos\theta = \frac{v\cdot w}{||v||\ ||w||}$
- Schwarz Inequation : $|v\cdot w| \le ||v|| \ ||w||$
- Triangle Inequation : $||v + w|| \le ||v|| + ||w||$

### Matrix

- Matrix Times Vector
    - Combination of the columns of A
    - Dot product with rows

# Chapter 2

### Solving Linear Equation

- How to see a matrix(linear equation)
    - Row picture
        - The interception of n spaces depended on each row equation.
    - Collumn picture
        - The coefficient of combination that can satisfy the result
    
- Elimination
    - Produce an upper triangular system
- Back substitution
    - Calculate the result from bottom to top
- Pivot
    - First nonzero in the row that does the elimination
- Multiplier
    - (entry to eliminate) divided by (pivot)
- The reason why elimination failed
    - Row picture
        - The two plane depended by different row are parallel
    - Column picture
        - There are two arror on same lilne
    - Two possible situation
        - For n equations we do not get n pivots
            - $0 \ne 0$ (No solution)
            - $0 = 0$ (Many solution)
    

### Elimination Using Matrix

- Associative law is true $A(BC) = (AB)C$
- Commutative law is false $AB \ne BA$
- Matrix Multiplication
    - Take the dot product of each row of A with each column of B
        
        $$⁍$$
        
    - Each column of AB is a combination of the columns of A
        
        $$⁍$$
        
    - Every Row of A times Matrix B
        
        $$⁍$$
        
    - **Multiply columns 1 to n of A times rows 1 to n of B. Add those Matrix**
        
        $$\begin{bmatrix} col\ 1 & col \ 2 &  col \ 3\\ . & . & .\\  .& . & .\end{bmatrix}\begin{bmatrix} row\ 1 & . & .\\  row\ 2 & . &. \\   row\ 3& . &.\end{bmatrix} \\= (col\ 1)(row \ 1)+(col\ 2)(row\ 2) + (col\ 3) (row\ 3)$$
        
- Inner Product (Outer Product)
    - A row times a column
- Block Multiplication
    
    $$\begin{bmatrix}  A_{11}& A_{12} \\  A_{21} & A_{22}\end{bmatrix}\begin{bmatrix}B_{11} \\B_{21}\end{bmatrix} = \begin{bmatrix} A_{11}B_{11} + A_{12}B_{21}\\A_{21}B_{11}+A_{22}B_{21}
    \end{bmatrix}$$
    
- Schur Complement (Elimination By Blocks)
    
    $$Schur \ Complement = D-CA^{-1}B$$
    
    [矩阵的舒尔补(Schur complement)](https://blog.csdn.net/sheagu/article/details/115771184)
    
    [](https://blog.csdn.net/qq_25458977/article/details/102721773)
    

### Inverse

- If $Ax = 0$ for a nonzero vector $x$ , them $A$ has no inverse
- Calculate Inverse
    - Gauss-Jordan Elimination
        
        [高斯-约当（Gauss-Jordan）消元法_expleeve-CSDN博客](https://blog.csdn.net/expleeve/article/details/46647177)
        
        $$[A \ I] -> [I \ A^{-1}]$$
        
- Reduced Echolon Form
    - First Elimination
    - Rows are added to rows above them, to produce zeros above the pivots.
- Band Matrix = nDiagonal Matrix = 有n个对角线的矩阵
- Fast Determine A Matrix Inversible
    - Diagonally dominant matrices are invertible
        - $|a_{ii}|>\sum_{j\ne i}|a_{ij}|$

### LU factorization

- Gaussian Elimination factors A into L times U
- The lower triangular L contains the numbers $l_{ij}$ that multiply pivot rows, going from A to U. The product LU adds those rows back to recover A
- On the right side we solve $Lc= b$ (forward), and $Ux=c$ (backword)
- Factor: There are $\frac{1}{3}(n^3 - n)$ multiplications and subtractions on the left side.
- Solve: There are $n^2$ multiplications and subtractions on the right side.
- For a band matrix, change $\frac{1}{3}n^3$ to $nw^2$ and change $n^2$  to $2wn$

### Transpose and Permutations

- Understand inner product in Matrix aspect
    - The dot product or inner product is $x^T y$
    - The rank one product or outer product is $xy^T$
- The meaning of transpose
    - $A^T$ is the matrix that makes these two inner products equal for every x and y
    - $(Ax)^Ty=x^T (A^T y)$
- Symmetric matrix
    - The inverse of a symmetric matrix is also symmetric
    - We can produce symmetric matrix by products $A^T A$ , $AA^T$, $LDL^T$
        - When S is symmetric, the form $A=LDU$ becomes $S=LDL^T$, which makes elimination faster, we can work with half the matrix. From $\frac{n^3}{3}$  ot $\frac{n^3}{6}$
- Permutation Matrix
    - $PA=LU$ → This means permutate before elimination
    - $A=L_1P_1U_1$ → This means permutate after elimination
    - The determinant of P only depend on the exchange count of P
- Numbertic Computation Problem
    - Theoritically we can use any number except 0 as the pivot, while a good codes look down the column for the largest pivot to avoid error.
- key ideas
    - The transpose puts the rows of $A$ into the columns of $A^T$. Then $(A^T)_{ij} = A_{ji}$
    - The transpose of $AB$ is $B^T A^T$. The transpose of $A^{-1}$  is the inverse of $A^T$
    - The dot product is $x\cdot y=x^Ty$. Then $(Ax)^Ty=x^T (A^T y)$
    - When $S$ is symmetric （$S^T = S$）， its $LDU$ factorization is symmetric: $S=LDL^T$
    - A permutation Matrix $P$ has a 1 in each row and column, and $P^T=P^{-1}$
    - There are $n!$ permutation matrices of size $n$, Half even, half odd.
    - If $A$ is invertible then a permutation $P$ will recorder its rows for $PA=LU$

## Chapter 3

### Spaces of Vectors

- $R^n$  contains all colulmn vectors with $n$ real components
- $M$(2 by 2 matrices) and $F$ (functions) and $Z$ (zero vector alone) are vector spaces
- A subspace containing $v$ and $w$ must contain all their combinations $cv+dw$
- The combinations of the columns of $A$ form the column space $C(A)$. Then the column space is “spanned” by the columns
- $Ax = b$ has a solution when $b$ is in the column space of $A$
- $C(A)$ = all combinations of the columns = all vectors $Ax$

### The Nullspace

- The nullspace $N(A)$ is a subspace of $R^n$. It contains all solutions to $Ax = 0$
- Elimination on $A$ produces a row reduced $R$ with pivot columns and free columns
- Every free column leads to a special solution. That free variable is 1, the other are 0.
- The $rank\ r$ of $A$ is the number of pivots. All pivots are 1's in $R=rref(A)$
- The complete solution to $Ax = 0$ is a combination of the $n-r$ special solutions.
- $A$ always has a free column if $n>m$, giving a nonzero soluution to $Ax=0$
- All matrix have $r$ independent rows and columns
- The rank $r$ is the dimension of the column space. It is also the dimension of the row space.

### The Complete Solution to $Ax=b$

- The rank $r$ is the number of pivots. The matrix $R$ has $m-r$ zero rows.
- $Ax = b$ is solvable if and only if the last $m-r$ equations reduce to $0=0$
- One particular solution $x_p$ has all free variables qeual to zero.
- The pivot variables are determined after the free variables are chosen.
- Full column rank $r=n$ means no free variables: one solution or none
- Full row rank $r= m$ means one solution if $m=n$ or infinitely many if $m<n$

### Independence, Basis and Dimension

- The columns of $A$ are independent if $x= 0$ is the only solution to $Ax=0$
- The vectors $v_1,...,v_r$ span a space if their combinations fill that space
- A basis consists of linearly independent vectors that span the space. Every vector in the space is a unique combination of the basis vectors
- All bases for a space have the same number of vectors. This number of vectors in a basis is the dimension of the space
- The pivot columns are one basis for the column space. The dimension is $r$
- **Multiply a matrix on the left side only change the basis in column space and do not change the row space.**

### Dimensions of the Four Subspaces

- The $r$ pivot rows of $R$ are a basis for the row spaces of $R$ and $A$
- The $r$ pivot columns of $A$ are a basis for its columns space $C(A)$ instead of $C(R)$
- The $n-r$ special solutions are a basis for the nullspaces of $A$ and $R$ (smae space)
- (If $EA=R$, the last $m-r$ rows of $E$ are a basis for the left nullspace of $A$)? Not sure
- Every rank $r$ matrix is a sum of $r$ rank  one matrices: Pivot columns of $A$ times nonzero rows or$R$.

## Computation Speed

- Matrix Multiplication
    - General : $n^3$
    - Breaking n by n martices to 2 by 2 blocks : $n^{2.376}$
- Inverse
    - $n^3$
- Solve a single $Ax = b$
    - $\frac{n^3}{3}$ for A
    - $n^2$ for b
- Solve $Ax = b$ by $Lc=b, Ux=c$
    - if the matrix is sparse(eg. A band matrix with w diagnol)
    - $nw^2$ for A → U
    - $2nw$ for b

## Special Matrix

- Difference Matrix
    
    $$\begin{bmatrix}  1& 0 & 0\\  -1& 1 & 0\\ 0 & -1 &1\end{bmatrix}$$
    
    The Matrix have inverse
    
- Sum Matrix
    
    $$\begin{bmatrix}  1& 0 & 0\\  1& 1 & 0\\ 0 & 1 &1\end{bmatrix}$$
    
    The Matrix has inverse
    
- Centered Difference Matrix
    
    $$\begin{bmatrix}  1& 0 & -1\\  -1& 1 & 0\\ 0 & -1 &1\end{bmatrix}$$
    
    This Matrix has no inverse
    
- Elimination Matrix
    
    $$E_{31} = \begin{bmatrix}  1& 0 &0 \\ 0 & 1 & 0\\ -l & 0 &1\end{bmatrix}$$
    
    $E_{ij}$ subtracts a multiple $l$ of row j from row $i$
    
- Permutation Matrix
    
    $$P_{23} = \begin{bmatrix} 1 & 0 & 0\\ 0 & 0 & 1 \\ 0 & 1 & 0\end{bmatrix}$$
    
    $P_{ij}$ exchanges the rows $i$ and rows $j$
    
- Augmented Matrix·
    
    $$Aug = [A \ b]$$
    
- Incidence Matrix
    
    $$\left[\begin{array}{ccccc}1 & 0 & 0 & -1 & 1 \\-1 & -1 & 0 & 0 & 0 \\0 & 1 & 1 & 0 & -1 \\0 & 0 & -1 & 1 & 0\end{array}\right]$$
    
    ![Untitled](Linear%20Algebra%20abcfa2b6ec3d4bb2856de5be1d39fca1/Untitled.png)