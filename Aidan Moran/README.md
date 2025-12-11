# Aidan Moran ECE 445 Lab Notebook #
Fall 2025, Nuerogaurd, Team 31
All images, graphs, designs, etc. are in the documents folder

## 9/11
Initial meeting with nuerogaurd team. I selected this project as I felt the focus on power electronics and specifically buck converter design matched well with the courses I was taking (ECE 464) and previous internship experience. Main criteria for our scope of the project sounded like 
  - power supply to interface
  - creation of the nerve stimulation waveform
  - Unsure if we will have to do the signal prossesing and nerve detection as well.

Next Steps 
  - Reading literature
  - contact fellow nuerogaurd team members

## 9/16
Meeting with nuerogaurd members. We divided project scope and focus. I will be taking the power system (2kV DC step down to 5V DC) 
Next steps
  - reading literature on nerve stimulation, Electrosurgical Unites
  - Papers

N. D. Shilev and G. A. Konesky, “Multi-button electrosurgical apparatus,” U.S. Patent 9,326,810, May 3, 2016.

M. Singhal, K. Kiunga, N. Kelhofer, P. Dullur, N. Chigullapally, S. Pappu, and M. L. Oelze, “Expanding the Scope of Intraoperative Neuromonitoring with Nerve-Specific Stimulatory Waveform Design,” Carle Illinois College of Medicine and Department of Electrical and Computer Engineering, University of Illinois Urbana-Champaign, Urbana, IL, USA.
Chappell AG, Bai J, Yuksel S, Ellis MF. Post-Mastectomy Pain Syndrome: Defining Perioperative Etiologies to Guide New Methods of Prevention for Plastic Surgeons. WORLD JOURNAL OF PLASTIC SURGERY. 2020;

Chappell AG, Bai J, Yuksel S, Ellis MF. Post-Mastectomy Pain Syndrome: Defining Perioperative Etiologies to Guide New Methods of Prevention for Plastic Surgeons. WORLD JOURNAL OF PLASTIC SURGERY. 2020;9(3):247-253.

Coopey S, Keleher A, Daniele K, et al. Careful Where You Cut: Strategies for Successful Nerve-preserving Mastectomy. Plastic and reconstructive surgery Global open. 2024;12(5):e5817-e5817

## 9/23
Meeting with team to discuss block diagram 
- High voltage stage, must interface with the Electrosurgical Machine.
- Need considerations for isolation and safety,  IPC 2221-b specifies design clearances, creepage, and dialectic breakdown      for high voltage PCB designs
- Need to find resources on how to design high voltage PCB. look at requirements for different dialectrics, trace spacing,      specialized components, etc.
- Mid voltage stage - get AC input and step down to ~100V. Feel confident finding a buck converter and selecting LC components, filtering, etc to manage this.
- Find resources on noise tolerences for hospital applications, how much noise can be put back into the cauterizing machine without causing issues ?

## 9/29
Design buck converter for first round PCB orders october 8th. Selected the LM5186, 120Vin max, 300mA max output current wide Vin DC-DC converter. I feel one of the big difficulties for the project is the very unconventional nature of the design. It is high voltage, yet low power. Most reference design I have found are high voltage, and high power, which is not what we need. 

Design notes
Switching frequency selection
- need something fast to decrease L size, but not too fast there are timing issues in the design. Going from 100V - 5V will require a miniscule duty cycle of 0.05. Doing this on too high of a switch frequency means the switch will not be able to keep up
- Reference design suggests 750kHz, choose 750kHz. gives Rt = 33.2 k
  
  $$R_T = \frac{2500 \, V_{\text{OUT}}}{F_{\text{SW}}} \;\text{k}\Omega$$

Inductor selection
- need to size up the inducotr significantly, want very tight output tolerences. Any noise created by the buck converter will spread throughought the low voltage design, potentially interfering with the creation of nerve stimulatory waveforms

  $$L = \frac{(V_{\text{IN}} - V_{\text{OUT1}})}{K \, I_{\text{PRI}} \, F_{\text{SW}}}
\frac{V_{\text{OUT1}}}{V_{\text{IN}}}$$

selecting a value of 120 uH, sized up slightly from this equation. there is no consequence for using a larger inductor other than taking up more space on the PCB. Must manage space but I beleive this is worthwile tradeoff for the quality of the output. 

Sizing of Cout - again need larger Cout to minimize ripple, particularly from how extreme the Vin-Vout conversion is. 

$$C_{\text{OUT}} > \frac{\Delta I}{8 F_{\text{SW}} V_{\text{ripple}}}$$

$$\Delta I = \frac{(V_{\text{IN}} - V_{\text{OUT1}})}{L F_{\text{SW}}}
\frac{V_{\text{OUT1}}}{V_{\text{IN}}}$$

very interesting to the application of 464 content within this class. Inductor ripple current creates the output voltage ripple, want L and C as large as possible to reduce this, without sacrificing PCB sizing. Additionally adding 10n capacitors around this design to filter out high frequency noise, as this will also interfere with the output quality.

Feedback Resistors.

Working of reference as well. As this converter will not need to handle extreme transients (I think), there does not need to be capacitors in the feedback bath. 

$$R_{\text{FBT}} = R_{\text{FBB}} 
\left( \frac{V_{\text{OUT}}}{V_{\text{REF}}} - 1 \right)$$

Select RFBT as 198.6k, RFBB as 66.8k. Reference voltage is 1.8V 

Input capacitors

Adding them close to the Vin pin to reduce parasitic inductance in the input path/ voltage spikes. Large filtering capacitor from the full bridge rectifier AC/DC converter shouold take care of supplying most of the charge. Adding 10nF high frequency capacitors as well. 

Boost capacitor - use 2.2n
EN - dont think this design will need, Tie to Vin
PGND vs AGND - tie to one plane

Other considerations / need more research
- what transients will this design need to handle
- what filtering (EMI/EMC) to not disrupt other medical devices.
- Thermal considerations, low power but how do I simulate these. Do not want to order a PCB and find out it cannot handle the power present. Maybe look at increasing pour size, Vias, extra layers, ETC.


AC/DC converter
- use high frequency shotkey diodes for the full bridge rectifier
- oversize capacitor, with senior design funds and nuerogaurd funds I see no reason to not spend more for larger filter and thus better performance.

Design and simulation is looking good in PSpice for TI
- Input ripple - 0.8V at 100V AC sine inout 1000Hz
- Output ripple - 300 mV
  
## 10/5

PCB layout and design
Considerations
- keep SW node as small as possible
- HF filtering caps around the design
- keep feedback line away from switch node to not couple noise into the 

  Resources
  - Reference PCB design for LM5186
  - previous experience
    
## 10/10 
- prep for Design document, need to research more the high voltage system
- If we want the nuerogaurd to be purely powered by a high voltage source, how can we cantrol the high voltage inverter without a 5V dc/low level voltage supply already there. Would be easier to pivot to having an external power source for this.
- Look at self resonating oscillators/relaxation oscillators, probably the best idea as they do not require active control from low voltage logic and can self resonant from their LC/LCR constants

## 10/15
Email correspondance with proffesor Fliflet and professor Banerjee discussing options for high voltage measurements. A few key questions must be answered. 
- What equipment is available that can handle these measurements. Currently, students have acces to 1.5kV max differential probes in the power electronics labaratory.  However, we must engineer for worst case scenaries, not an ideal scenaria. Max open circuit output would be 3.9 kV. Is there any equipment that students have acces to that would allow for this measuremnt, ideally a probe rated for up to 5kV-6kV
- Alternative measurement options. Could we use a resistive load and current probe to get proxy measurements for voltage, in an inderect manner.
- SAFETY. How can this measurement be done safely, without risking harm to students. An electrocautyr machine is easily capable of damaging human flesh, and worst case scenario would be permenant damage to student, and potentially death. Are there high voltage encolsures, isolation mechanisms, and remote sensing we can implement to mititgate safety risks ?

  The outputs of the Bovie Electrosurgical generator are given below.

| Mode        | Output Power      | Output Frequency                 | Repetition Rate       | Open Circuit Vpeak max | Crest Factor* (Rated Load) |
|------------|-------------------|----------------------------------|-----------------------|------------------------|----------------------------|
| Cut        | 120 W @ 500 Ω     | 357 kHz ± 50 kHz                 | N / A                 | 1250V                  | 2.9 ± 20%                  |
| Blend      | 90 W @ 800 Ω      | 357 kHz ± 50 kHz                 | 30 kHz ± 5 kHz        | 1850V                  | 3.3 ± 20%                  |
| Coagulation| 80 W @ 1000 Ω     | 475 kHz ± 19 kHz                 | 57 kHz ± 5 kHz        | 3300V                  | 5.5 ± 20%                  |
| Fulguration| 40 W @ 1000 Ω     | 410 kHz ± 50 kHz                 | 25 kHz ± 5 kHz        | 3900V                  | 7.7 ± 20%                  |
| Bipolar    | 30 W @ 200 Ω      | 520 kHz (-14 kHz, +29 kHz)       | 32 kHz ± 5 kHz        | 1200V                  | 6.9 ± 20%                  |

\* an indication of a waveform's ability to coagulate bleeders without a cutting effect

Proffesors responded, requesting more data and research before this may get started. I believe a well thought out document outlining the details of this measurement will be sufficient to gain instructor approval. 

# 11/20 Meeting with Meenakshi at Carle Hospital Cardiovascular Center

Meeting with Meenakshi in the morning of November 20 to get a look at the Bovie Electrosurgical generator. While not a technical meeting, it was very inspring to see the depth and breadth of novel technologies being implemented in the medical space. I was able to visit their surgical training center and look at the Electrosurgical generator. It would have been helpful to be able to take it apart and atempt to reverse engineer the electronics present, However this was not possible given the price of these machines and the fair of permanently damaging its operatio.  

## 11/17
PCB has been delayed until after fall break

## 11/30

PCB construction session 
Parts and PCB have finally arrived, can now start assembly. Total assembly took about 3 hours. No issues with assembly.
During testing there was in issue with the vairac interfacing with the oscilliscope. When the probe was set to high Z input, with the mid voltage stage disconnected, It worked well and I could easily see a smooth sine wave from the variac, measured to be around 59.7 Hz. However when the device was connected, and I tried to turn on the variac, the variac would buzz loudly, and it would be difficult to turn, making me think there was some type of short circuit being created through the oscilliscope. When the device was connected, and there was no oscilliscope probe measuring the variac output, the device and variac worked as expected. After double checking the design and making sure all solder joints were done well and there was no possible short circuit from the Mid Voltage stage, I concluded that the oscilliscope probe must somehow be causing interference when connected to the output of the variac. 

The results of the mid voltage stage are as follows 

| Load | Output Voltage | Output Ripple | AC Input | Rectified DC | Rectified DC Ripple |
|------|----------------|---------------|----------|--------------|----------------------|
| 100mA | 5V | 288 mV  | 40V | 39.6V | 6.9V |
| 0mA   | 5V | 1400 mV | 40V | 39.6V | 6.9V |

The full load ripple is slightly outside the ripple specified in the design document of 250mV. However I am confident that slightly increasing the output capacitance, say from 22uF to 27 uF, would cause a suitable drop in the output voltage ripple that would bring this design within specification. Again with the delays with the PCB we missed an entire round of prototyping and revision that would have allowed for these changes. 

Because of the soft start mechanism in the buck converter, I am not worried about start up transients corrupting the quality of the output voltage and damagin the low voltage analog circuitry for this device. Additionally, there are other safety mechanisms such as under voltage lockout and overvoltage protection to ensure that the device safely and reliably outputs 5V. 

### Testing integration with low voltage stage

Setup: Power supply used was a varaic. Input was 120V 60Hz, output was tested from 30V-60V AC at 60Hz. No visible issues with the power supply. No smoke, no parts felt excessively hot after testing. The power supply was ran continuously for around 20 minutes, and performad consisntently throughought this timeframe. The Input from the variac was rather noisy. However hoping this is not a pervasive issue as the output from the electrosurgical generator should be higher frequency, but also significantly less noisy. Again this is just a suspicion as we were unable to characterize the output of the ESU this semester, and I do not want to rely on such suspicion for design decisions. May be wise to add a lowpass EMI filter to the input of the buck converter in further design implementations

The low voltage PCB worked identically with the mid voltage stage power supply as with a benchtop DC power supply. All performance waveforms of the low voltage PCB were verified, and we are confident that it would then work during our demo tommorow. 

## 12/01
Demo went well. Proffesor Yu seemed satisfied with the quality of our demonstration, and meeting all demonstration criteria. He however, hinted that the scope of our project may have been too easy. 

## 12/02
With the senior design demo being completed, this concludes the technical development for this project 

# END OF LAB NOTEBOOK
  

