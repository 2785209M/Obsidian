
The **Laplace transform** converts complex integral or differential equations in the time domain into simpler linear algebraic equations in the frequency domain, showing the stability and transient behaviour of a dynamic system

Laplace Transform Function: $F(s) = \int_{\infty}^{0}f(t)e^{-st}dt$ 

![[Pasted image 20260201144439.png]] 

Laplace Transform Example:
$f(t) = 1$ 
$F(s) = \int_{0}^{\infty}e^{-st}dt$
$= \frac{1}{s}[e^{-st}]_{0}^{\infty}$ 
$= \frac{1}{s}(0-1)$
$= \frac{1}{s}$ 

Common Laplace transforms will be given in the exam data sheet.

In Laplace transforms the letter s is used to represent complex frequency variables

Below is a list of how applying a Laplace transform effects a function and its derivatives.
![[Pasted image 20260204182831.png]]

This is an example of how a Laplace transform can be applied to a second order differential equation:
![[Pasted image 20260204183721.png]]
![[Pasted image 20260204183801.png]]

#### Transfer Function 

The __transfer function__ is the ratio of input to output in the Laplace domain
Transfer Function: $G(s) = \frac{X(s)}{U(s)} = \frac{p(s)}{q(s)}$ 
Where X(s) represents the Laplace transform of the input signal
And U(s) is the Laplace Transform of the output Signal

q(s) = 0 is the characteristic equation. Useful for analysing the stability of systems

You can tell the order of a system by looking at the highest power of s in the denominator. For example: $G(s) = \frac{s^2 + 1}{s^3 - 5s^2 +9s + 16}$ (Characteristic equation  $s^3 - 5s^2 + 9s + 16 = 0$) represents a third order system.

Transfer Function Example: 
$\ddot{x}(t) + 2\zeta\omega\dot{x} + \omega_{n}^{2}x(t) = Ku(t)$ -> Laplace Transform 
-> $s^2X(s) + 2\zeta\omega_{n}sX(s) + \omega_{n}^{2}X(s) = KU(s)$
-> $X(s)(s^2 + 2\zeta\omega_ns + \omega_{n}^{2} = KU(s)$ 
-> $G(s) = \frac{X(s)}{U(s)} = \frac{K}{s^2 + 2\zeta\omega_ns + \omega_n^2}$ = The transfer function

#### Characteristic Equation

The characteristic equation is the denominator of the transfer function. Solving the characteristic equation tells you if a system is stable or not. 

![[Pasted image 20260205172052.png]]


#### Step Input

The step input is the Laplace transform of the unit step. This is usually $\frac{1}{s}$. However is the unit step is 5, the step input would be $\frac{5}{s}$, etc.

#### Inverse Laplace Transform

Transforms complex frequency domain functions into time domain functions. No formula is given in the lecture slides so just use the table.

 Example: $F(s) = \frac{4}{s-2}-\frac{3}{s+5}$ ($\frac{1}{s+a}$ -> $e^{-at}$)
 -> $f(t) = 4e^{2t} - 3e^{-5t}$ 

#### Partial Fractions

The Transfer function can be represented as partial fractions.
$G(s) = \frac{p(s)}{q(s)}$ 

Case 1: Expressing q(s) as linear fractions $q(s) = (s-a_1)(s-a_2)...(s-a_n)$ 
$\frac{p(s)}{q(s)} = \frac{A}{s-a_1}+\frac{B}{s-a_2}+...+\frac{W}{s-a_n}$ 

Case 2: Expressing q(s) as a repeated factor $q(s) = (s-a)^n$ 
$\frac{p(s)}{q(s)} = \frac{A}{(s-a)}+\frac{B}{(s-a)^2}+...+\frac{W}{(s-a)^n}$  

$G(s) = \frac{s}{(s+1)^3}$
-> $\frac{s}{(s+1)^3} = \frac{A}{(s+1)}+\frac{B}{(s+1)^2}+\frac{C}{(s+1)^3}$  

Case 3: q(s) has a quadratic term $q(s)=(s^2+as+b)(s+c)$ 
$\frac{p(s)}{q(s)}=\frac{As+B}{s^2+as+b}+\frac{C}{s+c}$ 
$G(s)=\frac{5s}{(s^2+s+1)(s-2)}$ 
-> $\frac{As+B}{s^2+s+1}+\frac{C}{s-2}$ 

#### Example

![[Pasted image 20260205173334.png]]

1. Start with the time domain representation of the system
2. take the Laplace transform to get the frequency domain representation
3. Calculate the Transfer Function. This is equal to the Laplace transform of the output X(s) over the Laplace transform of the input U(s).
4. Use the characteristic equation to find the roots (the poles) of the Laplace transform. If the poles are below zero the system is stable. If they equal zero the system is marginally stable. If they are greater than zero the system is unstable.
5. We are given the step input  $U(s) =\frac{1}{s}$ . Multiply the transfer function by this value to give the Laplace transform of the output. Perform Partial Fraction Decomposition on the output transform to find the amplification coefficients for the poles of the Laplace transform
6. Finish by performing an Inverse Laplace Transform on the Decomposed Output transform. This gives you the time domain representation of the systems output, which can be plotted.