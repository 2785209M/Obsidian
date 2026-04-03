The Nyquist Stability Criterion allows us to deduce the characteristics of a closed loop system just by looking at the open loop system.

![[image.png]]

1. The point where $G_o(j \omega)$ crosses the x axis is lambda. $\frac{1}{\lambda}$ gives us the **Gain margin**, kmax. The gain margin is the maximum amount of gain you can have before the system becomes unstable. If the line crosses -1 before crossing the x axis, the system is **UNSTABLE**
2. The **Phase Margin** is given by $|-180\degree - ∠G_o(j\omega)|$ at $|G_o(j\omega)|=1$. This is the point where the line crosses the dotted circle which goes through all the 1s.
3. The phase margin is related to the **time delay**. $\frac{\theta_m}{360}=\frac{\Delta}{T}=\frac{\Delta}{\frac{2\pi}{\omega}}$ . This is known as the **time margin**. The phase margin is the maximum amount of delay you can add to a system before it becomes unstable.
![[image-2.png]]
![[image-1.png]]

