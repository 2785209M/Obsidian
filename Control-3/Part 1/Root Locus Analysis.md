**DEFINITION**: The root locus diagram is a method for studying the closed loop migration of poles in a system as a parameter (typically the gain, K) varies from zero to infinity.

# Properties of a Root Locus Diagram

![[image.png]]

**Points worth Noting**:
The closed loop zeros are the same as the open loop zeros.
The closed loop poles are determined by roots of the characteristic equation.

The complex values of s which are **Solutions of the characeristic equation** for varying values of **K** form the **Root Locus** of the system.
$F(s) = 1+KG_o(s) = 0$
$G_o(s) = -1/K$ 
$|G_o(s)| = \frac{1}{K}$
$argG_o(s) = -180\degree+-K.360\degree$   

# Asymptotes
An asymptote is a line that tends towards another line but never reaches is in infinity.

In root locus analysis you calculate the asymptotes by the following equation:
$\theta = \frac{180}{n-m} + \frac{K360}{n-m}$ 
The number of asymptotes you have is the number of poles minus the number of zeros (represented by n-m). The asymptotic lines rotate around the x axis like a clock hand starting at quarter past.
The asymptotic lines originate from the meeting point of the poles. The meeting point can be calculated using the following equation:
$X=\frac{\Sigma R(p)-\Sigma R(z)}{n-m}$ (that's the sum of the real parts of the poles minus the sum of the real parts of the zeros over the number of poles minus the number of zeros)

## Types of Aymptotes
