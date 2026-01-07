# Cramers Rule

## Dependencies

Used to solve `Linear System Matrices`. The solution taken by calculating the `determinant` of the $x_1$ value divided by the determinant of the x value.

In terms of the equations:

$$
x_1 = \frac{\Delta_1}{\Delta}
$$

where:

- $\Delta$ = determinant of the coefficient matrix
- $\Delta_1$ = determinant of the matrix with the first column replaced by the constants

For example, $\Delta$ can be written as:

$$
\Delta = \begin{vmatrix}
a_{11} & a_{12} & \cdots & a_{1n} \\
a_{21} & a_{22} & \cdots & a_{2n} \\
\vdots & \vdots & \ddots & \vdots \\
a_{n1} & a_{n2} & \cdots & a_{nn}
\end{vmatrix}
$$

The generalised version of this becomes:

$$
x_n = \frac{\Delta_n}{\Delta}
$$

The constants is the $b$ column.

$$
\begin{bmatrix}
a_{11} & a_{12} & \cdots & a_{1n} \\
a_{21} & a_{22} & \cdots & a_{2n} \\
\vdots & \vdots & \ddots & \vdots \\
a_{n1} & a_{n2} & \cdots & a_{nn}
\end{bmatrix}
\begin{bmatrix}
x_1 \\
x_2 \\
\vdots \\
x_n
\end{bmatrix}
=
\begin{bmatrix}
b_1 \\
b_2 \\
\vdots \\
b_n
\end{bmatrix}
$$

So the $\Delta_n$ becomes:

$$
\Delta = \begin{vmatrix}
b_{11} & a_{12} & \cdots & a_{1n} \\
b_{21} & a_{22} & \cdots & a_{2n} \\
\vdots & \vdots & \ddots & \vdots \\
b_{n1} & a_{n2} & \cdots & a_{nn}
\end{vmatrix}
$$

## Implementations

## Dependents

## Interactions
