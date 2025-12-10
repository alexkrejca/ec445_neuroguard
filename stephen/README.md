# Stephen Simberg
# Lab Notebook

# 9/24
Held a meeting with the lead on the medical team for NeuroGuard,
received clarification on what we are building:
A nerve stimulation waveform from 100Hz-1kHz that only is ON during the OFF cycle of ESU source. Power needs to be from wall (plug in)
or directly from ESU source.

# 9/29
First Design of ECU Monitor in LTSpice has been created.
Simulation Screenshot shown in ECUMonitor folder.  
2000V AC Source Capacitance Voltage Divider:  
```text
Ratio ≈ 10pF / (10pF + 3900pF) = 10 / 3910 ≈ 1/391 

This reduces the peak voltage from 1850V down to a much safer level: 
1850V / 391 ≈ 4.7V

▪ 1 / 307kHz ≈ 3.26 µs
▪ τ = R * C = 5,600 Ω * 0.000000001 F = 5.6 µs 
▪ V(ref) = 5V * (R3 / (R6 + R3)) ≈ 0.3V
```
# 10/6
All parts received for beginning to assemble first design of ESU Monitor, incuding ad dev board ESP32, comparator, resistors.

# 10/7
Found that the ESP32 only accepts 3.3V into dev board. This required picking up new resistors from self-service to output 3.3V 
where-ever we were outputting/receiving 5V. This is temporary for
bread-board demos of ESU Monitor.
# 10/8
First breadboard completed, screenshots in ESU Monitor folder. Everything went smoothly.

# 10/14
Created Timer IC circuit output at 1kHz and 100 us high, and integrated with ESU monitor circuit, uploaded LTSpice in ESU_MONITOR_V2 folder.  
**555 Timer High/Low Calculations:**

$$ T_{high} = 0.693 \times R_a \times C $$
$$ T_{high} = 0.693 \times 15,000 \Omega \times 0.00000001 F $$
$$ T_{high} = 100 \mu s $$

$$ T_{low} = 0.693 \times R_b \times C $$
$$ T_{low} = 0.693 \times 130,000 \Omega \times 0.00000001 F $$
$$ T_{low} = 900 \mu s $$
# 10/16
Designed first low-power PCB v1. Realized had issue with noise in output of cicuit when using actual components instead of the default LTSpice ones. Fixed this by adding a comparator output at end to filter out the noise and put a voltage divider to output 100mV from the 5V output of the comparator with the same nerve stimulation waveform as previous without any noise.


# 10/17
Low Power PCB has been ordered and approved by PCBway.

# 10/24
Designed 2nd BreadBoard of Low Power circuit. Changed out resistors to work with 3.3V ESP32.

# 10/28
2nd BreadBoard Demo was successful, images in ESU_monitor_V2 folder.

# 11/1 
Noise isolation added in first PCB required for 2kV filtering and dividing to 5V AC signal is not able to be tested in ECE 445 lab, so desigined a PCB_stripped. This takes as input a max 25V AC signal from waveform generator that acts as ESU source without actually being 2kV. This involved changing 2 Capacitors in voltage divider to 1, same exact characteristics displayed.
Ordered this PCB as well.

# 11/18 
PCBs ordered have finally arrived, picked up, verified look good.

# 11/19
ESU Monitor part of PCB built, same characteristics as in simulation shown on breadboard.

# 11/20
All parts of PCB have been soldered on, having issue programming micorcontroller, cannot detect device from programmer.

# 11/21
Received a new USart to USB-A adaptor, and that worked! Successfully ran end-to-end test of low-power PCB. Had to add 
1kOhm resistor to back of PCB, as forgot to add it in the schematic.

# 11/30
Added High Voltage Power component as input to low power PCB 5V source, and everything worked. 

Had to replace ATTiny85 with a spare because it stopped reposonding, possibly due to wrong output selected from DC Power Supply during testing. After replacement everything again ran smoothly.

# 12/1
Successful final demo, images attached.
