[[Laplace Transforms]] [[Root Locus Diagrams]]
the Routh-Hurwitz stability criterion allows you to predict the stability of a system without needing to solve the characteristic equation

Start by building a table with every power of s:
![[Pasted image 20260205174844.png]]
![[Pasted image 20260205174900.png]]
the columns should be filled with the coefficients of s from the equation that have the same parity as the s from the row. e.g the row for $s^3$ is filled with the coefficients of $s^3$ and $s^1$ , whilst the row of $s^2$ is filled with the coefficients of $s^2$ and $s^0$ .

$b_i$ and $c_i$ are calculated using coefficients.

![[Pasted image 20260205181322.png]]
In the calculation above, you always multiply the matrix of the s3 and s2 with -1 over the first value of s2. To calculate c1 you move down by one row and repeat the process of b1

![[Pasted image 20260205181402.png]]

If there is a negative number in the first column, the system is unstable.