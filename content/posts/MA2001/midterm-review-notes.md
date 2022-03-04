---
title: "Midterm Review Notes"
date: 2022-03-04T13:19:44+08:00
draft: false
series: ["MA2001"]
tags: ["Linear Algebra"]
---

## Linear Systems

- A linear system is a set of linear equations. 
- A linear system is **consistent** if it has 1 or infinite solutions, and **inconsistent** if it has no solutions. 
- There are only 3 different situations to the solutions of a linear system:
	- no solution (inconsistent)
	- 1 solution (consistent)
	- infinite solutions (consistent)
- A **consistent** linear system is different from a **homogeneous** one. 
	- **Consistent** means **having solutions**;
	- **Homogeneous** means a linear system where **all the constants are zero**. 
- Linear systems can be represented by augmented matrices. 

## REF, RREF, and Elementary Row Operations

- Augmented matrices (and matrices in general) can be reduced to their **Row Echelon** and **Reduced Row Echelon** form via elementary row operations. With the REF and RREF, we can find out the solution, or general solution, to the linear system. 
- There are three types of **elementary row operations**, each corresponding to a type of **elementary matrices**. 
	- $R_{i}+kR_{j}$ : add $k$ times the $j^{\text{th}}$ row to the $i^{\text{th}}$ row;
		- This approach **does not affect** the determinant of the matrix;
		- This corresponds to the pre-multiplication of the elementary matrix whose $(i, j)$-element is $k$.
	- $R_{i}\leftrightarrow R_{j}$ : exchange the $j^{\text{th}}$ row and the $i^{\text{th}}$ row;
		- This approach would result in the **negation** of the determinant of the matrix: if $\textbf{A}\xrightarrow{R_{i}\leftrightarrow R_{j}} \textbf{A}'$, then $\det({\textbf{A}'})=-\det({\textbf{A}})$.
		- This corresponds to the pre-multiplication of the elementary matrix whose $i^{\text{th}}$ row and $j^{\text{th}}$ row are exchanged. 
	- $kR_{i}$: multiply the $i^{\text{th}}$ row by $k$.
		- This approach would result in the **multiplication** of the determinant of the matrix by $k$: if $\textbf{A}\xrightarrow{kR_{i}}\textbf{A}'$, then $\det({\textbf{A}'})=k\det({\textbf{A}})$.
		- This corresponds to the pre-multiplication of the elementary matrix whose $(i, i)$-element is $k$ as opposed to $1$. 
## Invertible Matrices

- A matrix $\textbf{A}$ is **invertible** if there exists another matrix $\textbf{B}$ such that $\textbf{A}\textbf{B}=\textbf{I}$. $\textbf{B}$ is $\textbf{A}$'s  inverse. We denote the inverse of $\textbf{A}$ as $\textbf{A}^{-1}$. 
- When a matrix has an **inverse**, we say that it is **invertible**. When it has not, we say that it is **singular**. 
- **\[Important\]** Let $\textbf{A}$ be a square matrix, and the following properties are equivalent:
	- $\textbf{A}$ is an invertible matrix;
	- The linear system $\textbf{A}\textbf{x}=\textbf{b}$ has a unique solution; 
	- The linear system $\textbf{Ax}=\textbf{0}$ has only the trivial solution;
	- The **Reduced Row Echelon Form** of $\textbf{A}$ is $\textbf{I}$;
	- $\textbf{A}$ is the product of elementary matrices. 
	- The determinant of the matrix is not zero. $\det({\textbf{A}})\neq 0$.
- Finding the Inverse
	- The inverse of a matrix can be calculated using **augmented matrices**. With start with $(\textbf{A}|\textbf{I})$, where $\textbf{A}$ and $\textbf{I}$ are square matrices of the same size. Then, we reduce $\textbf{A}$ to $\textbf{I}$ via elementary row operations. Then, $\textbf{I}$ or the right side of our augmented matrix would become $\textbf{A}^{-1}$.
	- The inverse of a matrix can also be found with the **determinant** and the **adjugate** matrix using the fomula: $$\textbf{A}^{-1}=\dfrac{1}{\det({\textbf{A}})}\text{adj}(\textbf{A})$$
		- Adjugate matrix, $\text{adj}(\textbf{A})$, or adjoined matrix, is the **transpose** of the **cofactor matrix**. The cofactor matrix is $$\textbf{C}=\left((-1)^{i+j}M_{ij}\right)_{i\times  j}$$
		- $M_{ij}$, the minor, is the **determinant** of the $(n-1)\times  (n-1)$ matrix that removes the $i^{\text{th}}$ row and $j^{\text{th}}$ column. 
## Determinants
- The **determinant** of matrix $\textbf{A}$ is often denoted by $\det({\textbf{A}})$, or $|\textbf{A}|$.
- The determinant for $(a)$ is $a$, the determinant for $\begin{pmatrix} a & b \\\\ c & d \end{pmatrix}$ is $ac-bd$, and the determinant for $\begin{pmatrix}a & b & c \\\\ d & e & f \\\\ g & h & i\end{pmatrix}$ is $aei + bfg + cdh - ced - afh - bdi$. 
- **Determinants** can be defined recursively. 
	- To do this, we need to define the minor. Minor, $M_{ij}$, as mentioned above, is the determinant of the $(n-1)\times  (n-1)$ matrix that removes $i^{\text{th}}$ row and the $j^{th}$ column. For example, for the matrix $$\begin{pmatrix}a_{11} & a_{12} & a_{13}\\\\ a_{21} & a_{22} & a_{23} \\\\ a_{31} & a_{32} & a_{33}\end{pmatrix}$$ $M_{11}$ would be: $$\begin{vmatrix}a_{22} & a_{23} \\\\ a_{32} & a_{33} \end{vmatrix}$$
	- Then, we can find the determinant of a matrix by taking the $i^{\text{th}}$ row, and then use the following formula: $$\det({\textbf{A}})=\sum\limits^{n}_{j=1}(-1)^{i+j}M_{ij}$$
- Some rules for determinants:
	- $\det({\textbf{A}})\det({\textbf{B}})=\det({\textbf{AB}})$
	- $\begin{vmatrix}  a_{11} & a_{12} & a_{13} \\\\ a_{21} & a_{22} & a_{23} \\\\ a_{31} & a_{32} & a_{33} \end{vmatrix} + \begin{vmatrix}  a'_{11} & a'_{12} & a'_{13} \\\\ a_{21} & a_{22} & a_{23} \\\\ a_{31} & a_{32} & a_{33} \end{vmatrix} = \begin{vmatrix}  a_{11} + a'_{11} & a_{12} +a'_{12} & a_{13} + a'_{13} \\\\ a_{21} & a_{22} & a_{23} \\\\ a_{31} & a_{32} & a_{33} \end{vmatrix}$
	- If 1 row / column of the matrix is zero, then the determinant of the matrix is zero. 
	- If two rows are proportional to each other, than the determinant of the matrix is zero. 
	- For a diagonal matrix, the determinant of the matrix is the product of all the elements along its main diagonal axis. $\det({\textbf{A}})=\sum\limits^{n}_{i=1}a_{ii}$. 
- If the determinant of the matrix $\textbf{A}$ is zero, then:
	- $\textbf{A}$ is invertible;
	- $\textbf{Ax}=\textbf{0}$ has only the trivial solution;
	- $\textbf{Ax}=\textbf{b}$ has a unique solution;
	- The **RREF** of $\textbf{A}$ is $\textbf{I}$.

## Matrix Calculations

- Let $\textbf{A}=(a_{ij})_{m\times p}$ and $\textbf{B}=(b_{ij})_{p\times n}$. Then, $\textbf{A}\textbf{B}$ is a $m\times n$ matrix where the $(i,j)$-entry of the matrix is the product of the $i^{\text{th}}$ row of $\textbf{A}$ and the $j^{\text{th}}$ column of $\textbf{B}$. I.e. let $\textbf{AB}=(p_{ij})_{m\times n}$, then $$\begin{align*}p_{ij}&=a_{i1}b_{1j}+a_{i2}b_{2j}+\cdots + a_{ip}b_{pi} \\\\ &=\sum\limits^{p}_{k=1}a_{ik}p_{kj}\end{align*}$$
- Matrix Arithmetics ($\textbf{A}$, $\textbf{B}$, $\textbf{C}$ are square matrices of the same size)
	- $\textbf{A}+\textbf{B} = \textbf{B} + \textbf{A}$;
	- $\textbf{A}\cdot \textbf{B} \neq \textbf{B}\cdot\textbf{A}$ (In most cases);
	- $\textbf{A}(\textbf{B} + \textbf{C}) = \textbf{A}\textbf{B} + \textbf{A}\textbf{C}$;
	- $(\textbf{A}+\textbf{B})^{2}=\textbf{A}^{2}+\textbf{AB}+\textbf{BA}+\textbf{B}^{2}$;
	- $\textbf{A}\textbf{I}=\textbf{I}\textbf{A}=\textbf{A}$;
	- $\textbf{A}\textbf{0}=\textbf{0}\textbf{A}=\textbf{0}$.

## Spans and Subspaces

- What does $\text{span}(v_{1}, v_{2}, \cdots, v_{n})$ mean?
	- It means the set which contains all linear combinations of $v_{1}, v_{2}, \cdots, v_{n}$
- How to pove that $\text{span}(v_{1}, v_{2}, \cdots, v_{n})\subseteq \text{span}(u_{1}, u_{2}, \cdots, u_{m})$ ?
	- We can create the augmented matrix $\begin{pmatrix} u_{1} & u_{2} & \cdots & u_{m} & | & v_{1} & v_{2} & \cdots & v_{n}\end{pmatrix}$, then reduce the left part to REF to find whether if the system if consistent. 
	- If the system is consistent, then $\text{span}(v_{1}, v_{2}, \cdots, v_{n})\subseteq \text{span}(u_{1}, u_{2}, \cdots, u_{m})$ is true. Otherwise, this is not true.
	- If we were to prove two spans are equal, then we prove $\text{span1}\subseteq  \text{span2}$ and $\text{span2}\subseteq \text{span1}$.
- A set $S$ is a subspace of $\mathbb{R}^{n}$ if $S=\text{span}(v_{1}, v_{2}, \cdots, v_{n})$ where $v_{1}, v_{2}, \cdots, v_{n}\in \mathbb{R}^{n}$. 
- What are the **three conditions** to satisfy for a set to be a **subspace**?
	- It is **not empty**: $\textbf{0} \in S$;
	- **Addition** is contained: if $\textbf{u}, \textbf{v} \in S$, then $\textbf{u} + \textbf{v} \in S$;
	- **Scalar multiplication** is contained: if $\textbf{u}\in S$, then $k\textbf{u}\in S, k \in \mathbb{R}$.
- Note that the solution set of a **homologous system** is a subspace because it satisfies that:
	- $\textbf{0}\in S$
	- If $\textbf{x}\_{0}$ is a solution to the homologous system $\textbf{Ax}=\textbf{0}$, then $\textbf{A}(k\textbf{x}\_{0})=k\textbf{0}=\textbf{0}$, then  $x\_{0}'=k\textbf{x}\_{0}$ is also a solution.
	- If both $\textbf{x}\_{0}$ and $\textbf{x}\_{1}$ are solutions to the homologous system $\textbf{A}\textbf{x}=\textbf{0}$, then $\textbf{A}(\textbf{x}\_{0}+\textbf{x}\_{1})=\textbf{0}+\textbf{0}=\textbf{0}$, therefore $\textbf{x}\_{2}=\textbf{x}\_{0} + \textbf{x}\_{1}$ is also a solution. 
- What is the **alternative definition** for **subspace**? (Used in abstract linear algebra)
	- Let $V$ be an non-empty subset of $\mathbb{R}^{n}$, then $V$ is a subspace of $\mathbb{R}^{n}$ if and only if $$(\forall \textbf{u}, \textbf{v} \in V)(\forall c, d\in R)[c\textbf{u} + d\textbf{v} \in V]$$

