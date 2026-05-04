The slope of a bode plot is determined by the total number of poles and zeros in the transfer function. Every pole and zero creates a corner point in the plot. Each pole adds -20dB/Decade, and each zero adds +20dB/Decade.

To change an equation to frequency representation just change s to $j\omega$. take the absolute values of the poles to find the frequency representations of the corners.
 
The phase shifts by +-90 degrees for every pole/zero in the transfer function. It starts moving one decade before and stops one decade after the corresponding corner.

If the phase graph crosses 180 degrees and the magnitude graph hasn't crossed zero, the systen is unstable. 

Increasing and decreasing gain can change where the magnitude graph crosses the x axis.

The magnitude of the gain in decibels can be calculated using the equation $20log_{10}(K)$. You add this to the line on the magnitude graph.

That equation can also be used to calculate where a graph with a pole at 0 should begin. $-20log_{10}(\omega)$ will give you the magnitude of the line at any given point, not including gain and other corners. To include gain you should use $-20log_{10}(\omega) + 20log_{10}(K)$.

![[image-3.png]]

**Phase Margin**: Phase margin can be found from a bode plot by ascertaining the difference between -180 and the point on the phase asymptote where the magnitude asymptote crosses the x axis.

**Gain Margin**: Gain margin can be found by ascertaining the difference between the point on the magnitude asymptote where the phase asymptote crosses 180 and zero.