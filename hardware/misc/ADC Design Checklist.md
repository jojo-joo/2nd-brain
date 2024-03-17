1. Alias, 高频(超过$f_{sample}/2$的部分)混叠到低频, 所以采样pin最好有一个RC滤波
![alt text](<assets/ADC Design Checklist/image.png>)
2. latch up, 模拟PIN的电压全部必须检查, 有没有超过范围 
![alt text](<assets/ADC Design Checklist/image-1.png>)
3. Grounding, 模拟低和数字低有区别
![alt text](<assets/ADC Design Checklist/image-2.png>)
4. Impedance, 如果ADC PIN外部的输入阻抗太大, 充电时间太长, 会导致读到的值小于实际值, 所以这个时候需要延长读时间
![alt text](<assets/ADC Design Checklist/image-3.png>)
5.  Cross Talking, 几个ADC PIN公用一个充电电容