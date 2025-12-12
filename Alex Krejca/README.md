# Alex Krejca
# Lab Notebook
# 9/16
First meeting with team members and TA to discuss the start of our project. Went over the general goals of the project and planned the first steps.
# 9/19
Researched ways to separate the project into different modules to complete. Determined separating the project by voltage level was a good way to start. Completed the project proposal, specifically worked on the problem, solution, and ethics and safety section.
# 9/22
Presented our project proposal to our TA Shiyuan Duan and Professor Yu.  The project was approved and both provided feedback.


# 9/24
Held a meeting with Meenakakshi Singhal, the main member of the Carle Medical Team we are working with. We received clarification on some of the specifications the device needs to adhere to. THe nerve stimulation waveform needs to be from 100Hz-1kHz that will only be ON when the ESU source is OFF, or cauterizing. The device needs to be powered from the wall or ESU source. Most likely,  we will work to power it from the ESU source.
# 9/27
Met with Stephen to go over the design of the low power stage. Put the circuit that was previously designed into LTSpice to see how it functions. Multiple issues in this circuit. Determined that this ESU monitor should be redesigned. 
# 9/29
This week, Stephen worked to hash out a starting ESU monitor design that can be implemented for the first breadboard demo next week. Provided updates on the design and reviewed the circuit in simulation to test the functionality. Some of the design calculations noted before, such as the input source voltage divider at the 2000V from the ESU.

**1. Capacitive Divider Ratio**
$$
\text{Ratio} \approx \frac{10\text{pF}}{10\text{pF} + 3900\text{pF}} = \frac{10}{3910} \approx \frac{1}{391}
$$

**2. Voltage Scaling**
$$
V_{out} = \frac{1850\text{V}}{391} \approx 4.7\text{V}
$$

**3. Frequency Period**
$$
T = \frac{1}{307\text{kHz}} \approx 3.26\mu\text{s}
$$

**4. RC Time Constant**
$$
\tau = R \cdot C = 5,600\Omega \cdot 1\text{nF} = 5.6\mu\text{s}
$$

**5. Reference Voltage (Voltage Divider)**
$$
V_{ref} = 5\text{V} \cdot \left( \frac{R_3}{R_6 + R_3} \right) \approx 0.3\text{V}
$$
These calculations are important as it reduces the voltage significantly.
# 10/6
Met with Stephen in the lab to assemble this first ESU monitor design from simulation. Parts were easily acquired using parts found in the lab and the ECE service shop.
# 10/7
Initial implementation problems occurred on this day. The main problem found was that the ESP32 works at a 3.3 V logic level. This meant we needed to make changes to some of our voltage divider calculations and acquire different parts from self-service to work at this voltage level. I will work to figure out a different solution to this problem after our first breadboard demo tomorrow.
https://learn.sparkfun.com/tutorials/esp32-thing-hookup-guide/hardware-overview 
# 10/8
Met with TA for our first breadboard demo, everything works successfully in our first design. This breadboard design is below.
Breadboard 1 Design: 
![alt text](https://github.com/alexkrejca/ec445_neuroguard/blob/main/Alex%20Krejca/IMG_0089.JPG)
# 10/12
Timer was added to the ESU monitor to match the BOVIE specifications that were noted by the Carle Medical Team. This was done using a 555 Timer IC. 
# 10/13
Worked on finishing the design document while Stephen converted the LTSPice simulation onto a PCB in KiCAD before the order deadline on Thursday.
# 10/17
The Low Power PCB was ordered today due to the extension in the order deadline.
# 10/24
Met with Stephen in the lab to add changes made to the design onto the breadboard and adjusted the resistors for the 3.3V output ESP32.
# 10/28
Met with TA, 2nd breadboard demo went well, no issues. Waveform’s shown below.
Breadboard 2 Waveforms: 
![alt text](https://github.com/alexkrejca/ec445_neuroguard/blob/main/Alex%20Krejca/IMG_0134.JPG)
# 11/3
Not much happened this week, modifications made to the previous PCB submitted as it became clear we would be unable to test 2000 Volts in the lab. Stephen recommended we resubmit the PCB with a design for the lower voltage AC source that can be tested in lab so another PCB was ordered. 
# 11/18
TAs informed us the PCBs ordered from the previous order arrived, will go in the lab tomorrow to assemble the Low Power PCB. Aiden’s PCB has arrived and will be assembled over break before the final demo. 
# 11/19
Soldered the ESU Monitor part of the PCB, some soldering difficulties but this part appears to work in simulation.
# 11/20
Soldered the rest of the PCB board, but the microcontroller used, the ATTiny85, would not let us program, will figure this issue out tomorrow before Mock Demo hopefully. Detection issue so either a soldering issue or the cable to connect to computer may not be working as this can be a common issue.
# 11/21
Went to one of the TA office hours to exchange the adaptor cable, and this solved our issue.1 minor issue was the omission of a resistor that was included in our schematic but accidentally omitted in the PCB design. Went through each section of the low-power stage and ensured each part worked, everything locked good for the mock demo.
# 11/30
Coming back from fall break, Aiden had assembled the Mid Voltage stage the previous day so we could implement all the stages together. However, we ran into a significant problem, the microcontroller has completely stopped responding, most likely due to overvoltage being into the microcontroller when testing the mid voltage stage. Desoldered this broken ATTiny85 and replaced it with the spare we had. Thankfully, everything worked after it was replaced so we were ready for the demo tomorrow.
# 12/1
TA’s and professors overviewed our final demo, everything worked accurately. An image of the successful waveforms is shown below.
Final Waveforms:
![alt text](https://github.com/alexkrejca/ec445_neuroguard/blob/main/Alex%20Krejca/IMG_0236.JPG)

