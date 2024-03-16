## Theory
In digital circuits, a clock source is often required, commonly used ones include crystal oscillators (active crystals) and crystals (passive crystals), with crystals being more commonly used. For crystals, in general circuit designs, a capacitor is usually connected to ground at each end of the crystal, as shown in the diagram below:
![alt text](<assets/Crystal Load Capacitance Calculation/image.png>)

In the circuit above, the capacitors at both ends of the crystal are used to match the load capacitance of the crystal (CL). If the load capacitance cannot be met, it can lead to frequency deviation of the crystal, and in severe cases, the crystal may fail to oscillate. In circuit design, it is essential to meet the load capacitance requirements of the crystal as much as possible to ensure the crystal operates optimally. The formula for calculating the load capacitance is as follows:

$ C_L = \frac{C_1 \times C_2}{C_1 + C_2} + C_S $

Where $C_S$ is the practical capacitance of the circuit board, generally taken as `3~5pF`. If $C_1 = C_2$, the formula can be simplified as follows:

$ C_L = C_1 / 2 + C_S $

## Example

![alt text](<assets/Crystal Load Capacitance Calculation/image-1.png>)

The above is the manual for a certain crystal. From the manual, we can see that the load capacitance of the crystal is `8pF`. If $C_S$ is set to `3pF`, we can calculate that $C_1 = C_2 = 10pF$. If $C_S$ is set to `5pF`, we can calculate that $C_1 = C_2 = 6pF$. In general, taking a value between `6~10p` for $C_1$ and $C_2$ should be fine. If stricter frequency requirements are needed, waveform testing should be done using an oscilloscope. (Typical oscilloscope probes themselves have a parasitic capacitance of a few pF, which may not measure correctly. If conditions allow, an active probe can be used for measurement.)