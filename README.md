# Control-PV-Cell
Using the Extremum Seeking Control technique to extract maximum active power from the photovoltaic system.
Implementation in Simulink version R2019.

<p align="center">
  <img width="460" height="300" src="https://github.com/rosasalberto/Control-PV-System/blob/master/images/photovoltaic-cell.jpg">
</p>

## Introduction

In this work, I present a method to extract the maximum active power of a photovoltaic plant. The theory of this method is explained and the simulation of the system is performed using Simulink. I compare the results of this method with those of the MPPT algorithm. 
It is well known that the dynamics of the system vary depending on the solar irradiation in an instant of time t.  The extremum seeking control method is applied to adapt to these changes in system dynamics.

## Theory

The goal of the extremum seeking control method is to find a system input that maximizes an objective function related to the output of the system.
It is widely used for complex nonlinear systems where the plant model is not available.

<p align="center">
  <img width="550" src="https://github.com/rosasalberto/Control-PV-System/blob/master/images/fig1.PNG">
</p>

  
In Figure 1 we can see the scheme of this control method. It is based on applying a disturbance in the control signal to find the gradient of the objective signal as a function of the control signal. For example, if we assume a SISO system, where the disturbance is a sinusoidal signal and an objective quadratic function with an absolute maximum, then you can see that multiplying the amplitude of the sinusoid by the increase in objective function, you get another signal where the area represents the sign and value of the gradient of the objective function as a function of the control signal.

<p align="center">
  <img width="450" src="https://github.com/rosasalberto/Control-PV-System/blob/master/images/fig2.PNG">
</p>

  
One way to implement the scheme in Figure 1 is as seen in Figure 3. There is a sinusoidal disturbance applied to the prediction of the control signal, this prediction is calculated by adding the gradient value to the control signal. The gradient is obtained by multiplying the sinusoidal signal by the increase in the objective function. 
The increase in the objective function can be calculated by applying a high pass filter to the objective function output, which filters the continuous component of the objective function output. 
By performing this loop, we achieve the local maxima of the objective function of respect to the control signal.

<p align="center">
  <img width="650" src="https://github.com/rosasalberto/Control-PV-System/blob/master/images/fig3.PNG">
</p>

## Simulation
The control signal is Iref and the variable to control is Pout. A plot is made to see their relationship.

<p align="center">
  <img width="500" src="https://github.com/rosasalberto/Control-PV-System/blob/master/images/fig4.PNG">
</p>

A function with an absolute maximum is obtained.
However for values ​​of Iref < 0 or Iref > Ipo it is seen that the derivative of Iref with respect
Pout is equal to 0. Then the gradient at any point of that domain will be 0 for 2
directions and the control signal will remain stagnant at that point.
Therefore a modification of the objective function is made to solve this problem.
We can implement this algorithm:

    For Pout <0, then → Pout = Pout -sign (Iref) * Iref * 5 - Pmin

<p align="center">
  <img width="500" src="https://github.com/rosasalberto/Control-PV-System/blob/master/images/fig5.PNG">
</p>

The relationship between Iref-Pout has been modified to make an objective function that maximizes the
power and solve the previous problem of gradient 0. 
It is observed that although this objective function is not derivable, the gradient values ​​can be obtained since the algorithm is based on directional increments to draw the gradient.
Our system is implemented in Simulink.

<p align="center">
  <img width="650" src="https://github.com/rosasalberto/Control-PV-System/blob/master/images/fig6.PNG">
</p>

The estimation of Iref is added to a small sinusoidal disturbance signal to obtain the new input to the plant.
We obtain Pout, the value which is introduced into the objective function created above. 
This loop runs indefinitely.
The values ​​of the amplitude and frequency of the disturbance, as well as the gain of the integrator, are hyperparameters to modify depending on the system to control.
In this case, I have chosen an amplitude of 0.05 and a frequency of 5Hz. 

<p align="center">
  <img width="650" src="https://github.com/rosasalberto/Control-PV-System/blob/master/images/fig7.PNG">
</p>

In Figure 7, the algorithm described above is performed. 

    if Pout <0, then → Pout = Pout -sign (Iref) * Iref * 5 - Pmin else Pout = Pout

<p align="center">
  <img width="750" src="https://github.com/rosasalberto/Control-PV-System/blob/master/images/fig8.PNG">
</p>

The plant implementation was made by Professor Antonio Miguel Lopez Martinez.

## Simulation
  
Simulation is performed for time-varying irradiation, and the following results are obtained.

<p align="center">
  <img width="650" src="https://github.com/rosasalberto/Control-PV-System/blob/master/images/fig9.PNG">
</p>

Figure 9 shows that extremum seeking control is a very optimal control for the photovoltaic system. 
The results obtained with the MPPT algorithm are also obtained.

<p align="center">
  <img width="650" src="https://github.com/rosasalberto/Control-PV-System/blob/master/images/fig10.PNG">
</p>

## Conclusion
The extremum seeking control method obtains good results extracting the maximum active power of a photovoltaic system. 
The results seem more optimal than using the MPPT algorithm. 
In addition, this method can be implemented in a continuous real physical system of a photovoltaic plant.
  
